# CO2 Ampel

```package
devices
bluetooth
SCD30=github:callitgs3/pxt-scd30#f1c135e2c8328f4210eed8bb8828b69537bb14fb
```

## ~avatar avatar @unplugged

Wir bauen mit dem @boardname@ eine CO2 Ampel für die Raumluft. 
Dazu benötigen wir einen CO2 - Sensor, den wir am Anschluss A0 des @boardname@ anschliessen. 
Der @boardname@ zeigt uns die CO2 - Konzentration in der Raumluft an, die Messung efolgt so oft wie wir das wollen. 
Wir überlegen uns, wie wir den Wert mit Hilfe von einfach zu erkennenden Ampel - Symbolen auf der 5X5 LED Matrix des @boardname@ anzeigen können.


## Schritt 1 @fullscreen

Wir speichern den gemessenen CO2 - Wert in einer Variablen. Erstelle dazu eine Variable namens ``||Variables:CO2||``, indem Du auf den Bereich ``||Variables:Variablen||`` klickst.


## Schritt 2 @fullscreen

Wir benutzen die bereits vorhandene unendliche Schleife ``||basic:dauerhaft||``, darin messen wir den CO2 - Wert und zeigen ihn an.
Nimm aus der Toolbox aus dem Block ``||Variables:Variablen||`` die Zuweisung ``||Variables:setze CO2 auf||`` und setze ihn in
die ``||basic:dauerhaft||`` Schleife. Setze aus der Toolbox den Block ``||SCD30:CO2||`` als Wert ein. 

```blocks
basic.forever(function () {
    let CO2 = SCD30.readCO2()
})
```


## Schritt 3 @fullscreen

Wir programmieren eine Funktion zum Anzeigen des gemessenen CO2 - Wertes, weil wir immer den gleichen Programmcode für die Anzeige aufrufen und unser Programm so viel übersichtlicher wird. 
Erstelle eine Funktion ``||Functions:ZeigeCO2||`` und rufe die Funktion unmittelbar nach der Messung des CO2 Wertes auf.
Setze in die Funktion einen Entscheidung-Block ``||Logic: wenn <wahr> dann .. ansonsten ..||`` ein, um den CO2 Wert zu prüfen und daraus ein Ampel - Symbol für die 5x5 LED Matrix zu erzeugen.
Wir haben leider keine Farben in der 5x5 LED Matrix verfügbar und so stellen wir die Ampel mit unterschiedlich großen Quadraten dar.  
Ergänze die Funktion selbständig für den Bereich 1000ppm bis 2000ppm und 2000ppm bis 3000ppm, indem Du zusätzliche wenn - dann - Zweige mit dem PLUS einfügst .

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
    ZeigeCO2()
})
```


## Schritt 4 @fullscreen

Wir möchten, daß die Anzeige blinkt, um Batteriestrom zu sparen. 
Erweitere dazu jeden einzelnen Zweig in der Funktion ``||functions:ZeigeCO2||``
um eine Wartzeit nach der Anzeige, das Löschen der 5x5 LED Matrix und einer zusätzlichen Wartezeit.
Experimentiere mit verschiedenen Wartezeiten, bis Dir die Anzeige gefällt!

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
        basic.pause(100)    
        basic.clearScreen()
        basic.pause(900)    
    }
}
```


## Schritt 5 @fullscreen

Die Schleife wird jetzt sehr oft abgearbeitet, weil unser @boardname@ sehr schnell ist, ungefähr ein paar hundert Mal pro Sekunde.
So oft brauchen wir die Messung gar nicht und deshalb lassen wir den @boardname@ in jeder Schleife einige Zeit warten.
Eine Wartezeit wird auf dem @boardname@ in Millisekunden (1 ms ist eine tausendstel Sekunde) angegeben.
Wir wollen nur einmal pro Minute messen und deshalb muss unser @boardname@ 60*1000ms = 60000 msec warten.
Dazu benutzen wir den Block ``||basic:pausiere (ms)||`` und tragen als Wert 60000 ein.

```blocks
basic.forever(function () {
    let CO2 = SCD30.readCO2()
    ZeigeCO2()
    basic.pause(60000)
})
```


## Schritt 6 @fullscreen

Um zu prüfen, ob unsere Ampel richtig arbeitet, programmieren wir eine Funktion, die uns den CO2 Wert als Zahl anzeigt.
Wir möchten mit einem Knopfdruck auf Taste A den Wert des CO2 Sensors auf der Anzeige des @boardname@ sehen.
Dazu nimmst Du den Block ``||Input:wenn Knopf A gedrückt||`` aus dem Bereich ``||Input:Eingaben||`` und setzt ihn an einer beliebigen Stelle im Programmfenster rechts.
Nimm aus der Toolbox aus dem Block ``||Variables:Variablen||`` die Zuweisung ``||Variables:setze CO2 auf||`` und setze ihn in
den ``||Input:wenn Knopf A gedrückt||`` Block. Setze in die Variablenzuweisung aus der Toolbox den Block ``||SCD30:CO2||`` als Wert ein. 

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
        basic.pause(100)    
        basic.clearScreen()
        basic.pause(900)    
    } else {
        if (CO2 < 2000) {
            basic.showLeds(`
                . . . . .
                . # # # .
                . # # # .
                . # # # .
                . . . . .
                `)
            basic.pause(100)    
            basic.clearScreen()
            basic.pause(900)    
        } else {
            basic.showLeds(`
                # # # # #
                # # # # #
                # # # # #
                # # # # #
                # # # # #
                `)
            music.playTone(262, music.beat(BeatFraction.Half))
            basic.pause(100)    
            basic.clearScreen()
            basic.pause(900)    
        }
    }
}
```


## Schritt 8 @fullscreen

Schliesse Deinen @boardname@ mit einem USB Kabel an und drücke auf ``|Herunterladen|``. Speichere Dein Programm auf dem Laufwerk **@drivename@**. 
Damit wird Dein Programm zum @boardname@ übertragen.


## Schritt 8

Gut gemacht! Du hast ein CO2 Messgerät für den @boardname@ programmiert.
Verlasse diese Anleitung, indem Du auf ``|Fertigstellen|`` klickst. 
