# Installation von MarcEdit

## MarcEdit herunterladen und entpacken

```
wget http://marcedit.reeset.net/software/marcedit.bin.zip
unzip marcedit.bin.zip
```

## Benötigte Programmbibliotheken installieren

```
sudo sh -c 'echo "deb http://ftp.indexdata.dk/ubuntu xenial main
deb-src http://ftp.indexdata.dk/ubuntu xenial main" >> /etc/apt/sources.list'
wget http://ftp.indexdata.dk/debian/indexdata.asc
sudo apt-key add indexdata.asc
sudo apt-get install mono-complete zlibc libyaz5 libyaz5-dev
```

## Konfiguration von MarcEdit anpassen

```
echo "<?xml version="1.0" encoding="utf-8" ?>
<configuration>-
   <dllmap dll="yaz4_32.dll" target="libyaz.so.5" />
   <dllmap dll="yaz4_64.dll" target="libyaz.so.5" />
   <dllmap dll="yaz3.dll" target="libyaz.so.5" />
</configuration>" > ~/marcedit/Zoom.Net.YazSharp.dll.config
```

## MarcEdit starten

```
mono ~/marcedit/MarcEdit.exe
```

Beim ersten Start muss noch im Menü ```Locations``` der Pfad zur Programmbibliothek Mono (```Mono Path```) angegeben werden: ```/usr/bin/mono```
