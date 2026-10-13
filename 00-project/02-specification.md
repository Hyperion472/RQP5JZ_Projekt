---
project_outline: "[[00-project/01-project-outline]]"
---
# Játékos-visszajelzések adattárháza

## 1. Szereplők és jogosultságok

| Szerep | Igény | Felelősség / hozzáférés |
| --- | --- | --- |
| Adatgyűjtő | Rendszer létrehozása, adatok sikeres begyűjtése és megfelelő tárolása | Hozzáfér a teljes rendszerhez, ő felel az adatgyűjtésért, a staging réteg létrehozásáért, az adattárház megfelelő működéséért, és az adatok egységesítésének SQL folyamatáért. |
| Adatelemző | Adatokból összetett hasznos információ kinyerése | SQL lekérdezések létrehozása, illetve azok optimalizálása |

## 2. Use case-ek

### Adatok begyűjtése és betöltése

- **Szereplő:** Adatgyűjtő
- **Fő folyamat:** 
  1. A Steam és Youtube API-ok használatával megszerezzük a nyers adatokat
  2. A nyers adatok a staging rétegbe töltődnek.
  3. A rendszer ellenőrzi és tisztítja az adatokat SQL lekérdezésekkel (pl. duplikáltak, haszontalan információk) 
  4. A rendszer egységesíti az adatokat (pl. időformátum, nyelvek)
  5. Az új/módosult adatok betöltődnek a központi ténytáblába.

-  **Nem ideális esetek:** 
  - API hiba esetén (pl. rate limit, csatlakozási hiba) a rendszer hibaüzenettel jelez, és új próbát tesz a következő futáskor.
  - Hibás adatok esetén a rendszer a staging rétegből nem engedi tovább a hibás adatokat a ténytáblába, és hibaüzenettel jelez.
- **Utófeltétel:** Az adatok betöltődnek a ténytáblába, elemzésre kész állapotban és jegyezve van az adatgyűjtés időpontja illetve forrása.

### Adatok elemzése

- **Szereplő:** Adatelemző
- **Előfeltétel:** az adatgyűjtés sikeres, az adattárház elérhető
- **Fő folyamat:**
  1. Az elemző megtervezi az igényelt elemzést
  2. Az elemző létrehozza a megfelelő SQL lekérdezést.
  3. A lekérdezési folyamatot lefuttatja
  4. A sikeres lekérdezés eredményét megjeleníti a rendszer

- **Nem ideális eset:**
  - Az adatbázis valami okból nem érhető el, ilyenkor hibaüzenettel jelzés.
  - A lekérdezés hibás, vagy túl sokáig fut, erről szintén hibaüzenetet kap a felhasználó.
- **Utófeltétel:** A lekérdezések eredménye megjelenik, és az adatbázis nem módosul. Hibás lekérdezés esetén egy hibaüzenet által tudja milyen hiba történhetett.

## 3. Funkcionális követelmények

| Követelmény | Elfogadási kritérium | Prioritás |
| --- | --- | --- |
| Adatgyűjtés Steam-ről és Youtube-ról | A rendszer sikeresen begyűjti az adatokat, a megfelelő játékokról, a megfelelő forrásokból. | Kötelező |
| Staging réteg létrehozása | A begyűjtött nyers adatok egy staging rétegbe kerülnek, mielőtt SQL folyamatokkal átalakítjuk és betöltjük a ténytáblába. | Kötelező |
| Duplikált/hibás adatok szűrése | A rendszer szűri, hogy egy vélemény egyszer kerüljön be, ha hiányzik egy adat akkor ad egy alapértelmezett értéket (pl. Youtube kommentek like száma hiányzik, akkor 0-t kap) | Kötelező |
| Adatbetöltés inkrementális | Ha az adatgyűjtés egy olyan forrásból történik ahonnan már egyszer gyűjtött adatot a rendszer, akkor nem helyettesíti a már begyűjtött véleményeket saját magukkal, minden lekérdezéssel csak bővül. | Kötelező |
| Dimenzionális adattárház | A fő adatbázis egy közös ténytáblából, és több dimenziótáblából áll. | Kötelező |
| Elemző SQL lekérdezések | Legalább 10 összetett SQL elemzés lekérdezés létrehozása. | Kötelező |
| Lekérdezések teljesítménymérése | Legalább 5 lekérdezés teljesítményének mérése, majd indokolt esetben optimalizálás. | Kötelező |
| Youtube komment szövegelemzés | A Youtube kommentek szöveges elemzése szószűréssel, vagy MI segítségével. | Ajánlott |
| További platformok/játékok | További platformokról vagy játékokról való sikeres adatgyűjtés. | Lehetséges |
| Interaktív BI dashboard | Az elemzések vizualizált formában megjelennek, az elemző segítségére. | Lehetséges |

## 4. Üzleti szabályok és korlátok

| Szabály vagy korlát | Indoklás |
| --- | --- |
| API rate limit | A Steam és Youtube korlátozza mennyi adatot lehet gyűjteni az oldalaikról. |
| Youtube kommentek hasznossága | Azok a Youtube kommentek amik nem a játék véleményezését segítik számunkra haszontalanok. |
| Személyes adatok szűrése | Személyes adatok tárolását szeretnénk elkerülni, mivel ezt jogtalanul tennénk. |
| Limitált tárolt adat | A játékok népszerűsége miatt sok a vélemény, és van egy tárhely határunk. |
| Vásárló és Kommentelő megkülönböztetése | A Steam véleményeknél szerepel, hogy az író megvette-e, illetve játszott-e a játékkal, de a Youtube kommenteknél ez nem áll a rendelkezésünkre. |

## 5. Nem funkcionális követelmények

| Követelmény | Ellenőrzés módja |
| --- | --- |
| A lekérdezések sebessége | A lekérdezések idejét lemérjük, és optimalizáljuk |
| Növekvő adatbázis | A dimenziótáblák ellenőrzése tervezéskor, hogy tud-e adatbetöltéskor magától bővülni. |
| Adatvédelem betartása | Adatbázis ellenőrzése lekérdezéssel, tartalmaz-e email címet, nevet stb. |
| Hálózati hiba kezelése | Hibás API kulcs megadása direkt, hálózat megszakítása, majd ezután az adatbázis ellenőrzése. |

## 6. Nyitott kérdések és kockázatok

| Kérdés / kockázat | Hatás | Felelős | Feloldás / döntés |
| --- | --- | --- | --- |
| Hogyan elemezzük a Youtube kommentek pozitív/negatív véleményét? | A vélemények elemzésekor a játék értékelése függhet a szöveg helyes elemzésétől. | Hallgató | Szövegelemzés szószűréssel, kulcskifejezésekkel, vagy MI segítségével, mindegyik opció tesztelése. |
| Dimenzionális modell: Star/Snowflake? | A lekérdezések optimalizálását befolyásolhatja | Hallgató | Elméleti teszt, kutatás után véglegesítés |

## 7. Kezdeti technikai javaslat
- Programkörnyezet: Python
- Adatbázis kezelő: MSSQL
- Adatbetöltés: ELT folyamat használata, hogy ha hibázunk az adatok átalakításában, a nyers adatok még mindig a staging rétegben megtalálhatóak.
