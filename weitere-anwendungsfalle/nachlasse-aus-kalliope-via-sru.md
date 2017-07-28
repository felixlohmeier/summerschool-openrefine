# Nachlässe aus Kalliope via SRU

In Kalliope sind Nachlässe katalogisiert, die auch im SLUB-Katalog angezeigt werden sollen. Hierzu sollen die Metadaten mittels der von Kalliope bereitgestellten SRU-Schnittstelle prozessiert werden.

Ausgangsdaten: MODS/XML über SRU-Schnittstelle
Ziel: Transformation in TSV mit Spaltenbezeichnungen gemäß [finc-Schema](https://github.com/finc/index/blob/master/schema.xml)

## Abfrage über die SRU-Schnittstelle

Die URL zur Abfrage der SRU-Schnittstelle lautet:
```
http://kalliope-verbund.info/sru?version=1.2&operation=searchRetrieve&query=ead.genre="Nachlass"+AND+ead.repository.isil="DE-14"&recordSchema=mods
```

Die Software MarcEdit bietet mit dem Tool "Z39.50/SRU Client" eine Möglichkeit Daten von SRU-Schnittstellen herunterzuladen.

Schritt 1: Datenbank anlegen
* Modify Databases / Add Database / Add SRU Server
* Name: Kalliope
* URL: http://kalliope-verbund.info/sru
* Metadata Schema: MODS
* Search Profile: Custom

Schritt 2: Records abfagen
* Search Mode
* Search: ead.genre="Nachlass"+AND+ead.repository.isil="DE-14"
* Im Pulldown statt "Title" den Wert "Raw (Adv.)" auswählen
* startRecord: 0
* maximumRecords: 1000
* grünen Pfeil drücken
* Rechte Maustaste in Trefferliste und "Download all Records wählen"

Schritt 3: Die von MarcEdit produzierte XML-Datei enthält alle Records ohne umrahmendes XML-Element. Das ist nicht standardkonform und kann von OpenRefine daher nicht korrekt gelesen werden. Damit der Import klappt, muss die Datei manuell bearbeitet werden:
* Zu Beginn des Dokuments einfügen: ```<records>``` (der Name des Elements ist beliebig)
* Am Ende des Dokuments einfügen: ```</records>```

## Hinweise zur Verarbeitung in OpenRefine

Import:
* Beim Import der XML-Daten auf das Element <mods ...> klicken

Leere Zeilen identifizieren:
* Auf beliebiger Spalte Facet / custom text facet... / isBlank(forEach(row.columnNames,cn,if(isNull(cells[cn]),"",cells[cn].value.trim())).join("")) / true
* All / Edit rows / Remove all matching rows

Folgende Spalten entstammen der Mods-Struktur, sind (weitgehend) inhaltsleer und können gelöscht werden:
* mods
* mods - version
* mods - xsi:schemaLocation
* mods - recordInfo
* mods - recordInfo - recordCreationDate - encoding
* mods - recordInfo - recordContentSource - authority
* mods - recordInfo - languageOfCataloging
* mods - recordInfo - languageOfCataloging - languageTerm
* mods - recordInfo - languageOfCataloging - languageTerm - type
* mods - recordInfo - languageOfCataloging - languageTerm - authority
* mods - name
* mods - name - role
* mods - location
* mods - titleInfo
* mods - physicalDescription
* mods - relatedItem
* mods - relatedItem - titleInfo
* mods - originInfo

Platzhalter einfügen:
* Beim Zusammenfassen von Zeilen zu einem Record ist zu beachten, dass zwischen manchen Spalten wichtige Beziehungen bestehen, wie beispielsweise zwischen Person (mods - name - namePart) und GND-ID (mods - name - valueURI). Wenn nicht allen Personen eine ID zugeordnet ist, könnte beim Zusammenfassen die Zuordnung durcheinandergeraten. In diesem Fall ist es sinnvoll vorher Platzhalter einzufügen.
* Als Trennzeichen ```␞``` verwenden (das Trennzeichen [Record Separator ␟](https://unicode-table.com/en/241E/) aus dem Unicode-Zeichensatz kommt mit Sicherheit nicht in den Daten vor, daher ist dieses gut geeignet. Das Zeichen ist am einfachsten per copy & paste einzufügen).
* mods - name - namePart / Facet / Customized facets / Facet by blank / false
* mods - name - valueURI / Facet / Customized facets / Facet by blank / true
* mods - name - valueURI / Edit cells / Transform... / "␞"

Mehrfachbelegungen zusammenfassen:
* Spalte auswählen / Edit cells / Join multi-valued cells...
* Als Trennzeichen ```␟``` angeben (das Trennzeichen [Unit Separator ␟](http://unicode-table.com/en/241F/) aus dem Unicode-Zeichensatz kommt mit Sicherheit nicht in den Daten vor, daher ist dieses gut geeignet. Das Zeichen ist am einfachsten per copy & paste einzufügen).
* mods - location - url / Edit cells / Join multi-valued cells... / ␟
* mods - name - namePart / Edit cells / Join multi-valued cells... / ␟
* mods - name - valueURI / Edit cells / Join multi-valued cells... / ␟

Überflüssige Zeilen löschen:
* mods - recordInfo - recordIdentifier / Facet / Customized facets / Facet by blank / true
* All / Edit rows / Remove all matching rows

Zwischenstand:
* OpenRefine-Projekt mit Änderungshistorie: [kalliope-slub-nachlaesse-2017-07-28.openrefine.tar.gz](https://www.felixlohmeier.de/slub/kalliope/kalliope-slub-nachlaesse-2017-07-28.openrefine.tar.gz) (aus Lizenzgründen zugriffsgeschützt)

## Mapping zum finc-Schema

Eine Zuordnung der Daten zum [finc-Schema](https://github.com/finc/index/blob/master/schema.xml) kann nun vorrangig über das Umbenennen der Spalten sowie dem Entfernen von Steuerzeichen erfolgen.
