# CO2 Ampel

## ~avatar avatar @unplugged

Wir bauen mit dem @boardname@ eine CO2 Ampel für die Raumluft. 
Dazu benötigen wir einen CO2 - Sensor, den wir am Anschluss A0 des @boardname@ anschliessen. 
Der @boardname@ zeigt uns die CO2 - Konzentration in der Raumluft an, die Messung erfolgt so oft wie wir das wollen. 
Wir überlegen uns, wie wir den Wert mit Hilfe von einfach zu erkennenden Ampel - Symbolen auf der 5X5 LED Matrix des @boardname@ anzeigen können.

![Sensor am Calliope mini anschliessen](https://github.com/CalliTGS3/co2-ampel-anleitung/blob/master/SCD30.jpg?raw=true)


## Schritt 1 @fullscreen

Wir speichern den gemessenen CO2 - Wert in einer Variablen. Erstelle dazu eine Variable mit dem Namen ``||Variables:CO2||``, indem Du auf den Bereich ``||Variables:Variablen||`` klickst.


## Schritt 2 @fullscreen

Wir benutzen die bereits vorhandene unendliche Schleife ``||basic:dauerhaft||``, darin messen wir den CO2 - Wert und zeigen ihn an.
Nimm aus der Toolbox aus dem Block ``||Variables:Variablen||`` die Zuweisung ``||Variables:setze CO2 auf||`` und setze ihn in
die ``||basic:dauerhaft||`` Schleife. Setze aus der ``||SCD30:SCD30||`` Toolbox den Block ``||SCD30:CO2 Wert||`` als Wert ein. 

```blocks
basic.forever(function () {
    let CO2 = SCD30.readCO2()
})
```


## Schritt 3 @fullscreen

Wir programmieren eine Funktion zum Anzeigen des gemessenen CO2 - Wertes, weil wir immer den gleichen Programmcode 
für die Anzeige aufrufen und unser Programm so viel übersichtlicher wird. 
Erstelle eine Funktion ``||Functions:ZeigeCO2||``.
Setze in die Funktion einen Entscheidung-Block ``||Logic: wenn <wahr> dann .. ansonsten ..||`` ein, um den CO2 Wert zu prüfen 
und daraus ein Ampel - Symbol für die 5x5 LED Matrix zu erzeugen.
Wir haben leider keine Farben in der 5x5 LED Matrix verfügbar und so stellen wir die Ampel mit unterschiedlich großen Quadraten dar.  
Benutze dafür den Block ``||basic:Zeige LEDs||`` und klicke die LEDs an, die aufleuchten sollen.
Ergänze die Funktion selbständig für den Bereich 1000ppm bis 2000ppm und 2000ppm bis 3000ppm, 
indem Du zusätzliche wenn - dann - Zweige mit dem PLUS einfügst .

```blocks
function ZeigeCO2 () {
    if (CO2 < 1000) {
        basic.showLeds(`
            . . . . .
            . . . . .
            . . # . .
            . . . . .
            . . . . .
            `)
    } else {
        if (CO2 < 2000) {
            basic.showLeds(`
                . . . . .
                . # # # .
                . # # # .
                . # # # .
                . . . . .
                `)
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
}

basic.forever(function () {
    let CO2 = SCD30.readCO2()
})
```


## Schritt 4 @fullscreen

Wir möchten, daß die Anzeige blinkt, um Batteriestrom zu sparen. 
Erweitere dazu die Funktion ``||Functions:ZeigeCO2||`` nach der Entscheidung 
um eine Wartezeit nach der Anzeige und das Löschen der 5x5 LED Matrix. Damit blitzen die LED's nur solange auf, wie Du vorgibst!
Experimentiere mit verschiedenen Wartezeiten, bis Dir die Anzeige gefällt! 
Die Wartezeit programmierst Du mit ``||basic:pausiere (ms)||`` und 100 ms und das Löschen der LED Matrix mit ``||basic:Bildschirminhalt löschen||``

```blocks
function ZeigeCO2 () {
    if (CO2 < 1000) {
        basic.showLeds(`
            . . . . .
            . . . . .
            . . # . .
            . . . . .
            . . . . .
            `)
    } else {
        if (CO2 < 2000) {
            basic.showLeds(`
                . . . . .
                . # # # .
                . # # # .
                . # # # .
                . . . . .
                `)
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
    basic.pause(100)    
    basic.clearScreen()
}

basic.forever(function () {
    let CO2 = SCD30.readCO2()
})
```


## Schritt 5 @fullscreen

Die Hauptschleife des Programmes wird jetzt sehr oft abgearbeitet, weil unser @boardname@ sehr schnell ist, ungefähr ein paar hundert Mal pro Sekunde.
So oft brauchen wir die Messung gar nicht und deshalb bauen wir eine zusätzliche Schleife unmittelbar nach der Messung ein,
in der der @boardname@ die Anzeige Funktion aufruft und eine Sekunde wartet. Wenn wir diese Schleife so programmieren, daß sie 60 Mal
durchlaufen wird, vergehen die Zeit für die Anzeige (100 ms) + die Wartezeit (900 ms) = 1 sec 60 mal, also ungefähr eine Minute
bis zur nächsten Messung des CO2 Wertes.  
Wir rufen in der Schleife die Funktion ``||Functions:ZeigeCO2||`` auf und für das Warten verwenden wir den Block ``||basic:pausiere (ms)||`` und tragen als Wert 900 ein.

```blocks
basic.forever(function () {
    let CO2 = SCD30.readCO2()
    for (let index = 0; index < 60; index++) {
        ZeigeCO2()
        basic.pause(900)
    }
})
```


## Schritt 6 @fullscreen

Um nicht immer eine Minute auf die nächste Messung warten zu müssen, programmieren wir unseren @boardname@ so, daß beim Drücken der Taste A
der CO2 Sensor sofort abgefragt wird. Dazu nimmst Du den Block ``||Input:wenn Knopf A gedrückt||`` aus dem Bereich ``||Input:Eingaben||`` und setzt ihn an einer beliebigen Stelle im Programmfenster rechts.
Nimm aus der Toolbox aus dem Block ``||Variables:Variablen||`` die Zuweisung ``||Variables:setze CO2 auf||`` und setze ihn in
den ``||Input:wenn Knopf A gedrückt||`` Block. Setze in die Variablenzuweisung aus der ``||SCD30:SCD30||`` Toolbox den Block ``||SCD30:CO2 Wert||`` als Wert ein. 

```blocks
input.onButtonPressed(Button.A, function () {
    CO2 = SCD30.readCO2()
})
```


## Schritt 7 @fullscreen

Um zu prüfen, ob unsere Ampel richtig arbeitet, programmieren wir eine Funktion, die uns den aktuellen CO2 - Wert als Zahl anzeigt.
Wir möchten mit einem Knopfdruck auf Taste B den Wert des CO2 Sensors auf der Anzeige des @boardname@ sehen.
Dazu nimmst Du den Block ``||Input:wenn Knopf B gedrückt||`` aus dem Bereich ``||Input:Eingaben||`` und setzt ihn an einer beliebigen Stelle im Programmfenster rechts.
Für die Anzeige des CO2 - Wertes benutzt Du den ``||basic:zeige Zahl||`` Block und wählst die ``||Variables:CO2||`` aus.

```blocks
input.onButtonPressed(Button.B, function () {
    basic.showNumber(CO2)
})
```


## Schritt 8 @fullscreen

Du kannst die Anzeigefunktion noch um einen Alarmton erweitern. Benutze dazu den Block ``||Music:spiele Note||`` oder eine andere
Musikfunktion. Probiere verschiedene Töne aus! Verwende die Alarmfunktion mit Bedacht, wir wollen ja niemanden stören, wenn "die Luft rein ist".

```blocks
function ZeigeCO2 () {
    if (CO2 < 1000) {
        basic.showLeds(`
            . . . . .
            . . . . .
            . . # . .
            . . . . .
            . . . . .
            `)
    } else {
        if (CO2 < 2000) {
            basic.showLeds(`
                . . . . .
                . # # # .
                . # # # .
                . # # # .
                . . . . .
                `)
        } else {
            basic.showLeds(`
                # # # # #
                # # # # #
                # # # # #
                # # # # #
                # # # # #
                `)
            music.playTone(262, music.beat(BeatFraction.Half))
        }
    }
    basic.pause(100)    
    basic.clearScreen()
}
```


## Prüfe Dein Programm @fullscreen

Und so sollte jetzt Dein gesamtes Programm aussehen:

```blocks
input.onButtonPressed(Button.A, function () {
    CO2 = SCD30.readCO2()
})
input.onButtonPressed(Button.B, function () {
    basic.showNumber(CO2)
})
function ZeigeCO2 () {
    if (CO2 < 1000) {
        basic.showLeds(`
            . . . . .
            . . . . .
            . . # . .
            . . . . .
            . . . . .
            `)
    } else {
        if (CO2 < 2000) {
            basic.showLeds(`
                . . . . .
                . # # # .
                . # # # .
                . # # # .
                . . . . .
                `)
        } else {
            basic.showLeds(`
                # # # # #
                # # # # #
                # # # # #
                # # # # #
                # # # # #
                `)
            music.playTone(262, music.beat(BeatFraction.Quarter))
        }
    }
    basic.pause(100)
    basic.clearScreen()
}
basic.forever(function () {
    CO2 = SCD30.readCO2()
    for (let index = 0; index < 60; index++) {
        ZeigeCO2()
        basic.pause(1000)
    }
})
```


## Schritt 9 @fullscreen

Schliesse Deinen @boardname@ mit einem USB Kabel an und drücke auf ``|Herunterladen|``. Speichere Dein Programm auf dem Laufwerk **@drivename@**. 
Damit wird Dein Programm zum @boardname@ übertragen.


## Schritt 10

Gut gemacht! Du hast ein CO2 Messgerät für den @boardname@ programmiert.
Verlasse diese Anleitung, indem Du auf ``|Fertigstellen|`` klickst. 

```package
SCD30=github:callitgs3/pxt-scd30#v0.9.0
```
