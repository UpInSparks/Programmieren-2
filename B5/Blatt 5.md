# Blatt 5
## Aufgabe 1
### 1.1 Syntaxhighlighting mit dem AntlrTokenCollector
#### // Implementieren Sie in AntlrTokenCollector die Methode List<HighlightRegion> collectMatches(String text). Nutzen Sie diesmal den generierten highlighting.antlr.MiniJavaLexer, um aus dem Eingabetext einen Token-Stream zu erzeugen, und wandeln Sie die Tokens anschließend in HighlightRegion-Objekte um. //

collect matches wurde implementiert.
Eingabetext wird über CharStreams.fromString() in einen Charstream gewandelt.
Der MiniJavaLexer tokenisiert den CharStream.
commonTokenStream mit Token.ALL_CHANNEL erfasst alle Tokens.
Jeder Token wird eine Farbe zugeordnet basierend auf den Typ,
dies wird gespeichert als HighlightRegion(start, stopIndex+1, farbe)

Tokens in HighlightRegion werden in Objekte umgewandelt.
Keywords -> MiniJavaColours.KEYWORD
LINE_COMMENT, BLOCK_COMMENT -> MiniJavaColours.COMMENT
JAVADOC_COMMENT -> MiniJavaColours.JAVADOC_COMMENT
STRING_LITERAL, CHAR_LITERAL -> MiniJavaColours.STRING
Annotationen (AT + IDENTIFIER) -> MiniJavaColours.ANNOTATION

#### // Überlegen Sie, ob und wie in diesem Fall noch Konflikte zwischen Regionen entstehen können und wie die beiden Hook-Methoden normalize und resolveConflicts aus der Oberklasse AntlrTokenCollector in diesem Fall aussehen müssen. Überschreiben Sie die Methoden entsprechend in AntlrTokenCollector (falls notwendig). //

Konflikte zwischen Regionen
Der ANTLR Lexer arbeitet von links nach rechts mit longestmatch.
Jede Zeichenposition gehört einem Token.
Überlappung daher nicht möglich.
Beim RegexHighlighter wendet dieser Patterns parallel an.

normalize und resolveConflicts
normalize: wird nicht überschrieben – die Basisimplementierung (Sortierung + Filterung) reicht aus
resolveConflicts: wird nicht überschrieben – da keine Überlappungen entstehen, ist die Standard-Implementierung ("alle behalten") korrekt

#### // Vergleichen Sie das so erzeugte Syntaxhighlighting mit den Varianten von Blatt 04. Wo gibt es Unterschiede, was sind die Gründe dafür? Welche der Varianten ist aufwändiger in der Implementierung? //

Unterschiede
Keywords wie abstract, int, boolean, etc. Der RegexHighlighter erkennt sie der ANTLRHighlighter nicht.
Zahlen, der RegexHighlighter erkennt Zahlen, der ANTLR hat keine NumberTOken Grammatik.
Annotation, Der RegexHighlighter macht eine Region, Der ANTLR Highlighter muss AT und IDENTIFIER zusammenführen.
Kommentare, beide erkennen Kommentare , aber beim RegexHighlighter kann ein Keyword in einem Kommentar sein und überlappen, dies wird mit resolveConflicts gelöst.

Gründe
Der ANTLR Highlighter ist basierend auf der definierten Grammatik, wenn kein Token definiert ist kann daruaf nicht gefärbt werden.
Der RegexHighlighter ist unabhängig von einer Grammatik und kann beliebig viele Pattern definieren.

Der ANTLR Highlighter ist einfacher in der Implementierung, weil der Lexer die komplette Tokenisierung übernimmt und Konflikte unmöglich sind,
während der RegexHighlighter Patterns manuel definiert und Reihenfolge und Konflikte spezifisch aufgelöst werden müssen.

### 1.2 Visitoren mit ANTLR (Pretty Printing)

#### Schritt 1 - ParseTree erzeugen
CharStreams.fromString(text) wandelt den Eingabetext in einen ANTLR Charstream um
MiniJavaLexer tokenisiert den Charstream
CommonTOkenStream verwandelt den Tokenstream für den PArser
MinJavaParser erstellt daraus einen Parse Tree als CompilationUnitContext
Der Visitor traversiert den ParseTree

#### Schritt 2 - Visitor Implementiren
visitorCompilationUnit: packagedeclaration, importdeclaration, typedeclaration, mit einer Leerzeile als trenner zwischen imports und klassen
visitClassBody: schreibt die Zeilenumbrüche ({}) und managed das eindrücken von Zeilen
visitBlock: wie visitClassBody, Zeileumbrüche und Inhalt eingedrückt für blockstatement, currentindent wird hoch und runtergezählt für verschachtelung
visitStatement: Erkennt anhand des ersten Tokens die art des statements, return schreibt Keyword + Ausdruck + ;, 
if schreibt Bedingung + then-block + optionalen else-block, while schreibt Bedingungen + Block, AusdrucksStatements werden direkt mit abschließendem ; ausgegeben

