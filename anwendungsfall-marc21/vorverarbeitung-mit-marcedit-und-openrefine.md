# Vorverarbeitung mit MarcEdit und OpenRefine

Ausgangsdaten: MARC21-Daten im Binärformat: [2016-2017_Wiley_UBCM_Auswahl-Kauf.mrc](https://www.felixlohmeier.de/slub/wiley/2016-2017_Wiley_UBCM_Auswahl-Kauf.mrc) (aus Lizenzgründen zugriffsgeschützt)

## Daten mit MarcEdit von MARC21 in TSV konvertieren

Starten Sie MarcEdit, öffnen Sie den Bildschirm "OpenRefine Data Transfer" und geben Sie die folgenden Daten in die Maske ein:

* Source File: Ausgangsdatei im MARC21-Format auswählen
* Save File: Ordner auswählen, Dateiname vergeben und bei "save as type" Tabbed Delimited Files (*.tsv) auswählen
* Export to OpenRefine auswählen und Button "Process" drücken

Ergebnis: [wiley-marcedit-export.tsv](https://www.felixlohmeier.de/slub/wiley/wiley-marcedit-export.tsv) (aus Lizenzgründen zugriffsgeschützt)

Achtung: MarcEdit ersetzt Dollarzeichen im Inhalt durch ```{dollar}```, damit das Dollarzeichen eindeutig als Steuerzeichen erkannt werden kann.

## Daten in OpenRefine laden

* Menü Create Project
* Im vorigen Schritt mit MarcEdit erstellte TSV-Datei hochladen
* In den Optionen "store blank rows" deaktivieren

## Subfields aufteilen

Führen Sie folgende Transformationsschritte in OpenRefine durch:

* column Column / Edit column / Remove this column
* column Content / Text filter: $
* All / Edit rows / Star rows
* column Content / Edit cells / Transform... / value.slice(1)
* close text filter
* column Content / edit cells / split multi-valued cells... / $
* column RecordNumber / Facet / Customized facets / Facet by blank / true
* All / Edit rows / Star rows
* close facet
* All / Facet / Facet by star / true
* column Content / add column based on this column / Subfields / value.get(0)
* column Content / Edit cells / Transform... / value.slice(1)
* close facet

## Records bilden

Führen Sie folgende Transformationsschritte in OpenRefine durch:

* All / Facet / Facet by star / true
* column RecordNumber / edit cells / Fill down
* column Tags / edit cells / Fill down
* column Indicators / edit cells / Fill down
* close facet
* All / Edit rows / Unstar rows
* column RecordNumber / edit cells / Blank down
* Show: 5 rows
* Show as: records

## Optional: Transformationsschritte als JSON-Konfiguration

* Alle Transformationsschritte oben als JSON-Konfiguration: [marc-vorverarbeitung.json](https://felixlohmeier.gitbooks.io/summerschool-openrefine/content/anwendungsfall-marc21/marc-vorverarbeitung.json)
