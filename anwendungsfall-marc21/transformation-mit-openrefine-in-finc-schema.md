# Transformation mit OpenRefine in finc-Schema

Ziel: Daten für den Import in den Suchindex vorbereiten

MARC21 ist sehr komplex und das [finc-Schema](https://github.com/finc/index/blob/master/schema.xml) hat ebenfalls etliche Felder, die teilweise kompliziert zu bilden sind. In dieser Summerschool können wir daher nur einen Teil erproben.

## Werte belegen \(am Beispiel von Feldern 001 und 245\)

Arbeitstabelle \(in Summerschool erstellt\): [openrefine/wiley.xls](/openrefine/wiley.xls)

* show as: rows
* column Tags / Facet / text facet / 245
* column Subfields / Facet / text facet / "a", "b", "c"
* column Tags / edit cells / transform... / "title"
* close facets
* column Tags / Facet / text facet / 001
* column Tags / edit cells / transform... / "id"
* close facet
* ...

## Transponieren

* All / Edit columns / Re-order / remove columns / Spalten "RecordNumber", "Indicators", "Subfields2" nach rechts bewegen \(d.h. löschen\)
* column Tags / Facet / text facet / include: "id", "title", "..." / invert
* All / Edit rows / Remove all matching rows
* close facet
* column Tags / edit cells / Blank down
* column Content / edit cells / join multi-valued cells / $
* column Content / Facet / customized facets / Facet by blank / true
* All / Edit rows / Remove all matching rows
* close facet
* Column key / transpose / columnize by key/value columns / ok

## Export

Wählen Sie oben rechts im Menü Export den Menüpunkt `Tab-separated-value`

