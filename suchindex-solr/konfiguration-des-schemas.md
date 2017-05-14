# Konfiguration des Solr Schemas

Ab Solr Version 6.0 ist das sogenannte "managed schema" (auch "schemaless mode" genannt) voreingestellt. Solr analysiert bei der Indexierung die Daten und versucht das Schema selbst zu generieren. Felder können aber weiterhin zusätzlich manuell definiert werden.

## Schema über Admin-Oberfläche konfigurieren

* Admin-Oberfläche aufrufen. Im Menü "Core Selector" den Index "gettingstarted" auswählen. Dann im zweiten Menü "Schema" aufrufen. Direktlink: http://localhost:8983/solr/#/gettingstarted/schema
* Button "Add Field" drücken
* Name eingeben (Groß- und Kleinschreibung ist wichtig)
* Einen ```field type``` (z.B. string) auswählen
* ggf. ```multivalued``` markieren.

## Literatur

* [Einführungsartikel zu "Managed Schema"](https://support.lucidworks.com/hc/en-us/articles/221618187-What-is-Managed-Schema-)
* [Einführungsartikel zur Definition von Feldern im Schema](http://www.solrtutorial.com/schema-xml.html)
