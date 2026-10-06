# cv02: Kostra projektu a modely

Teória a ukážky: <https://beata.stehlikova.website.tuke.sk/it/03.html>

**Odovzdávaš (do nasledujúceho cvičenia):** v tomto repository `sql/schema.sql`, `sql/seed.sql`, vlastné modely v `src/Model/` a upravený `index.php`; vo svojom webovom priestore na sigma.tuke.sk v `public_html/it/cv02` funkčný `index.php`, ktorý vypíše tabuľky a záznamy.

**Hodnotenie:** 1 bod.

**Čo je v kostre** ([kostra.zip](https://beata.stehlikova.website.tuke.sk/it/kostra.zip)): `composer.json` (autoload PSR-4), `.htaccess` (chráni `src/`, `vendor/`, `sql/` a `config.php`), `.gitignore`, `config.example.php`, `src/Database.php` (jedno PDO pripojenie), `src/Model/ModelInterface.php`, `src/Model/BaseModel.php` (rodič číselníkov), `src/Model/LookupModel.php` (vzor číselníka), `src/Model/MainModel.php` (vzor hlavnej entity), `index.php` (kontrolná stránka).

**Úloha 3.1: Kubeflow: príprava.** Podľa návodu [04 Odovzdávanie](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html), časť 1: otvor Kubeflow, skontroluj, že v Explorer je hore **WEB**, a skontroluj rozšírenia. Ak dole na stavovej lište nie je **Go Live**, rozšírenia sa po reštarte stratili. Vráti ich tento príkaz v termináli (jeden riadok):

```bash
code-server --install-extension Natizyskunk.sftp --install-extension yandeu.five-server --install-extension bmewburn.vscode-intelephense-client --install-extension mblode.twig-language-2
```

Potom `F1`, napíš `reload`, **Developer: Reload Window**.

**Úloha 3.2: Clone, kostra a nastavenie.** Ak ešte nemáš tento repository, prijmi zadanie tu: <https://classroom50.org/tuke-fberg-stehlikova/informacne-technologie-5k/assignments/cv02/accept?k=xzmai3oh>. Repository naklonuj do `~/web/it/cv02` podľa návodu 04, časti 2 a 3 (posledné slovo príkazu je `cv02`). Repository má zatiaľ len tento `README.md`; kostru stiahneš z webu predmetu a rozbalíš do neho. V termináli:

```bash
cd ~/web/it/cv02
curl -O https://beata.stehlikova.website.tuke.sk/it/kostra.zip
unzip kostra.zip && rm kostra.zip
composer install
cp config.example.php config.php
```

V Explorer sa objavia súbory kostry (`.htaccess` a `.gitignore` začínajú bodkou, Explorer ich ukáže sivšie). Otvor `config.php` v editore a doplň `dbname`, `user` a `pass` (heslo z cvičenia). Ulož. Potom pravý klik na priečinok `cv02`, **Upload Folder** – tým sa na sigma.tuke.sk dostane aj `vendor/`, ktorý neprešiel editorom. Kontrola: `https://sigma.tuke.sk/student/meno.priezvisko/it/cv02/` ukáže stránku „Kontrola projektu" s 0 tabuľkami.

**Úloha 3.3: Tabuľky a testovacie dáta cez AI.** Prompt 1 vlož do AI spolu s dátovým modelom zo svojho README z cv01. Výsledok ulož ako `sql/schema.sql` a `sql/seed.sql`. Prečítaj si každý `ON DELETE`, musí sedieť s tvojím modelom. V Adminer klikni vľavo **SQL command**, vlož obsah `schema.sql`, **Execute**; to isté so `seed.sql`. Kontrola: v Adminer vidíš svoje tabuľky s dátami a stránka „Kontrola projektu" ich vypíše v zozname.

**Úloha 3.4: Modely cez AI.** Prompt 2 vlož do AI spolu so súbormi `src/Model/ModelInterface.php`, `BaseModel.php`, `LookupModel.php`, `MainModel.php` a svojím `sql/schema.sql`. Vzniknuté triedy ulož do `src/Model/` (jedna trieda = jeden súbor, názov súboru = názov triedy). Vzorové `LookupModel.php` a `MainModel.php` potom zmaž. Prečítaj každú metódu, pri obhajobe ju vysvetlíš.

**Úloha 3.5: Kontrolná stránka.** V `index.php` doplň do poľa `$models` jeden riadok `new \App\Model\TvojModel(),` pre každú svoju triedu. Ulož a otvor stránku na sigma.tuke.sk: pri každom modeli vidíš popis, počet záznamov a prvých 5 riadkov. Pri hlavnej entite musí byť aj stĺpec s názvom z číselníka (JOIN). Ak stránka hlási chybu, prečítaj ju – `debug` v `config.php` je zapnutý práve preto.

**Úloha 3.6: Odovzdanie.** Commit a push podľa návodu 04, časť 5. `config.php` a `vendor/` do repository nejdú (sú v `.gitignore`), to je správne. Na GitHub skontroluj, že je tam celá kostra, `sql/`, `src/Model/` s tvojimi triedami a `index.php`.

**Úloha 3.7: Kontrola na sigma.tuke.sk.** Pravý klik na `cv02`, **Upload Folder**, potom otvor `https://sigma.tuke.sk/student/meno.priezvisko/it/cv02/`. Over aj, že `https://sigma.tuke.sk/student/meno.priezvisko/it/cv02/src/` a `…/cv02/config.php` vrátia chybu 404 alebo 403 – kód a heslo nie sú z webu prístupné.

**Prompt 1 – tabuľky a testovacie dáta:**

```text
Nižšie je dátový model mojej aplikácie (tabuľky, stĺpce, vzťahy, pravidlá ON DELETE).
Vyrob z neho dva súbory pre MariaDB 10.6.

1. schema.sql
- Na začiatku DROP TABLE IF EXISTS pre všetky tabuľky v poradí, v akom sa dajú zmazať
  (prepojky, potom hlavná entita, potom číselníky).
- CREATE TABLE v poradí, v akom sa dajú vytvoriť (číselníky, hlavná entita, prepojky).
- Každá tabuľka: ENGINE=InnoDB, DEFAULT CHARSET=utf8mb4, COLLATE=utf8mb4_unicode_ci.
- Primárny kľúč: id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY.
- Cudzie kľúče ako FOREIGN KEY ... REFERENCES ... s pravidlom ON DELETE presne podľa modelu.
- Pri každej tabuľke a každom cudzom kľúči krátky SQL komentár (-- ...) po slovensky,
  čo tabuľka drží a prečo platí dané pravidlo ON DELETE.
- Nepíš CREATE DATABASE ani USE.

2. seed.sql
- INSERT do každej tabuľky, 3 až 5 riadkov, zmysluplné hodnoty po slovensky k mojej téme.
- Poradie vkladania: číselníky, hlavná entita, prepojky; cudzie kľúče odkazujú na existujúce id.

Dátový model:
[SEM VLOŽ DÁTOVÝ MODEL Z README]
```

**Prompt 2 – modely:**

```text
Prikladám kostru PHP projektu (namespace App\Model, autoload PSR-4 cez Composer):
ModelInterface.php, BaseModel.php, LookupModel.php (vzor číselníka), MainModel.php
(vzor hlavnej entity) a svoj schema.sql.

Vyrob pre moje tabuľky triedy modelov, pre každú jeden súbor do src/Model/:
- pre každý číselník triedu podľa vzoru LookupModel: extends BaseModel, nastav $table,
  describe() vráti krátky popis po slovensky; nič iné nepridávaj,
- pre hlavnú entitu triedu podľa vzoru MainModel: implements ModelInterface, getAll() a
  getById() s JOIN na číselník pripojený vzťahom 1:N (vráť aj jeho name pod zrozumiteľným
  aliasom), getCount(), delete(), describe(); prepared statements s pomenovanými
  placeholdermi.
Názvy tried: anglicky, jednotné číslo, prípona Model (categories -> CategoryModel).
Každá trieda má PHP 8.1 syntax: declare(strict_types=1), typy parametrov a návratových hodnôt.
Nad každou metódou jednoriadkový komentár po slovensky, čo robí.
Na konci napíš riadky, ktoré mám vložiť do poľa $models v index.php.

[SEM VLOŽ OBSAH ŠTYROCH SÚBOROV KOSTRY A SCHEMA.SQL]
```

---

# cv02: Project skeleton and models (English)

Theory and examples: <https://beata.stehlikova.website.tuke.sk/it/03.html> (switch to EN at the top)

**You submit (by the next lab):** in this repository `sql/schema.sql`, `sql/seed.sql`, your own models in `src/Model/` and the updated `index.php`; in your web space on sigma.tuke.sk in `public_html/it/cv02` a working `index.php` that lists the tables and records.

**Grading:** 1 point.

**What the skeleton contains** ([kostra.zip](https://beata.stehlikova.website.tuke.sk/it/kostra.zip)): `composer.json` (PSR-4 autoload), `.htaccess` (protects `src/`, `vendor/`, `sql/` and `config.php`), `.gitignore`, `config.example.php`, `src/Database.php` (one PDO connection), `src/Model/ModelInterface.php`, `src/Model/BaseModel.php` (parent of lookup models), `src/Model/LookupModel.php` (lookup table example), `src/Model/MainModel.php` (main entity example), `index.php` (check page).

**Task 3.1: Kubeflow: getting ready.** Following guide [04 Submitting](https://beata.stehlikova.website.tuke.sk/navody/04-odovzdavanie.html), part 1: open Kubeflow, check that Explorer shows **WEB** at the top, and check the extensions. If the status bar at the bottom does not show **Go Live**, the extensions were lost on restart. This command in the terminal (one line) brings them back:

```bash
code-server --install-extension Natizyskunk.sftp --install-extension yandeu.five-server --install-extension bmewburn.vscode-intelephense-client --install-extension mblode.twig-language-2
```

Then `F1`, type `reload`, **Developer: Reload Window**.

**Task 3.2: Clone, skeleton and setup.** If you do not have this repository yet, accept the assignment here: <https://classroom50.org/tuke-fberg-stehlikova/informacne-technologie-5k/assignments/cv02/accept?k=xzmai3oh>. Clone the repository into `~/web/it/cv02` following guide 04, parts 2 and 3 (the last word of the command is `cv02`). The repository has only this `README.md` so far; you download the skeleton from the course website and unpack it into the repository. In the terminal:

```bash
cd ~/web/it/cv02
curl -O https://beata.stehlikova.website.tuke.sk/it/kostra.zip
unzip kostra.zip && rm kostra.zip
composer install
cp config.example.php config.php
```

The skeleton files appear in Explorer (`.htaccess` and `.gitignore` start with a dot; Explorer shows them greyed). Open `config.php` in the editor and fill in `dbname`, `user` and `pass` (the password from the lab). Save. Then right-click the `cv02` folder, **Upload Folder** – this gets `vendor/`, which did not pass through the editor, to sigma.tuke.sk as well. Check: `https://sigma.tuke.sk/student/name.surname/it/cv02/` shows the "Kontrola projektu" page with 0 tables.

**Task 3.3: Tables and test data with AI.** Paste prompt 1 into an AI together with the data model from your cv01 README. Save the result as `sql/schema.sql` and `sql/seed.sql`. Read every `ON DELETE`; it must match your model. In Adminer click **SQL command** on the left, paste the contents of `schema.sql`, **Execute**; the same with `seed.sql`. Check: Adminer shows your tables with data and the "Kontrola projektu" page lists them.

**Task 3.4: Models with AI.** Paste prompt 2 into an AI together with the files `src/Model/ModelInterface.php`, `BaseModel.php`, `LookupModel.php`, `MainModel.php` and your `sql/schema.sql`. Save the generated classes into `src/Model/` (one class = one file, file name = class name). Then delete the example `LookupModel.php` and `MainModel.php`. Read every method; you will explain it at the defence.

**Task 3.5: Check page.** In `index.php` add one line `new \App\Model\YourModel(),` to the `$models` array for each of your classes. Save and open the page on sigma.tuke.sk: for every model you see the description, the record count and the first 5 rows. The main entity must also show the column with the name from the lookup table (JOIN). If the page reports an error, read it – `debug` in `config.php` is on exactly for this.

**Task 3.6: Submitting.** Commit and push following guide 04, part 5. `config.php` and `vendor/` do not go into the repository (they are in `.gitignore`); that is correct. On GitHub check that the whole skeleton, `sql/`, `src/Model/` with your classes and `index.php` are there.

**Task 3.7: Check on sigma.tuke.sk.** Right-click `cv02`, **Upload Folder**, then open `https://sigma.tuke.sk/student/name.surname/it/cv02/`. Also check that `https://sigma.tuke.sk/student/name.surname/it/cv02/src/` and `…/cv02/config.php` return a 404 or 403 error – the code and the password are not reachable from the web.

**Prompt 1 – tables and test data:**

```text
Below is the data model of my application (tables, columns, relationships, ON DELETE rules).
Create two files from it for MariaDB 10.6.

1. schema.sql
- At the beginning DROP TABLE IF EXISTS for all tables in the order they can be dropped
  (pivot tables, then the main entity, then lookup tables).
- CREATE TABLE in the order they can be created (lookup tables, main entity, pivot tables).
- Every table: ENGINE=InnoDB, DEFAULT CHARSET=utf8mb4, COLLATE=utf8mb4_unicode_ci.
- Primary key: id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY.
- Foreign keys as FOREIGN KEY ... REFERENCES ... with the ON DELETE rule exactly as in the model.
- A short SQL comment (-- ...) at every table and every foreign key saying what the table holds
  and why the given ON DELETE rule applies.
- Do not write CREATE DATABASE or USE.

2. seed.sql
- INSERT into every table, 3 to 5 rows, meaningful values for my topic.
- Insert order: lookup tables, main entity, pivot tables; foreign keys refer to existing ids.

Data model:
[PASTE THE DATA MODEL FROM YOUR README HERE]
```

**Prompt 2 – models:**

```text
Attached is the skeleton of a PHP project (namespace App\Model, PSR-4 autoload via Composer):
ModelInterface.php, BaseModel.php, LookupModel.php (lookup table example), MainModel.php
(main entity example) and my schema.sql.

Create model classes for my tables, one file each into src/Model/:
- for every lookup table a class following LookupModel: extends BaseModel, set $table,
  describe() returns a short description; add nothing else,
- for the main entity a class following MainModel: implements ModelInterface, getAll() and
  getById() with a JOIN to the lookup table connected by the 1:N relationship (return its name
  under a readable alias), getCount(), delete(), describe(); prepared statements with named
  placeholders.
Class names: English, singular, suffix Model (categories -> CategoryModel).
Every class uses PHP 8.1 syntax: declare(strict_types=1), parameter and return types.
A one-line comment above every method saying what it does.
At the end write the lines I should add to the $models array in index.php.

[PASTE THE CONTENTS OF THE FOUR SKELETON FILES AND SCHEMA.SQL HERE]
```
