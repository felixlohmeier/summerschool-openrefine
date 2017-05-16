# Transformation mit OpenRefine in finc-Schema

Ziel: Daten für den Import in den Suchindex vorbereiten

MARC21 ist sehr komplex und das [finc-Schema](https://github.com/finc/index/blob/master/schema.xml) hat ebenfalls etliche Felder, die teilweise kompliziert zu bilden sind. In dieser Summerschool können wir daher nur einen Teil erproben.

Arbeitstabelle \(in Summerschool erstellt\): [openrefine/wiley.xls](/openrefine/wiley.xls)

## Felder definieren

Neue Spalte anlegen:

* column Subfields / Edit column / Add column based on this column... / Column Name: finc / Expression: ""

Felder definieren \(Beispiel für Titel = MARC 245\):

* show as: rows
* column Tags / Facet / text facet / "245" auswählen
* column Subfields / Facet / text facet / "a" und "b" auswählen
* column Indicators / Facet / text facet / "00", "02" und "04" auswählen
* column finc / edit cells / transform... / "title"
* close facets

Neue Zeile einfügen \(ist in OpenRefine nur mit einem Trick möglich\):

* Die Information in MARC LDR wird nicht benötigt. Wir können diese Zeile für den "Trick" benutzen.
* column Tags / Facet / text facet / "LDR" auswählen
* column Content / edit cells / transform... / Expression: "NEUEZEILE"
* column Content / edit cells / Split multi-valued cells / Separator: NEUEZEILE
* close facet
* Die neue Zeile kann nun über facet by blank ausgewählt werden: column Tags / Facet / Customized facets / facet by blank / true

## Transponieren

* All / Edit columns / Re-order / remove columns ... / Spalten "RecordNumber", "Tags", "Indicators", "Subfields" nach rechts bewegen \(d.h. löschen\)
* column finc / Facet / Customized facets / Facet by blank / true
* All / Edit rows / Remove all matching rows
* column finc / Edit cells / Blank down
* column Content / edit cells / join multi-valued cells / $
* column finc / Facet / Customized facets / Facet by blank / true
* All / Edit rows / Remove all matching rows
* close facet
* column finc / transpose / columnize by key/value columns ... / ok
* Bei Bedarf Spalten manuell alphabetisch sortieren: All / Edit columns / Re-order / remove columns ... /

## Export

Wählen Sie oben rechts im Menü Export den Menüpunkt `Tab-separated-value`

