---
name: Gombos Ádám
neptun: RQP5JZ
id: 2026-LA-03
github: https://github.com/Hyperion472
github_project: https://github.com/Hyperion472/RQP5JZ_Projekt
---
# Játékos-visszajelzések adattárháza

A projekt keretein belül, egy adattárházat létrehozni, amiben 3db videojátékról (Cyberpunk 2077, Counter-Strike 2, Grand Theft Auto V) írt vélemények gyűlnek Youtube kommentek és Steam értékelések formájában, amiket a begyűjtéskor az ETL folyamat egy staging rétegben tárol, arra az időre amíg az adatokat átalakítja egy egységes formára, majd végül betöltődnek az adattárházba. Az adattároló dimenzionális adatmodellt használ. Végül létrejön több elemzési lekérdezés, ami különböző módokon elemzi az adatokat, például a játékok összehasonlítása, vagy egy bizonyos patch vagy update utáni nagyobb véleményváltozás.

## Célok

- **Elsődleges cél:** A forrásadatok elkülönítése, egységesítése, majd tárolása az elemző folyamatok számára.
- **Célfelhasználók / érintettek:** Adatelemzők, videojátékok iránt érdeklődők, játékfejlesztők
- **Mérhető sikerkritériumok:**
    - A rendszer begyűjti az adatokat és megfelelően egységesíti őket.
    - A lekérdezések jól értelmezhető elemzéseket adnak vissza.
- **Korlátok:** 
    - Dimenzionális adatstruktúra
    - Begyűjtött adatok mennyisége
    - API használat korlátok (Adatvédelem és mennyiségi limit)

## Hatókör
### Benne van a hatókörben

- Adatok begyűjtése API segítségével
- Adatok egységesítése, és tisztítása ETL folyamattal
- Szöveges vélemények elemzése
- Adatok attribútumainak kezelése
- Betöltés időpontjának tárolása
- Lekérdezések létrehozása
- Lekérdezés teljesítménymérése

### Nincs benne a hatókörben

- Közösségi média elemzése
- Összetett webes felület létrehozása
- Több forrás bevonása
