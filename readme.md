  #SPIS#
- [Dane komputera](#dane-komputera)
- [Zadanie 1a)](#zadanie-1a)
- [Zadanie 1b)](#zadanie-1b)
- [Zadanie 1c)](#zadanie-1c)
- [Zadanie 1d)](#zadanie-1d)

	Dane Komputera
-*System Operacyjny:* Windows 8.1 x64
-*Procesor:* Intel(R) Core(TM) i7-4510HQ CPU @ 2.00GHz 2.60GHz
-*Liczba rdzeni:* 4 rdzenie
-*Pamiêæ RAM:* 8GB
-*Dysk Twardy:* HDD 932 GB
-*Predkosc zapisu/odczytu:* 102MB/s / 100MB/s
-*Postgesql*: 9.4 x64<br>
-*Mongodb*: 3.0.7 x64

#Zadanie 1a)

Import bazy redit:

Mongodb:

```sh

mongoimport -d zad1 -c redit --type json --file "C:\Users\Tomasz\Desktop\Bzyl\rc\RC_2015-01.json"

1:20:35

```

![alt tag](https://github.com/tomaszwolf/projekt/blob/master/cpu.png "")
![alt tag](https://github.com/tomaszwolf/projekt/blob/master/disk.png "")
![alt tag](https://github.com/tomaszwolf/projekt/blob/master/memory.png "")


PosgreSQL:

```sh

pgfutter_windows_amd64.exe --pw "polska1704" json "EC:\Users\Tomasz\Desktop\Bzyl\rc\RC_2015-01.json"

1:49:02

```

![alt tag](https://github.com/tomaszwolf/projekt/blob/master/cpu1.png "")
![alt tag](https://github.com/tomaszwolf/projekt/blob/master/disk.png "")
![alt tag](https://github.com/tomaszwolf/projekt/blob/master/memory2.png "")

![alt tag](https://github.com/tomaszwolf/projekt/blob/master/import.png "")

#Zadanie 1b)

Zliczanie rekordów:

Mongodb:

```sh

db.reddit.count()
53851542
00:00:15

```

PostgreSQL:

```sh

SELECT COUNT(*) from PLIKI;
53851542
00:00:35

```

#Zadanie 1c)

Mongodb:

```sh

db.reddit.find({"edited" : false}).count()
52233268
00:13:10

db.reddit.find({"edited" : false, "score" : {$gt: 10}}).count()
3379738
00:18:01

```

PosgreSQL:

```sh

SELECT count(*) FROM IMPORT.RC_2015_01 WHERE edited=false;
52233268
00:15:34

SELECT count(*) FROM IMPORT.RC_2015_01 WHERE edited=false AND score<10;
3379738
00:13:34

```

#Zadanie 1d)

Operacje na bazie danych KFC w Warszawie i okolicach<br>
<br>


Import bazy:

```sh

mongoimport -d zad1 -c geoj --type csv --file "C:\Users\Tomasz\Desktop\Bzyl\rc.csv" --headerline
00:01:10


```

Dodajemy geo-index:

```sh

db.geoj.ensureIndex({"loc" : "2dsphere"})

```

### 1

KFC w Warszawie (ograniczenie 20):

```sh

var warszawa = { "type": "Point", "coordinates": [52.2651, 21,0153] };
db.geoj.find({ loc: {$near: {$geometry: london}} }).limit(20)

```

[mapa](https://github.com/tomaszwolf/projekt/blob/master/geo1.geojson)

### 2

KFC w promieniu 0.1 stopnia od centrum Londynu:

```sh

db.geoj.find({loc: {$geoWithin: {$center: [[52.2651, 21.0153], 0.10]}}})


```

[mapa](https://github.com/tomaszwolf/projekt/blob/master/geo2.geojson)

### 3

KFC na lini Centrum Warszawa - Lotnisko Chopina:

```sh

var line = { "type": "LineString","coordinates": [[52.2651, 21.0153], [52.167433, 20.968307]] }
db.geoj.find({ loc: {$geoIntersects: {$geometry: line}} }).limit(20)

```

[mapa]()

### 4

KFC na obszarze od centrum Warszawa na po³udnie i po³udniowy wschód:


```sh

db.geoj.find({
    loc: {
        $geoWithin: {
            $geometry: {
                "type": "Polygon",
                "coordinates": [[
                    [52.26510, 21.01530],
                    [52.21142, 21.05053],
                    [52.21836, 20.99216],
		    [52.26510, 21.01530]
                ]]
            }
        }
    }
}).limit(20)

```

[mapa](https://github.com/tomaszwolf/projekt/blob/master/geo4.geojson)
