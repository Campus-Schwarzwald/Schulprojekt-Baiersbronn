# Servomotor
Als Servomotor bezeichnet man alle Arten von Elektromotoren, die mit einem Positionssensor für den Rotor ausgestattet sind. Diese Art von Motor ermöglicht eine sehr gute Positionskontrolle und wird sowohl in der Industrie als auch in vielen Arduino-Projekten eingesetzt.

![](/Bilder/Servomotor.jpg)

## Wie funktioniert ein Servomotor?

Ein Servomotor ist ein System mit geschlossenem Regelkreis, das seine Bewegung und Endposition mithilfe von Positionsrückmeldungen steuert. Es gibt viele Arten von Servomotoren, und ihr Hauptmerkmal ist die Fähigkeit, die Position ihrer Motorwelle präzise zu steuern.

![](//Bilder/Feedbackloop.png)

Bei industriellen Servomotoren ist der Positionssensor in der Regel ein hochpräziser Encoder, während bei kleineren RC- oder Hobbyservos der Positionssensor meist ein einfaches Potentiometer ist. Die von diesen Geräten erfasste Ist-Position wird an den Fehlerdetektor zurückgemeldet, wo sie mit der Soll-Position verglichen wird. Entsprechend der Abweichung korrigiert der Regler dann die Ist-Position des Motors, um sie mit der Soll-Position abzugleichen.

### Wie steuert man einen Servomotor?

Ein Hobbyservo wie das, das wir in diesem Projekt verwenden, besteht aus vier Hauptkomponenten: einem Gleichstrommotor, einem Getriebe, einem Potentiometer und einem Regler. Der Gleichstrommotor hat eine hohe Drehzahl und ein geringes Drehmoment, aber das Getriebe reduziert die Drehzahl auf etwa 60 U/min und erhöht gleichzeitig das Drehmoment.

Das Potentiometer ist am letzten Zahnrad oder an der Abtriebswelle befestigt. Wenn sich der Motor dreht, dreht sich auch das Potentiometer und erzeugt so eine Spannung, die mit dem absoluten Winkel der Abtriebswelle zusammenhängt. Im Regelkreis wird diese Potentiometerspannung mit der von der Signalleitung kommenden Spannung verglichen. Bei Bedarf aktiviert der Regler eine integrierte H-Brücke, die es dem Motor ermöglicht, sich in beide Richtungen zu drehen, bis die beiden Signale eine Differenz von Null erreichen.

![](/Bilder/ServoSteuerung.png)

Ein Servomotor wird durch das Senden einer Reihe von Impulsen über die Signalleitung gesteuert. Die Frequenz des Steuersignals sollte 50 Hz betragen oder ein Impuls sollte alle 20 ms auftreten. Die Impulsbreite bestimmt die Winkelposition des Servomotors, und **diese Art von Servomotoren kann sich in der Regel um 180 Grad drehen** (sie haben eine physikalische Grenze für den Verfahrweg).


### SG90 Servomotor technische Daten

| Eigenschaften | Wert |
| ------------- | ------------- |
| Stillstandsdrehmoment	 | 1.2kg*cm |
| Betriebsspannung | 3.5V - 6V |
| Strom ohne Last   | 100mA  |
| Stillstandsstrom  | 650mA  |
| Maximale Geschwindigkeit  | 60 Grad in 0,12s |
| Gewicht  | 9g |
| Rotationsbereich  | 180Grad |

### Verwendung mit einem Arduino
![](/Bilder/ArduinoServo.jpg)

## Programmierung

```C
/*
 * Schulprojekt
 * Code für SG90 Servomotor
 * 
 * Alexander Huss, William Lopez, Christof Schillinger
 * Campus Schwarzwald
 */

#include <Servo.h>

Servo myservo;  // Erstellen eines Servo-Objekts zur Steuerung eines Servomotors

int pos = 0;    // Variable zur Speicherung der Servoposition

void setup() {
  myservo.attach(6);  // verbindet das Servomotor an Pin 6 
}

- Code snipets to test the motors
void loop() {
  for (pos = 0; pos <= 180; pos += 1) { // geht von 0 Grad bis 180 Grad in Schritten von 1 Grad
  
    myservo.write(pos);              // Das Servo soll die Position in der Variablen 'pos' anfahren.
    delay(15);                       // Es wird 15ms gewartet, bis das Servo die Position erreicht hat.
  }
  for (pos = 180; pos >= 0; pos -= 1) { // geht von 180 Grad bis 0 Grad in Schritten von 1 Grad
    myservo.write(pos);              // Das Servo soll die Position in der Variablen 'pos' anfahren.
    delay(15);                       // Es wird 15ms gewartet, bis das Servo die Position erreicht hat.
  }
}
```
