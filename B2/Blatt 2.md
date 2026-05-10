### Git Quest

1.  Ändern Sie eine Datei, die im Branch `end` nicht verändert wurde.
    Erzeugen Sie mit diesen Änderungen auf dem `master` einen neuen
    Commit. Mergen Sie danach den Branch `end` in den `master`-Branch.

nvim stats.md 
Hunger auf 1 gesetzt

git add stats.md

git commit -m "stats.md überarbeitet."

git merge origin/end -m "merge end in master"

git log --graph 
git status
// zum einsehen

2.  Ändern Sie nun eine Datei, die auch im Branch `end` verändert wurde.
    Achten Sie dabei darauf, die Änderung an einer anderen Stelle in der
    Datei vorzunehmen. Erzeugen Sie mit diesen Änderungen auf dem
    `master` einen neuen Commit. Mergen Sie danach den Branch `end` in
    den `master`-Branch.

nvim questlog.md
// einen satz hinzugefügt.

git add questlog.md

git status

git commit -m "questlog überarbeitet."

git merge origin/end

3.  Wie (2), aber ändern Sie nun eine Stelle, die auch im Branch `end`
    verändert wurde. Erzeugen Sie mit diesen Änderungen auf dem `master`
    einen neuen Commit. Mergen Sie danach den Branch `end` in den
    `master`-Branch. Was passiert, wenn die Änderung im `master`
    identisch zu der in `end` ist? Was passiert, wenn die Änderung im
    `master` anders ist als in `end`?

git show origin/end
// änderung sind nur im questlog.md im untersten Paragraph

nvim questlog.md
// ändere den Text im questlog im untersten paragraph und entferne oder tausche wörter aus
/geänderte Wörter Geschichte = Fabel, Landes = Kontinentes

4.  Wie (2), aber setzen Sie bitte den Branch `end` auf die Spitze von
    `master`, bevor Sie `end` in `master` mergen.

nvim questlog.md 
git add questlog.md
git commit

git checkout -b end-neu origin/end
git rebase master

git checkout master
git merge end-neu

//hier als eine klare linie

//während die vorherigen 3 alle auf unterschiedlichen Graphs läuft

### KatzenCafe
## Gradle 
gradle init
// im ordner von gitclone erstellt den gradle build

-   `main()` in `catcafe.Main` über Gradle ausführen können, und
-   JUnit-Tests mit Gradle ausführen können, und
-   den Code mit `google-java-format` formatieren können. Können Sie für
    den Google-Java-Formatter die Einrückung auf 4 Leerzeichen (statt
    der Default 2 Leerzeichen) anpassen? Lange Strings sollen ebenfalls
    umgebrochen werden (falls nötig).

//main() ausführen 
./gradlew run
// test
./gradlew test
// spotless
./gradlew spotlessApply
./gradlew spotlessCheck

// 4 statt 2 Leerzeichen
.refloxLongStrings() 

### JUnit testfälle
10 Testfälle hinzugefügt
aufgeteilt in Anzahl nachfragen
Namen nachfragen
Gewicht nachfragen
