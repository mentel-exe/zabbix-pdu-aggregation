# Zabbix 7 PDU Aggregation & Advanced Triggering

Repozytorium zawiera kompletne instrukcje wdrożenia agregacji danych z dwóch fizycznych hostów PDU (`hostid1` oraz `hostid2`) w systemie **Zabbix 7**.

## Spis treści
1. [Temat 1: Wirtualny Host i Calculated Item](./1-calculated-item/README.md) - Konfiguracja wirtualnego obiektu i wyliczanie sumy mocy.
2. [Temat 2: Zaawansowany Trigger z nodata](./2-trigger-nodata/README.md) - Konfiguracja bezpiecznego alarmu sprawdzającego dostępność danych.

## Wymagania wstępne
- Zabbix Server w wersji **7.0** lub nowszej.
- Dwa skonfigurowane hosty fizyczne o nazwach w Zabbix:
  - `hostid1` (z itemem o kluczu `mspdu.phase1.power`)
  - `hostid2` (z itemem o kluczu `mspdu.phase1.power`)
