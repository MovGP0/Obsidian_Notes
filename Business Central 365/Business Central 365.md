## Filter

| Zeichen               | Beispiel   | Beschreibung                         |
| --------------------- | ---------- | ------------------------------------ |
| `=`                   | `5`, `rot` | Gleich / enthält                     |
| `<>`                  | `<>5`      | Ungleich                             |
| `..`                  | `3..5`     | Intervall                            |
| `>`                   | `>5`       | Größer als                           |
| `<`                   | `<5`       | Kleiner als                          |
| `|`                   | `3|4`      | Oder                                 |
| `&`                   | `3&4`      | Und                                  |
|                       | `*E*U*`    | Enthält `E` und danach ein `U`       |
| `%Benutzer` / `%User` |            | Aktueller Benutzer                   |
| `%Jetzt` / `%Now`     |            | Heute                                |
| `%Woche` / `%Week`    |            | Woche                                |
| `%Monat` / `%Month`   |            | Monat                                |
| `%Quartal`            |            | Quartal                              |
| `%Jahr` / `%Year`     |            | Jahr                                 |
| `%PeriodeN`           |            | Buchhaltungsperiode Nummer           |
| `%MeineDebitoren`     |            |                                      |
| `%MeineKreditoren`    |            |                                      |
| `%MeineArtikel`       |            |                                      |
| `%Ich` / `%Me`        |            | Alias für `%Benutzer`                |
| `%Mandant`            |            | Filter nach aktuellen Mandantennamen |

## Felder

- Zahlenfelder können einfache Grundrechnungsarten
- bei Datumsfeldern kann Trennzeichen entfallen
    - wenn Monat/Jahr nicht angegeben wird, wird das aktuelle Jahr/Monat angenommen
    - `a` ist aktueller Tag
    - `p1` erstes Monat des Jahres
    - `q1` erstes Quartal des Jahres
- Toogles können mit Leertaste gesetzt werden
- Drop-Down-Felder mit `alt+down` auswählen

## Programming

- [[Application Language (AL)]]