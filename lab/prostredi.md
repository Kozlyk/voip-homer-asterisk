# Prostředí laboratoře

Projekt spouštím na Windows 10 pomocí Docker Desktopu s WSL 2.
Asterisk 20.6.0 běží v linuxovém kontejneru. Sestavu spouštím
pomocí souboru `compose.yaml`.

## Testovací linky

| Linka | SIP klient | SIP server | Transport |
| --- | --- | --- | --- |
| 6001 | PhonerLite | `127.0.0.1:15060` | UDP |
| 6002 | MicroSIP | `127.0.0.1:15060` | UDP |

Oba SIP klienti běží na stejném počítači jako Docker Desktop.
Číslo `7000` slouží k testu ozvěny.

## Další krok

Spustím Homer a nastavím Asterisk tak, aby do něj posílal kopie SIP zpráv
přes HEP. Nejprve ověřím, že v Homeru vidím registraci a běžný hovor.