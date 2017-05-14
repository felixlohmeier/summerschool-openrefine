# TSV-Dateien in Solr laden

## Konfiguration neu einlesen

* Menü "Core Admin" aufrufen: http://localhost:8983/solr/#/~cores/gettingstarted
* Button "Reload" drücken

## Index leeren (im Terminal)

Der folgende Befehl löscht alle Daten im Index ```gettingstarted```

```
curl "http://localhost:8983/solr/gettingstarted/update?commit=true&stream.body=%3Cdelete%3E%3Cquery%3E*%3A*%3C/query%3E%3C/delete%3E"
```

## Daten laden (im Terminal)

Der folgende Befehl indexiert die Daten aus der Datei ```test.tsv```. Der Befehl ist so lang, weil Solr mitgeteilt werden muss, welche Felder mehrfachbelegt sind und mit welchem Zeichen diese getrennt sind.

```
curl "http://localhost:8983/solr/gettingstarted/update/csv?commit=true&separator=%09&f.title.split=true&f.title.separator=%E2%90%9F" --data-binary @test.tsv -H 'Content-type:text/plain; charset=utf-8'
```

Weitere mehrfachbelegte Felder ergänzen Sie, indem Sie vor dem zweiten Anführungszeichen einen weiteren Teil wie folgt anfügen:
```
&f.year.split=true&f.year.separator=%E2%90%9F 
```

Beispiel:
```
curl "http://localhost:8983/solr/gettingstarted/update/csv?commit=true&separator=%09&f.title.split=true&f.title.separator=%E2%90%9F&f.year.split=true&f.year.separator=%E2%90%9F" --data-binary @test.tsv -H 'Content-type:text/plain; charset=utf-8'
```


## Prüfen Sie das Ergebnis

Rufen Sie die Browsing-Oberfläche auf (http://localhost:8983/solr/gettingstarted/browse). Machen Sie ein paar Beispielsuchen, um sicherzugehen, dass die Daten richtig indexiert wurden.

## Solr beenden und starten

Solr wurde als Prozess gestartet, der bis zum nächsten Neustart des Rechners weiterläuft. Sie können Solr jederzeit manuell beenden und starten:

* Solr beenden:```~/solr-6.5.0/bin/solr stop```
* Solr starten:```~/solr-6.5.0/bin/solr start -s ~/solr-6.5.0/example/schemaless/solr```

Etwa 15-30 Sekunden nach dem Startbefehl sollte die Administrations- und die Browsingoberfläche unter den gewohnten Adressen erreichbar sein.

## Literatur

* [Offizielle Anleitung zum Einspielen von CSV-Daten](https://wiki.apache.org/solr/UpdateCSV#Updating_a_Solr_Index_with_CSV)
