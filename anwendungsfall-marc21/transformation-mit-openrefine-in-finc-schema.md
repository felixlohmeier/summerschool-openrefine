# Transformation mit OpenRefine in finc-Schema

Ziel: Daten für den Import in den Suchindex vorbereiten

MARC21 ist sehr komplex und das [finc-Schema](https://github.com/finc/index/blob/master/schema.xml) hat ebenfalls etliche Felder, die teilweise kompliziert zu bilden sind. In dieser Summerschool können wir daher nur einen Teil erproben.

## Neue Spalte anlegen für Definition der Felder (gemäß finc Schema)

* column Subfields / Edit column / Add column based on this column... / Column Name: finc / Expression: ""

## Felder definieren

Arbeitstabelle \(in Summerschool erstellt\): [wiley-mapping.xls](https://felixlohmeier.gitbooks.io/summerschool-openrefine/content/anwendungsfall-marc21/wiley-mapping.xls)

Die Zuordnung der MARC-Daten zum finc-Schema kann in der Regel in einer drei folgenden Varianten erfolgen.

### Variante A: Felder direkt aus einem MARC-Tag/Subfield definieren

Beispiel für author = MARC 100

* show as: rows
* column Tags / Facet / text facet / "100" auswählen
* column Subfields / Facet / text facet / "a" auswählen
* column Indicators / Facet / text facet / "1\" auswählen
* column finc / edit cells / transform... / "author"
* close facets

Beispiel für id = MARC 001

* column Tags / Facet / text facet / "001" auswählen
* column finc / edit cells / transform... / "id"
* close facets

## Variante B: Felder aus mehreren MARC-Tags/Subfields zusammensetzen 

Wenn Felder sich aus mehreren Tag-Subfield-Indicator-Kombinationen zusammensetzen sollen, ist es am einfachsten, wenn die Teilbestandteile zunächst einzeln definiert werden. Beispiel für Titel = MARC 245

* column Tags / Facet / text facet / "245" auswählen
* column Subfields / Facet / text facet / "a" auswählen
* column Indicators / Facet / text facet / "00", "02" und "04" auswählen
* column finc / edit cells / transform... / "title1"
* close facets
* column Tags / Facet / text facet / "245" auswählen
* column Subfields / Facet / text facet / "a" auswählen
* column Indicators / Facet / text facet / "10", "12", "13" und "14" auswählen
* column finc / edit cells / transform... / "title2"
* close facets
* column Tags / Facet / text facet / "245" auswählen
* column Subfields / Facet / text facet / "b" auswählen
* column Indicators / Facet / text facet / "00" und "04" auswählen
* column finc / edit cells / transform... / "title3"
* close facets
* column Tags / Facet / text facet / "245" auswählen
* column Subfields / Facet / text facet / "b" auswählen
* column Indicators / Facet / text facet / "10", "12", "13" und "14" auswählen
* column finc / edit cells / transform... / "title4"
* close facets

## Variante C: Zusätzliche Felder definieren

Um zusätzliche Felder mit Werten zu belegen, die in den Ausgangsdaten nicht vorkommen, müssen zunächst mit einem Trick neue Zeilen eingefügt werden. Beispiel für die Facette "Zugang" im SLUB-Katalog.

* column Tags / Facet / text facet / "LDR" auswählen
* column Content / edit cells / transform... / Expression: value + "NEUEZEILE"
* column Content / edit cells / Split multi-valued cells / Separator: NEUEZEILE
* close facet
* column Tags / Facet / Customized facets / facet by blank / true
* column finc / edit cells / transform... / "access_facet"
* column content / edit cells / transform... / "Electronic Resources"
* close facet

## Transponieren

Nachdem alle Felder (bzw. in Variante B deren Teilbestandteile) definiert wurden, müssen die Gesamtdaten transponiert werden, um die für den Import in den Suchindex benötigte Struktur zu erhalten.

* All / Edit columns / Re-order / remove columns ... / Spalten "RecordNumber", "Tags", "Indicators", "Subfields" nach rechts bewegen \(d.h. löschen\)
* column finc / Facet / Customized facets / Facet by blank / true
* All / Edit rows / Remove all matching rows
* column finc / Edit cells / Blank down
* column Content / edit cells / join multi-valued cells / $
* column finc / Facet / Customized facets / Facet by blank / true
* All / Edit rows / Remove all matching rows
* close facet
* column finc / transpose / columnize by key/value columns ... / key column: finc; value column: Content
* Spalten manuell sortieren: All / Edit columns / Re-order / remove columns ... / alphabetisch sortieren, Ausnahme: Spalte "id" soll als erstes stehen

## Teilbestandteile zusammenfassen

Beispiel für Titel

* column title1 / Edit column / Add column based on this column... / Column Name: title / forNonBlank(cells["title1"].value,v,v,"") + forNonBlank(cells["title2"].value,v," "+v,"") + forNonBlank(cells["title3"].value,v," "+v,"") + forNonBlank(cells["title4"].value,v," "+v,"")
* column title / edit cells / transform... / value.trim().slice(0,-1)
* All / Edit columns / Re-order / remove columns ... / Spalten "title1", "title2", "title3", "title4" nach rechts bewegen \(d.h. löschen\)

## Optional: Transformationsschritte als JSON-Konfiguration

* Alle Transformationsschritte oben als JSON-Konfiguration: [wiley-minimal.json](https://felixlohmeier.gitbooks.io/summerschool-openrefine/content/anwendungsfall-marc21/wiley-minimal.json)

## Export

Wählen Sie oben rechts im Menü Export den Menüpunkt `Tab-separated-value`

## Zwischenstand aus der Summerschool mit weiteren Transformationsschritten

* OpenRefine-Projekt mit Änderungshistorie: [wiley.openrefine.tar.gz](https://www.felixlohmeier.de/slub/wiley/wiley.openrefine.tar.gz) (aus Lizenzgründen zugriffsgeschützt)

