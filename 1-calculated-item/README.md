# Temat 1: Tworzenie Wirtualnego Hosta i Calculated Item (Zabbix 7)

Instrukcja pozwala na utworzenie wirtualnego hosta `Aggregated_PDU` oraz elementu obliczeniowego, który sumuje pobór mocy z `hostid1` i `hostid2`.

---

## KROK 1: Utworzenie wirtualnego hosta

1. Zaloguj się do Zabbix 7.
2. Przejdź do: **Data collection** -> **Hosts**.
3. W prawym górnym rogu kliknij przycisk **Create host**.
4. Uzupełnij zakładkę **Host**:
   - **Host name**: `Aggregated_PDU`
   - **Templates**: (zostaw puste)
   - **Host groups**: Wybierz istniejącą grupę lub wpisz nową, np. `Virtual Devices` i kliknij *Add as new host group*.
   - **Interfaces**: (nie dodawaj żadnego interfejsu - host wirtualny go nie potrzebuje).
5. Kliknij przycisk **Add** na dole strony.

---

## KROK 2: Utworzenie Calculated Item (Suma mocy)

1. Na liście hostów (**Data collection** -> **Hosts**) znajdź nowo utworzony host `Aggregated_PDU`.
2. W jego wierszu kliknij odnośnik **Items**.
3. W prawym górnym rogu kliknij **Create item**.
4. Uzupełnij pola w formularzu:
   - **Name**: `Suma mocy PDU A+B`
   - **Type**: Wybierz `Calculated`
   - **Key**: `power.sum.ab`
   - **Type of information**: `Numeric (float)`
   - **Formula**: Wklej poniższe wyrażenie:
     ```text
     last(/hostid1/mspdu.phase1.power)+last(/hostid2/mspdu.phase1.power)
     ```
   - **Units**: `W`
   - **Update interval**: `1m` (lub dopasuj do interwału odświeżania fizycznych urządzeń)
   - **History storage period**: `7d` (lub według uznania)
   - **Trend storage period**: `365d`
5. Kliknij przycisk **Add** na dole strony.

---

## Weryfikacja działania
Przejdź do **Monitoring** -> **Latest data**, wybierz host `Aggregated_PDU` i sprawdź, czy wartość sumy jest poprawnie obliczana i wyświetlana.
