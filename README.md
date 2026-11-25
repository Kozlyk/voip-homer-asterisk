# Homer a Asterisk (HEP) – diagnostika SIP

**Autor:** Ladislav Kozel  
**Předmět:** VoIP I, VŠB–TU Ostrava  
**Téma:** 4 – Homer a Asterisk (HEP)

## Plán testů

Asterisk bude posílat kopie SIP zpráv přes HEP do Homeru. Pro každý
scénář porovnám výchozí stav, stav při poruše a stav po opravě.

| Scénář | Co budu sledovat |
| --- | --- |
| Selhání registrace | Průběh zpráv `REGISTER` při zadání chybného a správného hesla. |
| Nesoulad kodeků | Nabídku kodeků v SDP a výslednou SIP odpověď před opravou a po ní. |
| Jednosměrné audio | Navázání hovoru v Homeru a záznam o tom, ve kterém směru chybí zvuk. |

## Spuštění Asterisku

Na Windows jsem použil Docker Desktop s WSL 2. Při prvním spuštění
jsem musel ve Windows zapnout součásti **Virtual Machine Platform**
a **Windows Subsystem for Linux**. Podrobný postup je v
[návodu pro Windows](START-WINDOWS.md).

Se spuštěným Docker Desktopem otevři PowerShell ve složce repozitáře
a spusť:

```powershell
docker compose up -d --build
docker compose exec asterisk asterisk -rx "core show version"
docker compose exec asterisk asterisk -rx "pjsip show endpoints"
```

První příkaz sestaví a spustí kontejner. Další dva ověří verzi
Asterisku a načtení linek `6001` a `6002`.

## Ruční SIP ukázka

Oba klienty připojím přes UDP k `127.0.0.1:15060`. Linku `6001`
používám v **PhonerLite** a linku `6002` v **MicroSIPu**. Nastavení
účtů je v [návodu pro Windows](START-WINDOWS.md).

Přihlášené linky ověřím příkazem:

```powershell
docker compose exec asterisk asterisk -rx "pjsip show contacts"
```

Pro test ozvěny slouží číslo `7000`. Hovor mezi dvěma linkami vyzkouším
tak, že v PhonerLite vytočím `6002` a přijmu hovor v MicroSIPu.
První hovor se úspěšně spojil. Podrobnosti jsou v
[zápisu z prvního hovoru](lab/baseline.md).

Po ukázce laboratoř zastavím příkazem:

```powershell
docker compose stop
```

## Dokumentace

- [Prostředí laboratoře](lab/prostredi.md)
- [Návod ke spuštění na Windows](START-WINDOWS.md)
- [Zápis z prvního hovoru](lab/baseline.md)

Po zprovoznění Homeru doplním postup nastavení HEP. Odkaz na web
s důkazy a záložním videem přidám po zveřejnění.
