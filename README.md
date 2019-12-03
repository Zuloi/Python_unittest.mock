# Python_unittest.mock
## mock object library



## Inhaltsverzeichnis

<!--ts-->
   * [Erklährung](#erklärung)
   * [Arten des Mocking](#arten des mocking)
   * [Einsatzgebiete](#einsatzgebiete)
   * [Installation](#installation)
   * [Quick Guide](#quick-guide)
   * [Mock Class](#mock-class)
   * [Patchers](#patchers)
   * [Anmerkungen](#anmerkungen)
<!--te-->








## Erklärung


#### Warum sollten wir in einem Test ein Mock anstelle eines echten Serviceobjekts verwenden?

Stellen Sie sich vor, dass die oben beschriebene Service-Implementierung von einer Datenbank oder einem anderen System eines Drittanbieters abhängig ist. Wir möchten nicht, dass unser Test mit der Datenbank ausgeführt wird. Wenn die Datenbank nicht verfügbar ist, schlägt der Test fehl, obwohl unser getestetes System möglicherweise völlig fehlerfrei ist. Je mehr Abhängigkeiten wir in einem Test hinzufügen, desto mehr Gründe müssen für ein Fehlschlagen des Tests verantwortlich sein. Und die meisten dieser Gründe werden die falschen sein. Wenn wir stattdessen eine Attrappe verwenden, können wir all diese potenziellen Fehler eliminieren.


Neben der Reduzierung von Fehlern reduziert das Mocken auch die Komplexität unserer Tests und spart uns somit einige Mühen. Es ist viel Code erforderlich, um ein ganzes Netzwerk korrekt initialisierter Objekte einzurichten, die in einem Test verwendet werden sollen. Mit Mocks müssen wir nur einen Mock „instanziieren“, anstatt einen ganzen Reihe von Objekten, für die das reale Objekt möglicherweise instanziiert werden muss.

Zusammenfassend möchten wir von einem potenziell komplexen, langsamen und flockigen Integrationstest zu einem einfachen, schnellen und zuverlässigen Komponententest übergehen.


#### Was ist ein Mock?

Ein Mock ist eine Attrappe, die als Platzhalter für noch nicht realisierte Objekte in der Softwareentwicklung verwendet wird, um frühzeitige Modultests zu ermöglichen. Auch wenn der Begriff „to mock“ etwas vortäuschen bedeutet, so wird ein Mock nicht in böser, sondern in guter Absicht verwendet. Beispielsweise lassen sich Mock nutzen, wenn:

- das gewünschte Objekt noch nicht zur Verfügung steht.
- das reale Objekt nicht durch einen Test beschädigt werden soll – bspw. Daten dauerhaft löscht.
- ein Verhalten nachgestellt werden soll, dass nur schwierig auszulösen ist.
- die reale Lösung zu komplex und langsam für einen Test ist – bspw. eine vollständige Datenbank, die vor jedem Test initialisiert      werden müsste.


#### Wie könnte dies Aussehen?

- Als 1. Variante haben wir eine Service-Implementierung, dies ist um zu testen am wenigsten geeignet.
- Als 2. Variante haben wir den Server gemocked und bekommen nur das zurück was wir anfragenund nicht alles.
- Als 3. Variante haben wir win Interface was zwischen der Software und eine Service-Implementierung gehängt wir somit brauchen wir keinen neuen Server der den anderen Server mocked.

![Diagramm](https://github.com/Zuloi/Python_unittest.mock/blob/master/Untitled%20Diagram%20(2).png)



## Arten des Mocking

#### Fakes
Fakes (engl. fake = Fälschung, Imitation) können ohne Framework einfach selbst implementiert werden.
Ihre Implementierung ähnelt der echten, ist aber z.B. einfacher/schneller oder gibt nur harte Werte zurück.
Beispiel: InMemory-Datenbank statt einer echten verwenden.


#### Dummy
Dummies (engl. dummy = Attrappe, Strohmann) sind Platzhalter, deren Funktion im Test eigentlich gar nicht benötigt wird.
Sie werden verwendet, um den Compiler zufriedenzustellen, da z.B. ein Objekt als Parameter erwartet wird.
Wenn die Funktionalität wirklich überhaupt nicht verwendet (=aufgerufen) wird, kann auch null verwendet werden.


#### Stubs
Stubs (engl. stub = Stummel, Stumpf) geben auf Anfragen definierte (=harte) Werte zurück, um das Verhalten des SUT vorhersagbar zu machen oder teure und fehleranfällige Zugriffe auf die Infrastruktur zu vermeiden. Außerdem dienen sie dazu, ansonsten schwer zu produzierende Zustände abzubilden, z.B. das Werfen einer Exception.
Stubs werden für in das SUT eingehende Daten verwendet.
Beispiel: Ein Repository gibt dem SUT immer den gleichen Datensatz zurück, ohne auf die Datenbank zuzugreifen.
Das Verhalten kann parametrisiert werden, z.B. für ID 1 ein bestimmter Datensatz und für andere IDs eine Exception.
Beispiel in Mockito: when(repo.getUser(1)).thenReturn(new User("Stefan"));
Einsatzgebiete: Dateisystem, DB, Netzwerk usw.
Teilt man die Methodenaufrufe seines Systems in Queries (nur lesen, keine Zustandsänderung, Rückgabewert) und Commands (Zustandsänderung, kein Ergebnis als Rückgabewert) auf, verwendet man Stubs für die Queries.
Die Tests verwenden „normale“ Assertions, um das Ergebnis des SUT zu prüfen (assert in JUnit).


#### Mocks
Mocks (engl. mock = Fäschung, Nachahmung) „merken“ sich die Methodenaufrufe an ihnen und können im Nachhinein verifizieren, ob ein Methodenaufruf stattfand, wie oft und mit welchen Parametern.
Mocks werden für aus dem SUT ausgehende Befehle verwendet.
Beispiel: Das SUT soll eine Mail verschicken und dafür am MailServer die Methode send() mit bestimmten Parametern (z.B. Adresse, Betreff) aufrufen.
Oftmals müssen die Mocks auch Daten zurückgeben, damit das SUT funktioniert. Eigentlich sollten sie das aber nicht tun. Dies weist auf eine Vermischung von Command und Query hin.
Beim Command-Query-Pattern, verwendet man Mocks für die Commands.
Die Tests verwenden keine Assertions gegen das SUT, sondern prüfen am Mock, ob die richtigen Methoden aufgerufen wurden (verify in Mockito).
Beispiel in Mockito: verify(mailServer).send("stefan@macke", "Hallo Stefan!");


#### Spy
Spies (engl. spy = Spion) sind nicht eindeutig definiert.
Ein Spy kann ein Stub mit „Aufzeichnungsfunktion“ der Interaktionen (ähnlich zum Mock) sein (siehe Test Double).
In Mockito ist ein Spy eine Art Mock zur Aufzeichnung der Interaktionen, aber mit der Möglichkeit der Delegation der Aufrufe an die echte Komponente (siehe Spy). Der Spy „umschließt“ also das echte Objekt, kann einzelne Methoden überschreiben und delegiert den Rest an das echte Objekt. Im Nachhinein kann dann noch geprüft werden, welche Methoden aufgerufen wurden.

## Einsatzgebiete
## Installation
## Quick Guide
## Mock Class
## Patchers
## Anmerkungen













