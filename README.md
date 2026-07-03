# E-commerce Customer Support Automation

Modelová automatizace pro příjem a evidenci zákaznických požadavků v e-commerce.

## Jak automatizace funguje

1. Webhook v Make přijme zákaznický požadavek ve formátu JSON.
2. Router zkontroluje povinná pole a základní formát e-mailu.
3. Platné požadavky se zapíší do listu `Valid Requests`.
4. Neplatné požadavky se přes fallback větev zapíší do listu `Invalid Requests`.
5. Každému požadavku je přiřazen čas přijetí a stav.

## Přenášená data

- jméno zákazníka
- e-mail
- číslo objednávky
- kategorie požadavku
- zpráva
- čas přijetí
- stav požadavku

## Použité technologie

- Make
- Webhooks
- HTTP / JSON
- Google Sheets
- curl

## Validace vstupu

Požadavek je považován za platný, pokud:

- obsahuje jméno,
- obsahuje e-mail,
- e-mail obsahuje znak `@`,
- obsahuje číslo objednávky,
- obsahuje zprávu.

Neplatné požadavky jsou označeny:

- `Error Reason`: Missing or invalid required data
- `Status`: Needs review

## Workflow

![Make workflow](screenshots/make-workflow.png)

## Výsledek

![Google Sheets result](screenshots/google-sheets-result.png)

## Řešený problém

Při prvním testování se čas ukládal v nečitelném ISO formátu. Následně byla upravena funkce pro formátování data a nastaveno české časové pásmo.

{{formatDate(now; "DD.MM.YYYY HH:mm:ss"; "Europe/Prague")}}

Výsledkem je čitelný čas ve formátu:

02.07.2026 16:35:38

Poté mi čas spadal automaticky do UTC, protože jsem použil formát - {{formatDate(now; "DD.MM.YYYY HH:mm:ss "; " Europe/Prague")}} - tady je videt, ze tam jsou dvě mezery navíc, po jejich odstranění mi to bralo správně pražský čas.

## Chybné výsledky

![Timestamp debugging](screenshots/timestamp-debugging.png)

## Update – validace a routing

Automatizace byla rozšířena o router, validaci povinných údajů a fallback větev pro chybné požadavky.

## Testovací vstup

```json
{
  "name": "Jan Novak",
  "email": "jan.novak@example.com",
  "order_number": "ORD-123456",
  "category": "reklamace",
  "message": "Dorucena kniha ma poskozeny obal."
}
