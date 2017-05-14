# Verarbeitung von bibliothekarischen Metadaten

Ein möglicher Weg zum Laden von Metadaten in den Suchindex Solr ist die Transformation der bibliothekarischen Metadaten in eine klassische Tabellenstruktur:
* Spaltenüberschriften wie Felder im Suchindex
* Mehrfachbelegung mit Trennzeichen
* Export in CSV/TSV

OpenRefine ist ein generisches Werkzeug und kennt den MARC-Standard nicht. MARCXML kann allgemein als XML geladen werden, aber binäres MARC-Format kann OpenRefine nicht laden. Daher ist es sinnvoll für die Verarbeitung von Daten im Format MARC21 die zusätzliche Software MarcEdit zu verwenden.
