# Vorverarbeitung mit MarcEdit und OpenRefine

## Beispieldaten von MARC21 in TSV konvertieren

vgl. Anleitung im vorigen Kapitel

## Daten in OpenRefine laden

* Menü Create Project
* TSV-Datei hochladen
* In den Optionen "store blank rows" deaktivieren

## Subfields aufteilen

Führen Sie folgende Transformationsschritte in OpenRefine durch:

* column Column / Edit column / Remove this column
* column Content / Text filter: $
* column Content / add column based on this column / Subfields / forEach(value.split("$"),v,get(v,0)).join("$")
* column Content / edit cells / transform... / forEach(value.split("$"),v,slice(v,1)).join("$")
* close text filter
* column Subfields / edit cells / split multi-valued cells... / $
* column Content / edit cells / split multi-valued cells... / $

## Records bilden

Führen Sie folgende Transformationsschritte in OpenRefine durch:

column Subfields / Facet / customized facets / Facet by blank / false
column RecordNumber / edit cells / Fill down
column Tags / edit cells / Fill down
column Indicators / edit cells / Fill down
close facet
column RecordNumber / edit cells / Blank down
Show: 5 rows
Show as: records
