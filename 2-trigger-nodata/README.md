# Temat 2: Zaawansowany Trigger z nodata (Zabbix 7)

Instrukcja konfiguracji bezpiecznego triggera na hoście `Aggregated_PDU`. Trigger sprawdza, czy oba hosty fizyczne (`hostid1` i `hostid2`) przesyłają dane, zanim sprawdzi warunek przekroczenia 4.5 kW (4500 W). Zapobiega to fałszywym alarmom w przypadku awarii sieciowej jednego z PDU.

---

## KROK 1: Konfiguracja Triggera

1. Przejdź do: **Data collection** -> **Hosts**.
2. Przy hoście `Aggregated_PDU` kliknij odnośnik **Triggers**.
3. W prawym górnym rogu kliknij **Create trigger**.
4. Uzupełnij pola w formularzu:
   - **Name**: `Suma mocy hostid1 + hostid2 > 4.5 kW`
   - **Severity**: Wybierz `High` (lub według własnych standardów)
   - **Expression**: Wklej dokładnie poniższą formułę:
     ```text
     (nodata(/hostid1/mspdu.phase1.power,5m)=0 and nodata(/hostid2/mspdu.phase1.power,5m)=0) and (last(/hostid1/mspdu.phase1.power)+last(/hostid2/mspdu.phase1.power)>4500)
     ```
   - **OK event generation**: `Recovery expression`
   - **Recovery expression**: Wklej poniższą formułę (histereza - powrót do normy poniżej 4300 W):
     ```text
     (last(/hostid1/mspdu.phase1.power)+last(/hostid2/mspdu.phase1.power))<4300
     ```
5. Kliknij przycisk **Add** na dole strony.

---

## Jak działa to wyrażenie?

1. Sekcja `nodata(..., 5m) = 0` pilnuje, aby alarm nie aktywował się, jeśli Zabbix nie otrzymał nowych danych z któregokolwiek urządzenia przez ostatnie 5 minut.
2. Sekcja `last(...) + last(...) > 4500` odpowiada za właściwy próg alarmowy (4.5 kW).
3. Sekcja `Recovery expression` sprawia, że alarm zamknie się dopiero wtedy, gdy suma poboru mocy spadnie poniżej bezpiecznych 4.3 kW, co zapobiega ciągłemu otwieraniu i zamykaniu się alarmu (tzw. flapping) na granicy 4.5 kW.
