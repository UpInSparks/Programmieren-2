# Aufgabe 1
### Einführung von Optional<T>
In der Klasse enclosure wurde die Methode findAnimalByName
mit dem rückgabetyp Optional<T> implementiert.

find Animals by Name wurde auch in Zoo.java implementiert.

Begründung für Optional<T> in enclosure, wir nehmen Optional<T> weil das Gehege schon weiß welche Tierart
darin ist. Wenn wir ein Tier finden, ist es garantiert vom Typ T.

Begründung für optional<Animal> in zoo, der Zoo beinhält alle möglichen Gehege. Wir wissen nicht in welchem Gehege das 
gesuchte Tier auftaucht, ist Animal der einzige Anhaltspunkt für alle gehege.

# Aufgabe 2
## Command Pattern
### Afugabe 2.1 Command Interface

Generisches Interface Command<T> mit Methoden execute, undo und description.
> public interface Command<T> {
Result<ZooError, Void> execute(T target);
Result<ZooError, Void> undo(T target);
String description();
} 
> 

### Aufgabe 2.2 Konkrete Commands auf den Gehegen

`AddAnimalCommand` fügt ein Tier in der Bewohnerliste des Geheges hinzu, sofern es nicht schon existiert
Über die Methode `undo` lässt sich das hinzufügen rückgängig machen, indem das tier welches grade hinzugefügt wurde
aus dem selben Gehege wieder entfernt wird.

`RemoveAnimalCOmmand` entfernt ein ausgewähltes Tier aus dem aktuellen Gehege, kann abbrechen wenn das 
Tier nicht in diesem Gehege zu finden ist. Wenn dieser Command Rückgängig gemacht wird via `undo`
wird das Tier wieder zurück ins selbe Gehege gesetzt.

### Aufgabe 2.3 Command-Manager mit Undo/Redo
Der CommandManager steuert die Ausführung von Gehege Aktionen und managed die History
durch 2 getrennte Stacks für undo und Redo.
Managed das logging von Zoo indem man das Result von Commands abfängt und protokolliert.

## Aufgabe 3 `Result<E,R>`
das logging wird durch ein typsicheres Fehlermodell mit einem Result Interface gemacht.
Die Commands geben ein Success oder Failure Record mit dem ZooError Enum zurück.
Die Auswertung dieser Ergebnisse erfolgt im COmmandManager um das Log Level zu geben(FINE, WARNING, SEVERE)