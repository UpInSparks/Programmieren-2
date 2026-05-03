### Git status erklären ###

            pm-lecture % git status
            On branch b03

            Changes not staged for commit:
            (use "git add <file>..." to update what will be committed)
            (use "git restore <file>..." to discard changes in working directory)
                    modified:   CONTRIBUTING.md
                    modified:   homework/b03.md

            Untracked files:
            (use "git add <file>..." to include in what will be committed)
                    foo.java

            no changes added to commit (use "git add" and/or "git commit -a")


Der Branch ist b03 
die CONTRIBUTING.md und homework/b03.md wurden bearbeitet aber noch nicht committed
um hinzuzufügen 
git add CONTRIBUTING.md homework/b03.md

foo.java ist neu und existiert noch nicht im branch bis sie hinzugefügt wird

die staging area ist leer

um weiter zu machen
LÖSUNG:
git add CONTRIBUTING.md homework/b03.md foo.java
git commit -m "CONTRIBUTING.md und homework/b03.md überarbeitet und foo.java hinzugefügt"

### Git Spiel ###

## 1. 
erst alle sachen anzeigen lassen mit 
git log --oneline --all

- was passierte an Tag 01?
git show 4aa8014

- wann hat der Held zum ersten mal 4 experience Punkte?
git log -p --all | grep -A5 -B5 'experience'
git log --all --oneline 
abgleichen zeigt das bei 

2a0717e Tag 01.5
wurden zum erstn mal 4 erfahrung gesammelt

-   Wann hat der Held zum ersten Mal 10 `hunger` Punkte?
commit 38be9017998568a8dab20e3cfbe8e7cf0580ad23

wieder mit grep aber 'experience' mit 'hunger' austauschen
38be901 tag 04.1 wurde zum ersten mal hunger 10 erreicht

-   Wie viele Heiltränke hat der Held insgesamt in seinem Rucksack gehabt?
git log -p -- rucksack.md | grep 'Heiltrank'

wird angegeben das er maximal 1 hatte

-   Was hat der Held im Shop gekauft? Und wie viel Gold hat er dafür bezahlt?

git log -p -- shopkeeper.md
am tag 03 Zwergenbrot

für 5 Gold gekauft

    -   Was passierte zwischen `tag 03` und `tag 04`, d.h. was änderte
        sich zwischen diesen Commits?
git diff 2ffebcf 39568d5 
zeigt die änderung im rucksack und die änderung in stats

git log 2ffebcf..39568d5 -p
zeigt alle änderungen zwischen den beiden commits 
also alles zwischen tag03 und tag04
der unterschied zwischen tag3 und tag4


    -   Hat der Held etwas gegessen? Falls ja, was und wann?
Brot
tag 03.17
git log 2ffebcf..39568d5 -p

## 2. Beim letzten Commit (`tag 04.5`) ist etwas schief gelaufen, es
    wurden versehentlich zu wenig `experience` Punkte eingestellt.
    Ändern Sie diesen letzten Commit und passen Sie die `experience`
    Punkte auf 42 an.

tag04.5 = 76973c1

mit nvim die stats.md geändert und erfahrung auf 42 angepasst

git add stats.md 

git commit --amend --no-edit
--amend = anpassung(kein neuer commit)
--no-edit = Die commit nachricht ändert sich nicht

mit git show wird die änderung dann angezeigt

## 3.  Schreiben Sie die Geschichte in der Datei `questlog.md` fort und
    erzeugen Sie einen neuen Commit für `tag 04.6`. Ändern Sie bitte
    hierzu nur die eine Datei `questlog.md`.

questlog.md und rucksack.md bearbeitet
git add questlog.md
git add rucksack.md

git commit -m "tag04.6"
git tag tag04.6

## 4.  Schreiben Sie die Geschichte noch weiter fort (`tag 04.7`), aber
    ändern Sie diesmal mehrere Dateien, die an diesem Tag (neuer Commit)
    gemeinsam eingecheckt werden sollen.

verändert wurde 
questlog.md
rucksack.md
stats.md

git add questlog.md rucksack.md stats.md

git commit -m "tag04.7"
git tag tag04.7

## 5.  Fälschlicherweise wurden die Statuspunkte und die Ausrüstung bisher
    gemeinsam in der Datei `stats.md` geführt. Korrigieren Sie das und
    verschieben Sie die Ausrüstungsgegenstände aus der Datei `stats.md`
    in eine neue Datei `gear.md`. Checken Sie Ihre Änderungen als
    `tag 04.8` (neuer Commit) gemeinsam ein. (*Hinweis*: Es reicht, wenn
    diese Änderung als letzter Commit auf der Spitze des
    `master`-Branches existiert. Sie brauchen/sollen die Trennung von
    Statuspunkten und Ausrüstung **nicht rückwirkend** in die Historie
    einbauen!)

gear.md erstellt und die Weapon and rüstung aus stats.md hinbewegt und aus stats.md entfernt

git add stats.md gear.md
git commit -m "tag04.8"
git tag tag04.8

git show tag04.8


### Commit Meldungen

Die Erste ist gut kommentiert und verständlich die 2. ist es nicht

### Gradle Installation

groovy Version 4.4.1

mit gradle tasks --all
kann man die tasks sehen

die Java dateien gehen in den src ordner rein wenns geht mit subordnern für bessere struktur

Das Projekt ist aufgeteilt in Konfiguration (settings.gradle, build.gradle)
tests (test/)
Code (main/)

## Erklären des buildscripts
build.gradle 

Plugins (groovy, application)   
repositorys(jcenter())          
Dependencies (compile, testcompile)
Konfiguration (mainClassName)

Application baut auf groovy auf und erweitert es um starten und verteilen zuermöglichen

compileGroovy kompilliert .groovy Dateien
processResources kopiert resourcen aus main nach build/classes/main
jar packt den kompilierten code in eine JAR datei
compileTestGroovy kompiliert Testcode aus src/test/groovy
test führt spock tests aus, macht berichte in build/reports/tests
run startet die Anwendung 
startScripts erzeugt startskripte .sh
installApp legt die Anwendung ab ohne gradle ausführbar
distZip in eine Zip

### JAVA ide

Ich nutze IntelliJ

java version "25.0.2" 2026-01-20 LTS

Die Richtige Version ist vorhanden und funktionsfähig

