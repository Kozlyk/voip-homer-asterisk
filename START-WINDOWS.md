# VoIP I – Homer a Asterisk na Windows

První etapa projektu připraví Asterisk, dvě testovací linky a číslo pro ověření zvuku.
Na tento základ naváže Homer a pokusy se selháním registrace, nesouladem kodeků a jednosměrným audiem.

## 1. Připrav Docker Desktop

Podle [návodu Dockeru pro Windows](https://docs.docker.com/desktop/setup/install/windows-install/)
nainstaluj Docker Desktop. Použij WSL 2 a linuxové kontejnery. Docker Desktop si spustí vlastní linuxové
prostředí. Potom otevři program Docker Desktop a počkej, až začne běžet. V PowerShellu ověř:

```powershell
docker version
docker compose version
```

První příkaz má ukázat část Client i Server. Pokud ukáže chybu připojení, nejdřív spusť Docker Desktop.

## 2. Soubory vlož do svého místního repozitáře

Pokud máš repozitář zatím jen na GitHubu, nainstaluj [Git pro Windows](https://git-scm.com/install/windows) a otevři nový PowerShell:

```powershell
cd $env:USERPROFILE
git clone https://github.com/Kozlyk/voip-homer-asterisk.git
cd voip-homer-asterisk
git status
explorer .
```

Pokud repozitář už máš místně, přejdi příkazem `cd` do jeho složky a otevři ji pomocí `explorer .`.
Přesuň do něj obsah rozbaleného ZIPu. Ponech stávající `README.md` a `lab/prostredi.md`.
Ve stejné složce jako README musí být `compose.yaml`. Uvnitř `lab/asterisk/` budou čtyři soubory.

## 3. Spusť Asterisk a ověř, že přijímá registrace

Spusť příkazy v PowerShellu ze složky s `compose.yaml`:

```powershell
docker compose config
docker compose up -d --build
docker compose ps
docker compose exec asterisk asterisk -rx "core show version"
docker compose exec asterisk asterisk -rx "pjsip show endpoints"
```

Poslední příkaz má vypsat účty 6001 a 6002. Pokud kontejner neběží, zkontroluj výstup příkazu
`docker compose logs --tail 80 asterisk`.

V SIP klientovi na stejném Windows počítači použij tyto údaje:

| Nastavení | Hodnota |
| --- | --- |
| Server nebo doména | 127.0.0.1 |
| SIP port serveru | 15060 |
| Transport | UDP |
| SIP uživatel a přihlašovací jméno | 6001 |
| Heslo pro místní laboratoř | Homer-Lab-6001 |
| Zvukové kodeky | PCMU nebo PCMA |

Pokud klient vyžaduje server i port v jednom poli, zadej `127.0.0.1:15060`. Pokud dovoluje nastavit lokální SIP port, nech automatický nebo použij jiný než 15060. K testu stačí jeden klient.
Druhý účet používá jméno `6002` a heslo `Homer-Lab-6002`. Druhý klient musí mít jiné lokální porty.

```powershell
docker compose exec asterisk asterisk -rx "pjsip show contacts"
```

Až se objeví kontakt 6001, vytoč v klientovi `7000`. Když mluvíš do mikrofonu, měl bys slyšet vlastní ozvěnu.
Zapiš skutečný výsledek do `lab/prostredi.md`. Pokud se spojení naváže a ozvěna chybí, zaznamenej tento stav a zkontroluj,
jestli klient používá místní připojení, povolený mikrofon a nabídku PCMU/PCMA.

## 4. Ulož skutečný stav na GitHub

Jestli je to první místní commit, nastav v tomto repozitáři jméno a svůj GitHub e-mail nebo adresu `noreply` z GitHub Settings → Emails:

```powershell
git config user.name "Tvoje jméno"
git config user.email "TVUJ_EMAIL_Z_GITHUBU"
```

Potom vyber nové soubory, zkontroluj je a odešli commit:

```powershell
git add -- compose.yaml lab/asterisk START-WINDOWS.md lab/prostredi.md
git diff --cached --stat
git commit -m "Přidat lokální Asterisk přes Docker Compose"
git push
```

Pokud se otevře přihlášení GitHubu, dokonči ho v prohlížeči a ověř změnu na GitHubu.
Až skončíš s testem, zastav laboratoř příkazem `docker compose stop`. Příště ji zapneš přes `docker compose up -d`.

## Struktura dalších etap

- `lab/asterisk/` – konfigurace Asterisku a později nastavení HEP.
- `docs/index.html` – veřejná dokumentace se třemi pokusy a odkazy.
- `docs/dukazy/` – vybrané skutečné výřezy z Homeru a případně RTP.
- `prezentace/` – scénář živé ukázky a podklady k obhajobě.

Tento balíček odpovídá etapě Asterisk. Po ověření registrace a ozvěny je dalším milníkem spuštění Homeru a ověření toku HEP. Testovací hesla jsou určená pro místní poslouchání na 127.0.0.1.
