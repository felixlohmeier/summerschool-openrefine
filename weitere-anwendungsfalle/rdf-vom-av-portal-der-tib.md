# RDF-Daten vom AV-Portal der TIB

Das [AV-Portal der TIB Hannover](https://av.tib.eu) stellt wissenschaftliche Videos aus den Bereichen Naturwissenschaft und Technik unter freien Lizenzen bereit. Die Metadaten sind frei verfügbar und können in Bibliothekskataloge integriert werden.

Die Metadaten werden als RDF in den Containern XML, N-Triples und Turtle zum Download angeboten. Neben den üblichen bibliografischen Metadaten enthalten die Metadaten auch extrem umfangreiche Annotationen die durch automatische Analyse aus den Videos extrahiert wurden (OCR und Spracherkennung). Informationen zu den Metadaten: [https://av.tib.eu/opendata](https://av.tib.eu/opendata)

Ausgangsdaten: RDF/N-Triples (Format: .nt)

Ziel: Transformation in TSV mit Spaltenbezeichnungen gemäß [finc-Schema](https://github.com/finc/index/blob/master/schema.xml)

## Vorverarbeitung

OpenRefine kann RDF in XML oder N-Triples laden (Turtle wird nicht unterstützt). Da das Format N-Triples (.nt) etwas platzsparender ist, sollte dieses zum Import verwendet werden. Die Dateigröße in Höhe von 2103 MB (entpackt) ist problematisch und erfordert eine sehr große Menge an Arbeitsspeicher (ca. 45GB) für OpenRefine. Die hier relevanten bibliografischen Angaben haben daran den geringsten Anteil. Leider wird (Stand: 28.7.2017) kein Subset ohne Annotationen angeboten, so dass die benötigten Daten zunächst extrahiert werden müssen.

Ein Import in OpenRefine (Format: RDF/N3) ergibt folgendes Bild:

![Screenshot Daten AV-Portal](images/tib-av-portal-facets.png)

Die benötigten Daten können also anhand des Typs (http://www.w3.ord/1999/02/22-rdf-syntax-ns#type) separiert werden.

Ergebnis:
* Collections: [tib-av-portal-export-1-0-3-nt-collections.tsv](https://felixlohmeier.gitbooks.io/summerschool-openrefine/content/weitere-anwendungsfalle/tib-av-portal-export-1-0-3-nt-collections.tsv)
* Movies: [tib-av-portal-export-1-0-3-nt-movies.tsv](https://felixlohmeier.gitbooks.io/summerschool-openrefine/content/weitere-anwendungsfalle/tib-av-portal-export-1-0-3-nt-movies.tsv)

Achtung: Bei dieser einfachen Extraktion der Daten gehen Daten verloren. Die Gesamtzahl der auf diesem Weg extrahierten Filme (5147) entspricht nicht der Anzahl der Filme im Online-Katalog (vgl. z.B. Zahlen in der [Übersicht nach Publishern](https://av.tib.eu/publishers). Außerdem ist bei Mehrfachbelegungen jeweils nur der erste Wert erhalten geblieben. Die hier extrahierten Daten können also nur Testzwecken dienen. Für eine produktive Verarbeitung müsste die Datenextraktion mit OpenRefine komplexer erfolgen. Alternativ könnte auch mit einem Triple Store gearbeitet werden. Auf der Webseite des AV-Portals sind Beispiele für die Verarbeitung mit Blazegraph beschrieben. Über Select-Statements lassen sich die gewünschten Daten direkt auflösen und anschließend extrahieren, vgl. [https://av.tib.eu/opendata](https://av.tib.eu/opendata).

## Mapping zum finc-Schema

Eine Zuordnung der Testdaten zum [finc-Schema](https://github.com/finc/index/blob/master/schema.xml) kann vorrangig über das Umbenennen der Spalten sowie dem Entfernen von Steuerzeichen erfolgen. Hilfreich ist dazu insbesondere die Funktion ```value.slice(1,-4)```.

Zur Auflösung der URLs auf Ressourcen (z.B. Autor http://av.tib.eu/resource/Lauth__G%C3%BCnter_Jakob) ist es erforderlich aus dem Gesamtabzug der Daten weitere Teilbestandteile zu extrahieren, in ein weiteres OpenRefine-Projekt zu laden und über die Cross-Funktion zu verknüpfen. Es wäre mit OpenRefine auch möglich, die Daten live über eine HTTP-Schnittstelle abzufragen, wenn die TIB diese anbieten würde.

Zwischenstand:
* OpenRefine-Projekt mit Änderungshistorie: [tib-av-portal-finc-schema.tsv](https://felixlohmeier.gitbooks.io/summerschool-openrefine/content/weitere-anwendungsfalle/tib-av-portal-finc-schema.openrefine.tar.gz)
