<img align="right" width="200" height="183" src="https://cloud.githubusercontent.com/assets/532272/21507867/3376e9fe-cc4a-11e6-9350-7ec4f680da36.gif">

# Python_unittest.mock
## mock object library



## Inhaltsverzeichnis

<!--ts-->
   * [Erklährung](#erklärung)
   * [Arten des Mocking](#arten-des-mocking)
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

<p align="center"><img src="https://github.com/Zuloi/Python_unittest.mock/blob/master/Untitled%20Diagram%20(2).png"></p>



## Arten des Mocking
Es gibt verschiedene Arten von Mock-Objekten:

- Ein Dummy ist ein Objekt, das im Code weitergegeben, aber nicht verwendet wird. Es wird genutzt, um Parameter mit Werten zu befüllen.
- Ein Fake ist ein teilweise implementiertes Objekt, das nur eingeschränkt einsatzfähig ist. Beispiel: eine Datenbank die Daten nicht persistent speichert.
- Ein Stub ist ein Objekt, das unabhängig von einer Eingabe / Input immer dieselbe Antwort / Output liefert.
- Ein Spy ist ein Objekt, das Aufrufe und Werte protokolliert und gegebenenfalls ausliefert.
- Ein Mock ist ein Objekt,das verwenden wird um ein gewünschtes Verhalten nachzustellen.
- Ein Shim bzw. ein „Shiv“ ist ein Objekt, das Anfragen an eine Schnittstelle abfängt und selbst antwortet.


#### Fakes
Fakes können ohne Framework einfach selbst implementiert werden. Ihre Implementierung ähnelt der echten, ist aber z.B. einfacher/schneller oder gibt nur definierte Werte zurück.
Beispiel: InMemory-Datenbank statt einer echten verwenden.



#### Dummy
Dummies sind Platzhalter, deren Funktion im Test eigentlich gar nicht benötigt wird. Sie werden verwendet, da z.B. ein Objekt als Parameter erwartet wird. Wenn die Funktionalität wirklich überhaupt nicht verwendet wird, kann auch als null verwendet werden.



#### Stubs
Stubs geben auf Anfragen definierte Werte zurück, um das Verhalten des Testing vorhersagbar zu machen oder teure und fehleranfällige Zugriffe auf die Infrastruktur zu vermeiden. Außerdem dienen sie dazu, ansonsten schwer zu produzierende Zustände abzubilden, z.B. das Werfen einer Exception. Stubs werden für in das Testing eingehende Daten verwendet.
Beispiel: Ein Repository gibt dem Testing immer den gleichen Datensatz zurück, ohne auf die Datenbank zuzugreifen.
Das Verhalten kann parametrisiert werden, z.B. für ID 1 ein bestimmter Datensatz und für andere IDs eine Exception. somit könten beide zustände getested werden.



#### Mocks
Mocks „merken“ sich die Methodenaufrufe an ihnen und können im Nachhinein verifizieren, ob ein Methodenaufruf stattfand, wie oft und mit welchen Parametern. Mocks werden für aus dem Testing ausgehende Befehle verwendet. Oftmals müssen die Mocks auch Daten zurückgeben, damit das Testing funktioniert. Eigentlich sollten sie das aber nicht tun. Die Tests verwenden keine Sicherstellung(Assertions) gegen das Testing, sondern prüfen am Mock, ob die richtigen Methoden aufgerufen wurden. Die meisten Frameworks unterscheiden nicht zwischen Stubs und Mocks. 
Beispiel: Das Testing soll eine Mail verschicken und dafür am MailServer die Methode send() mit bestimmten Parametern aufrufen.



#### Spy
Spies sind nicht eindeutig definiert. Ein Spy kann ein Stub mit „Aufzeichnungsfunktion“ der Interaktionen (ähnlich zum Mock) sein.
Ein Spy ist eine Art Mock zur Aufzeichnung der Interaktionen, aber mit der Möglichkeit der Delegation der Aufrufe an die echte Komponente. Der Spy „umschließt“ also das echte Objekt, kann einzelne Methoden überschreiben und delegiert den Rest an das echte Objekt. Im Nachhinein kann dann noch geprüft werden, welche Methoden aufgerufen wurden.

## Einsatzgebiete
Die Meinungen gehen auseinander bei der Frage, ob ein Mock auch bei einem Integrationstest nützlich sein kann. Idealerweise testet ein Integrationstest ein gesamtes System auf funktionale Vollständigkeit und Fehlerfreiheit. In der Praxis kann es jedoch immer wieder vorkommen, dass nicht alle Elemente rechtzeitig fertiggestellt wurden – hier könnte der Einsatz von Mocks Sinn ergeben.

#### Vorteile
- Tests sind nicht abhängig von änderungsanfälliger Infrastruktur.
- Tests sind einfacher, da keine komplexe Infrastruktur aufgebaut werden muss.
- Tests lassen sich jederzeit und überall wiederholbar durchführen.
- Tests sind schneller, da keine Infrastruktur berührt wird.
- Der Code wird modularer und Abhängigkeiten werden offensichtlich.
#### Nachteile
- Das Zusammenspiel der „echten“ Komponenten wird nicht getestet. Es sind zusätzliche Integrationstests nötig.
- Ncht für den Produktiven einsatzt nur um zu testen.

## Installation
```bash
pip install mock
```

## Quick Guide
## Mock Class
## Patchers
## Anmerkungen













