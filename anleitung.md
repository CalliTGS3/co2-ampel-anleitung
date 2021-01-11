# CO2 Ampel

```package
devices
SCD30=https://github.com/CalliTGS3/pxt-SCD30
```
## ~avatar avatar @unplugged

Wir bauen mit dem @boardname@ ein CO2 Messgerät. Dazu benötigen wir einen Sensor, den wir Anschluss A0 des @boardname@ anschliessen. Der @boardname@ zeigt uns die CO2 Konzentration in der Raumluft an,
die Messung efolgt sooft wir das wollen. Wir überlegen uns, wie wir den Wert mithilfe von einfach zu erkennenden Symbolen auf der 5X5 LED Matrix des @boardname@ anzeigen können.


## Schritt 1 @fullscreen

Wir speichern den CO2 Wert in einer Variablen. Erstelle dazu eine Variable namens ``||Variables:CO2||``, indem du auf den Bereich ``||Variables:Variablen||`` klickst. <br>


## Schritt 2 @fullscreen

Wir die bereits vorhandene unendliche Schleife ``||basic:dauerhaft||``, darin messen wir den CO2 - Wert und zeigen ihn an.
Nimm aus der Toolbox aus dem Block ``||Variables:Variablen||``Variablen die Zuweisung ``||Variables:setze CO2 auf||`` und setze ihn in
die ``||basic:dauerhaft||`` Schleife. Setze aus der Toolbox den Block ``||SCD30:CO2||`` als Wert ein. 

```blocks
basic.forever(function () {
    CO2 = SCD30.readCO2()
})
```


## Schritt 3 @fullscreen

Wir programmieren eine Funktion zum Anzeigen des gemessenen CO2 Wertes, weil wir immer die gleichen Programmcode für die Anzeige aufrufen und unser Programm so viel übersichtlicher wird.. Erstelle eine Funktion ``||functions:ZeigeCO2||`` und rufe die Funktion unmittelbar nach der Messung des CO2 Wertes auf.
In der Funktion ist eine Entscheidung 
Ergänze die Funktion selbständig für den Bereich 1000ppm bis 2000ppm und 2000ppm bis 3000ppm, indem Du zusätzliche wenn - dann - Zweige mit dem PLUS einfügst .


```blocks
function ZeigeCO2 {
    if (CO2 < 1000) {
        basic.showLeds(`
            . . . . .
            . . . . .
            . . # . .
            . . . . .
            . . . . .
            `)
    } if (CO2 < 2000) {
        basic.showLeds(`
            . . . . .
            . # # # .
            . # # # .
            . # # # .
            . # # # .
            `)
    }
    } else {
        basic.showLeds(`
            # # # # #
            # # # # #
            # # # # #
            # # # # #
            # # # # #
            `)
    }
}
```


## Schritt 4 @fullscreen

Wir möchten, daß die Anzeige blinkt und so programmieren wir jeden einzelnen Zweig in der Funktion Zeige CO2   


## Schritt 5 @fullscreen

Die Schleife wird jetzt sehr oft abgearbeitet, weil unser @boardname@ sehr schnell arbeitet, ein paar hundert Mal pro Sekunde.
So oft brauchen wir die Messung gar nicht und deshalb lassen wir den @boardname@ in jeder Schleife einige Zeit warten.
Wir wollen nur einmal pro Minute messen und das deshalb muss unser @boardname@ 60*1000ms (1ms ist eine tausendstel Sekunde) = 60000 msec warten.


## Schritt 6 @fullscreen

Wir möchten mit einem Knopfdruck auf Taste A, daß wir auf der Anzeige des @boardname@ den Wert des CO2 Sensors sehen, um zu prüfen, daß unser Meßgerät richtig arbeitet.
Dazu nimmst Du den Block ``||input:wenn Knopf A gedrückt||`` aus dem Bereich ``||input:Eingaben||`` und setzt ihn an einer beliebigen Stelle im Programmdenster rechts.
Nimm aus der Toolbox aus dem Block ``||Variables:Variablen||``Variablen die Zuweisung ``||Variables:setze CO2 auf||`` und setze ihn in
die ``||basic:dauerhaft||`` Schleife. Setze aus der Toolbox den Block ``||SCD30:CO2||`` als Wert ein. 

```blocks
input.onButtonPressed(Button.A, function () {
    CO2 = SCD30.readCO2()
})
```


## Schritt 7 @fullscreen

Für die Anzeige des CO2 - Wertes benutzt Du den ``||basic:zeige Zahl||`` Block.

```blocks
input.onButtonPressed(Button.A, function () {
    CO2 = SCD30.readCO2()
    basic.showNumber(CO2)
})
```


## Schritt 8 @fullscreen

Schliesse Deinen @boardname@ mit einem USB Kabel an und drücke auf ``|Herunterladen|``. Speichere Dein Programm auf dem Laufwerk **@drivename@**. 
Damit wird Dein Programm zum @boardname@ übertragen.


## Schritt 8

Gut gemacht! Du hast ein CO2 Messgerät für den @boardname@ programmiert.
Verlasse diese Anleitung, indem Du auf ``|Fertigstellen|`` klickst. 

