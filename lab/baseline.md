# Výchozí stav před testováním poruch

Cílem bylo ověřit, že se obě testovací linky přihlásí k Asterisku
a že mezi nimi lze navázat hovor.

## Postup

1. Přihlásil jsem linku `6001` v aplikaci PhonerLite.
2. Přihlásil jsem linku `6002` v aplikaci MicroSIP.
3. V PhonerLite jsem vytočil `6002` a v MicroSIPu hovor přijal.

## Výsledek

- Hovor z `6001` na `6002` se úspěšně spojil.
- Zvuk z PhonerLite do MicroSIPu (`6001` → `6002`): zatím neověřeno.
- Zvuk z MicroSIPu do PhonerLite (`6002` → `6001`): zatím neověřeno.