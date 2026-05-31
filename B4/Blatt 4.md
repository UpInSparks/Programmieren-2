# Blatt 4
## Aufgabe 1
### 1.1 MiniJavaTokens.java
MiniJavaTokens.java beinhält 
die RegEx und die Farben zuweisung

public static List<Token> defaultTokens() {
        return List.of(
 
         JavaDoc
            Token.of(
                Pattern.compile("/\\*\\*.*?\\*/", Pattern.DOTALL),
                MiniJavaColours.JAVADOC_COMMENT
            ),
 
         block comment
                Pattern.compile("/\\*.*?\\*/", Pattern.DOTALL),
                MiniJavaColours.COMMENT
            ),
 
         comment
            Token.of(
                Pattern.compile("//[^\\n\\r]*"),
                MiniJavaColours.COMMENT
            ),
 
         string " .... "
            Token.of(
                Pattern.compile("\"(?:[^\"\\\\]|\\\\.)*\""),
                MiniJavaColours.STRING
            ),
         ' .... '
            Token.of(
                Pattern.compile("'(?:[^'\\\\]|\\\\.)'"),
                MiniJavaColours.STRING   // gleiche Farbe wie Strings
            ),
         annotation
            Token.of(
                Pattern.compile("@\\w+"),
                MiniJavaColours.ANNOTATION
            ),
         Keywords
            Token.of(
                Pattern.compile(
                    "\\b(package|import|class|interface|enum|extends|implements|"
                    + "public|private|static|abstract|"
                    + "return|null|"
                    + "if|else|while|for"
                    + "int|long|double|float|boolean|char|byte|void|"
                    + "true|false)\\b"
                ),
                MiniJavaColours.KEYWORD
            ),
         numbers
            Token.of(
                Pattern.compile("\\b\\d+\\b"),
                MiniJavaColours.NUMBER
            ) 

### 1.2 MiniJavaTokensTest.java

Die Testdatei beinhält Tests basierend auf Teil 1.1
im Given, when, then Format

1. Prüft ob /** */ als JAVADOC_COMMENT erkannt wird.
2. Prüft ob /* */ als COMMENT erkannt wird.
3. Prüft ob // als COMMENT erkannt wird.
4. Prüft ob "..." als STRING erkannt wird.
5. Prüft ob 'a' als STRING erkannt wird.
6. Prüft ob @Override als ANNOTATION erkannt wird.
7. Prüft ob class als KEYWORD erkannt wird.
8. Prüft ob 42 als NUMBER erkannt wird.
9. Prüft ob die Default-Token-Liste mindestens einen Eintrag hat.
10. Prüft ob ein Keyword am Textanfang gefunden wird.
11. Prüft ob Keywords in der Mitte und am Ende eines Textes gefunden werden.
12. Prüft ob mehrere Keywords im selben Text alle erkannt werden.
13. Prüft ob kein Keyword in einem Text ohne Keywords gefunden wird.

## Aufgabe 2
### 2.1 RegexHighlighter.collectMatches

2.1 collectMatches
geht durch jeden Token aus MiniJavaTokens durch den kompletten Text 
und sucht nach Stellen wo das TokenPattern passt.
Für jeden Treffer wird eine HighlightRegion mit Start Ende und Farbe erstellt.
Die Liste wird zurück gegeben

### 2.2 RegexHighlighter.resolveConflicts

2.2 resolveConflicts
resolveConflicts geht durch die sortierte Liste
Und entscheidet welche Regionen behaltet werden
Jede Region wird überprüft ob sie sich bereits überlappen
Überlappen = entfernt, kein Überlappen = Region wird akzeptiert
Intervalle sind halb offen = [0, 5) und [5, 8) sind nicht überlappt

### 2.3 JUnit Tests RegexHighlighter
Die Testdatei beinhält Tests basierend auf Teil 2.1 und 2.2
im Given, when, then Format

1. Prüft ob ein Leerstring keine Regionen zurückgibt (collectMatches).
2. Prüft ob ein Text ohne Token-Matches keine Regionen zurückgibt (collectMatches).
3. Prüft ob ein einzelnes Keyword genau eine Region ergibt (collectMatches).
4. Prüft ob zwei angrenzende Regionen [0,5) und [5,10) beide behalten werden (resolveConflicts).
5. Prüft ob bei zwei überlappenden Regionen nur die erste behalten wird (resolveConflicts).
6. Prüft ob ein Keyword innerhalb eines Zeilenkommentars nach der Pipeline nur als COMMENT überlebt.
7. Prüft ob ein JavaDoc-Kommentar als JAVADOC_COMMENT und nicht als COMMENT erkannt wird.
8. Prüft ob ein Leerstring durch die gesamte Pipeline keine Regionen ergibt.

## Aufgabe 4
### 4.1 CI-Pipeline

### 4.2 Branches und Pull Requests

### 4.3 Gegenseitige Reviews der PR