## Aufgabe 3.
### 3.1: Generics
- Wo helfen Ihnen die Generics im Zoo-Szenario, Fehler bereits zur Compile-Zeit zu vermeiden?

Generics garantieren, dass Tiere nicht im falschen Gehege sein können da Gehege nur Tiere vom eigenen Typ und dessen Kinder akzeptieren.
z.B. kann eine Katze nicht im Aquarium sein

- Nennen Sie ein Beispiel aus Ihrer Implementierung, bei dem falsche Tier-Gehege-Kombinationen durch den Typ-Checker verhindert werden.

> public List<Mammal> getAllMammals() {
return enclosures.stream()
.flatMap(e -> e.getInhabitants().stream())
.filter(Mammal.class::isInstance)
.map(Mammal.class::cast)
.toList();
} 

Diese Methode durchsucht mithilfe der Java-Stream-API alle Gehege des Zoos um eine typsichere Liste aller enthaltenen Säugetiere zurückzugeben.
enclosures.stream() wandelt die liste in einen stream, der via .flatMap() die bewohnerlisten der gehege aufteilt und in einen gemeinsamen pakt.
via .filter(Mammal.class::isInstance) filtert alle nicht säugetiere aus.
.map(Mammal.class::cast) die übrigen Tierobjects werden für den typechecker in den Mammal typ umgewandelt.
der Datenstrom wird mit .toList() als fertige List<Mammal> gesammelt.
Ohne .filter(Mammal.class::isInstance) können fische oder vögel in die Liste eindringen.

### 3.2: Logging
- Warum ist systematisches Logging mit einem Logger und Log-Leveln für ein Zoo-Management-System sinnvoller als Ausgaben mit IO.println?

Ohne Logger wird nur in die Konsole ausgegeben mit println, damit ist das was ausgegeben wird nicht realistisch nutzbar.

Ein Logger erlaubt Konfiguration ohne den Code zu verändern.
Metadaten kommen mit Zeitstempel,Klassennamen und Thread-information.
Logs können auf die Konsole, in eine Datei oder an eine andere stelle geschickt werden.

- In welchen Situationen würden Sie in Ihrem System die Log-Level `INFO`, `WARNING` und ggf. `SEVERE` verwenden?
    - `INFO`
reguläre Informationen, und erwartete Ausgaben.
beispiel: "Aquarium erfolgreich registriert."
    - `WARNING`
Unerwartete Informationen und Situationen, bei den das System weiterläuft ohne absturz.
Beispiel: findEnclosureByName("Frankfurter-Hauptbahnhof") liefert null
Gibt es in diesem zoo nicht, aber es führt nicht zum absturz.
    - `SEVERE`
Kritische Fehler, Daten oder Systemzustände die sofortige Aufmerksamkeit brauchen.
Beispiel: threshold < 0 in getOvercrowdedEnclosures, ist nicht möglich

### 3.3: Streams
Vorteile:
Weniger Code
Beispiel: CountAnimalsByType() mit einer normalen Schleife müsste man eine map machen durch gehege loopen, 
durch die tiere loopen, dann prüfen ob die Klasse in der map schon existiert einen Counter erhöhen und so weiter
Mit Streams wirds durch Collectors.groupingBy kurz zusammen gepackt.

Nachteile:
Es kann unübersichtlich werden. Wenn Enclosure<?> mit einem unbekannten Typ mit flatMap nutzen wollen, verliert Java den Typ-Zusammenhang.
Debugging ist schwerer als in einer klassischen for schleife, wenn man aus flatMap, filter und map einen Fehler zufinden.
