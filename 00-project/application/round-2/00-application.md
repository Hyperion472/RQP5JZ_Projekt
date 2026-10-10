---
name: Gombos Ádám
neptun: RQP5JZ
project: 2026-LA-03
---
# Játékos-visszajelzések adattárháza

## A brief értelmezése
A projekt célja, hogy 3 videojátékról (Counter-Strike 2, Cyberpunk 2077, Grand Theft Auto V)
szóló véleményeket (főként Steam-en írt értékelések és Youtube kommentek formájában) különböző staging
rétegekben összegyűjtő, majd integráló, és végül tároló folyamatot tervezzek meg, majd hozzak létre.
Az adatok különböző szempontok alapján  megkülönböztethetőek és elemezhetőek legyenek, 
amiket egy jól működő, verziókezelt, és jól dokumentált adattárban tárolok.

## Miért én lennék alkalmas erre a projektre?
Amióta ismerem a Rotten Tomatoes nevű films véleményeket/kritikákat gyűjtő oldalt, azóta érdekelt egy ilyen rendszernek a működése,
és létrehozása egy videojátékos témában, mivel az az egyik kedvenc hobbim. 
A különböző oldalak API-ainak működése és az adatbázisok kezelése, illetve az emberi vélemények gyűjtése és rendszerezése pedig szintén érdekes téma számomra.

## Releváns tapasztalat és előzmények

## Tervezett megközelítés
Először megtervezem a dimenzionális modell megközelítését, majd létrehozom az adattáblákat MSSQL segítségével.
Meghatározom az adatgyűjtés, integráláshoz tartozó folyamatok és rétegek működését, illetve azok reagálását hibákra, duplikálásra és egyéb kivételes esetekre.
Tervezek egy script-et ami az adott platformok hivatalos hozzáférésein keresztül végzi az adatgyűjtést és megvizsgálom a használt API-ok használati korlátait.
Megtervezem az lekérdezési funkciókat, és az ahhoz tartozó folyamatokat.

## Kezdeti terv 
1. Használt API-ok korlátozottságainak vizsgálata
2. Dimenzionális adatmodell megtervezése
3. Adatgyűjtési folyamatok, staging réteg, és adatfeldolgozás megtervezése és létrehozása
4. ETL folyamat megtervezése és létrehozása
5. Lekérdezések megtervezése, létrehozása, majd optimalizálása.

## További információk
A projekten való munkát Git repóban vezetem, issue-kat használva feladatokra bontom
és a folyamatos munkát oda tölteném fel.
