# Metadaten eines Anbieters im CSV-Format

Ausgangsdaten: CSV-Format des Anbieters "Lynda"
Ziel: Transformation in TSV mit Spaltenbezeichnungen gemäß [finc-Schema](https://github.com/finc/index/blob/master/schema.xml)

## Hinweise zur Verarbeitung in OpenRefine

* Für kleinere Korrekturen bietet sich die Transformation ```value.replace("vorher","nachher")``` an. Es ist besser eine Transformation auf einer Spalte anzuwenden als Daten manuell zu editieren, weil Transformationen in der Änderungshistorie gespeichert werden und auf neue Datenlieferungen erneut angewendet werden können.

## Mapping zum finc-Schema

Eine Zuordnung der Testdaten zum [finc-Schema](https://github.com/finc/index/blob/master/schema.xml) kann vorrangig über das Umbenennen der Spalten sowie dem Entfernen von Steuerzeichen erfolgen.

Zwischenstand:
* OpenRefine-Projekt mit Änderungshistorie: [lynda-com-Courses-csv.openrefine.tar.gz](https://www.felixlohmeier.de/slub/lynda/lynda-com-Courses-csv.openrefine.tar.gz) (aus Lizenzgründen zugriffsgeschützt)
