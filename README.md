# TeklaPelletCAN – integracja Home Assistant

Integracja Home Assistant (HACS) dla urządzenia TeklaPelletCAN – odczyt i zapis parametrów kotła pelletowego
Tekla/Estyma po sieci lokalnej. Wykrywa urządzenie automatycznie (zeroconf) albo po podaniu adresu IP.

Ten plik jest publikowany automatycznie z głównego repozytorium rozwojowego:
[GieOeRZet/TeklaPelletCAN](https://github.com/GieOeRZet/TeklaPelletCAN) — tam też zgłaszaj
problemy/propozycje i tam trwa rozwój.

## Instalacja (HACS)

1. HACS → Integracje → ⋮ → Własne repozytoria → dodaj `https://github.com/GieOeRZet/TeklaPelletCAN_HA`, kategoria **Integration**.
2. Zainstaluj, zrestartuj Home Assistant.
3. Ustawienia → Urządzenia i usługi → Dodaj integrację → „TeklaPelletCAN” (zwykle znajdzie się samo przez zeroconf,
   jeśli urządzenie jest w tej samej sieci; w przeciwnym razie podaj adres IP ręcznie).

## Karty do dashboardu

Integracja sama nie zawiera karty Lovelace — zainstaluj osobno jedną (lub obie):
- [TeklaPelletModern-card](https://github.com/GieOeRZet/TeklaPelletModern-card) – styl aplikacji Android, stała wysokość, przewijane ekrany.
- [TeklaPelletClassic-card](https://github.com/GieOeRZet/TeklaPelletClassic-card) – replika oryginalnego panelu dotykowego Igneo Touch.

## Zastrzeżenia

Projekt niezależny i nieoficjalny, niezwiązany z firmami Tekla ani Estyma. **Nie jest zamiennikiem oryginalnego
sterownika kotła.** Zapis parametrów odbywa się na własne ryzyko i może naruszać warunki gwarancji producenta oraz
prowadzić do uszkodzenia lub nieprawidłowej pracy kotła/sterownika. Licencja: [LICENSE](LICENSE).