#### Schritt 3 - Demonstration
Der Konstruktor PrettyPrinterVisitor(int indentWidth) nimmt die gewünschte Einrückbreite entgegen
Der Benutzer wird nach der Anzahl Leerzeichen gefragt, der Visitor wird mit diesem Wert erstellt
visitor.visit(tree) traversiert den Parse-Tree, visitor.result() liefert den formatierten String zur Ausgabe

## Aufgabe 2 Cycle Chronicles
### 2.1 Analyse der Äquivalenzklassen & Grenzwerte
ÄK1 (ungültig): Fahrradtyp == EBIKE -> Ablehnung
ÄK2 (ungültig): Fahrradtyp == GRAVEL -> Ablehnung
ÄK3 (gültig): Fahrradtyp ∈ {RACE, SINGLE_SPEED, FIXIE} -> kein Ablehnungsgrund
ÄK4 (ungültig): Kunde hat bereits einen offenen Auftrag -> Ablehnung
ÄK5 (gültig): Kunde hat keinen offenen Auftrag -> kein Ablehnungsgrund
ÄK6 (gültig): Warteschlangengröße < 5 -> Annahme möglich
ÄK7 (ungültig): Warteschlangengröße ≥ 5 -> Ablehnung

GW1: 0 Aufträge in der Queue -> Annahme möglich (untere Grenze)
GW2: 3 Aufträge in der Queue -> Annahme möglich (unter der Grenze)
GW3: 4 Aufträge in der Queue -> 5. Auftrag wird noch angenommen (genau an der Grenze)
GW4: 5 Aufträge in der Queue -> 6. Auftrag wird abgelehnt (eine über der Grenze)

### 2.2 Mocking 1
Testklasse ShopAcceptTest.java unter src/test/java/cyclechronicles/ angelegt
Hilfsmethode mockOrder(Type, String) kapselt das wiederholte Mockito-Setup:

Order o = mock(Order.class);
when(o.getBicycleType()).thenReturn(type);
when(o.getCustomer()).thenReturn(customer);

@BeforeEach erstellt vor jedem Test eine frische Shop-Instanz → Tests sind voneinander unabhängig
Für jeden der 13 Testfälle aus Aufgabe 2.1 eine @Test-Methode implementiert:

ÄK1/ÄK2: Mock mit EBIKE/GRAVEL → assertFalse
ÄK3a–c: Mocks mit RACE, SINGLE_SPEED, FIXIE → assertTrue
ÄK4: Zwei Mocks mit demselben Kundennamen, zweiter Auftrag → assertFalse
ÄK5: Zwei Mocks mit unterschiedlichen Kundennamen → beide assertTrue
GW1–GW3: Queue per Schleife mit Mock-Aufträgen befüllen, dann prüfen → assertTrue
GW4: Queue auf 5 Aufträge befüllen, weiterer Auftrag → assertFalse
Kombination volle Queue + E-Bike und Gravel + Doppelkunde → jeweils assertFalse

Alle 13 Tests mit ./gradlew test erfolgreich ausgeführt

### Mocking 2
repair():

Nimmt den ältesten Auftrag aus pendingOrders (FIFO, da LinkedList)
Verschiebt ihn in completedOrders
Gibt ihn als Optional<Order> zurück
Gibt Optional.empty() zurück, wenn keine offenen Aufträge vorhanden sind


deliver(String c):

Sucht in completedOrders nach einem Auftrag mit passendem Kundennamen
Entfernt den gefundenen Auftrag aus completedOrders
Gibt ihn als Optional<Order> zurück
Gibt Optional.empty() zurück, wenn kein passender Auftrag gefunden wird

Separate Testklasse ShopRepairDeliverTest.java angelegt, dieselbe mockOrder-Hilfsmethode wie in 2.2

Tests für repair():

Leere Queue → Optional.empty()
Ein Auftrag vorhanden → wird zurückgegeben, Identität per assertSame geprüft
Zwei Aufträge → FIFO-Reihenfolge verifiziert (ältester zuerst)
Nach repair() landet Auftrag in completedOrders → deliver() findet ihn
Nach repair() kann derselbe Kunde einen neuen Auftrag einreichen (nicht mehr in pendingOrders)


Tests für deliver():

Kein passender Auftrag → Optional.empty()
Fertiggestellter Auftrag → wird zurückgegeben, Identität per assertSame geprüft
Nach Auslieferung ist Auftrag aus completedOrders entfernt (zweites deliver() → Optional.empty())
Falscher Kundenname → Optional.empty()
