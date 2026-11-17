# TeklaPelletCAN — integracja Home Assistant

Integracja Home Assistant (HACS) dla urządzenia [TeklaPelletCAN](https://github.com/GieOeRZet/TeklaPelletCAN)
(własny firmware ESP32-C6 podłączony do magistrali CAN kotła) — odczyt i zapis parametrów kotła pelletowego
Tekla/Estyma po sieci lokalnej. Wykrywa urządzenie automatycznie (zeroconf) albo po podaniu adresu IP.

## Instalacja (HACS)

1. HACS → Integracje → ⋮ → Własne repozytoria → dodaj `https://github.com/GieOeRZet/TeklaPelletCAN_HA`,
   kategoria **Integration**.
2. Zainstaluj, zrestartuj Home Assistant.
3. Ustawienia → Urządzenia i usługi → Dodaj integrację → „TeklaPelletCAN” (zwykle znajdzie się samo przez
   zeroconf, jeśli urządzenie jest w tej samej sieci; w przeciwnym razie podaj adres IP ręcznie).
4. Podaj kod parowania widoczny na stronie WWW urządzenia (lub hasło właściciela urządzenia) — integracja
   dostaje własny token, który nie koliduje z inną instalacją HA korzystającą z tego samego urządzenia.

## Jak to działa (komunikacja)

```
Home Assistant
    │  REST API (odczyt stanów co ~2 s) + WebSocket (push zmian w czasie rzeczywistym)
    ▼
urządzenie TeklaPelletCAN (ESP32-C6, w sieci lokalnej)
    │  CANopen SDO, 125 kbit/s
    ▼
kocioł Tekla/Estyma (węzeł 4)
```

Integracja **nie łączy się z siecią CAN kotła bezpośrednio** — rozmawia wyłącznie z urządzeniem po REST/
WebSocket w sieci lokalnej, a to urządzenie tłumaczy żądania na ramki CANopen. Każdy zapis (zmiana nastawy,
trybu, harmonogramu) wymaga tokenu parowania tej instalacji HA; sam odczyt stanu jest dostępny bez parowania
(konfigurowalne po stronie urządzenia).

## Encje i usługi

Encje mają te same nazwy (`entity_id`) co w oryginalnym projekcie referencyjnym (ESPHome), dla zgodności z kartą
referencyjną — pełna lista w [`rejestry/encje.csv`](https://github.com/GieOeRZet/TeklaPelletCAN/blob/main/rejestry/encje.csv)
repozytorium głównego. Platformy dziś dostarczane przez tę integrację: `sensor`, `binary_sensor`, `select`,
`button`, `switch`.

Usługi (identyczne nazwy akcji co w oryginalnym projekcie, wołane przez kartę jako `<węzeł>_<akcja>`):

| Usługa | Przeznaczenie |
|---|---|
| `unlock` | Odblokowanie poziomu dostępu (PIN klucz 1/2, jak w panelu kotła) |
| `set_setpoint`, `set_boiler_mode` | Zmiana temperatury zadanej / trybu kotła |
| `set_param` | Zapis dowolnej nastawy z katalogu rejestrów |
| `set_schedule` | Zapis harmonogramu CO/CWU |
| `set_output`, `set_output_pct` | Test wyjść (podajnik, dmuchawa, zapalarka itd., poziom dostępu "klucz 2") |
| `test_refresh`, `test_reset`, `alarm_refresh`, `param_refresh`, `schedule_refresh` | Odświeżenie poszczególnych grup danych |
| `sdo_read`, `sdo_scan`, `sdo_scan_stop`, `sdo_write`, `sdo_poll_add`, `sdo_poll_clear` | Skaner rejestrów CANopen — patrz [`rejestry/README.md`](https://github.com/GieOeRZet/TeklaPelletCAN/blob/main/rejestry/README.md) repozytorium głównego ("Skaner rejestrów – instrukcja") |

> Brak natywnej encji `update` (powiadomienie o nowym firmware wprost w Home Assistant) — to wciąż otwarty punkt
> rozwoju integracji; dziś aktualizację robi się przez kartę/portal urządzenia lub aplikację Android.

## Karty do dashboardu

Integracja sama nie zawiera karty Lovelace — zainstaluj osobno jedną (lub obie):
- [TeklaPelletModern-card](https://github.com/GieOeRZet/TeklaPelletModern-card) – styl aplikacji Android, stała wysokość, przewijane ekrany.
- [TeklaPelletClassic-card](https://github.com/GieOeRZet/TeklaPelletClassic-card) – replika oryginalnego panelu dotykowego Igneo Touch.

## Dokumentacja

Pełny opis architektury integracji (pliki, platformy, czego brakuje): `DOKUMENTACJA/INTEGRACJA-HA.md` w
repozytorium [GieOeRZet/TeklaPelletCAN](https://github.com/GieOeRZet/TeklaPelletCAN) — tam powstaje kod
źródłowy, to repozytorium dostaje gotowe wydania dopiero przy publikacji.

## Zastrzeżenia

Projekt niezależny i nieoficjalny, niezwiązany z firmami Tekla ani Estyma. **Nie jest zamiennikiem oryginalnego
sterownika kotła.** Zapis parametrów odbywa się na własne ryzyko i może naruszać warunki gwarancji producenta oraz
prowadzić do uszkodzenia lub nieprawidłowej pracy kotła/sterownika. Licencja: [LICENSE](LICENSE).
