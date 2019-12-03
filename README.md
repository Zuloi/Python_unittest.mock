# Python_unittest.mock
## mock object library



## Inhaltsverzeichnis

<!--ts-->
   * [Erklährung](#erklärung)
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

![Diagramm]()

## Einsatzgebiete
## Installation
## Quick Guide
## Mock Class
## Patchers
## Anmerkungen













