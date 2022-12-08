# Arduino Programmierung
The Goal is to build knowledge inside of the Team as well as to provide a basis on which we can rely to assist the students.

## Arduino IDE
- Intro to how to upload a sketch and the buttons
- Intro to the Serial Monitor and how to setup a serial communication 

## How to use a DC motor controller 
- explain how it works (PWM)
- Explain how to use it with an arduino 
- schematics
- Code snipets to test the motors

## Servomotor
Als Servomotor bezeichnet man alle Arten von Elektromotoren, die mit einem Positionssensor für den Rotor ausgestattet sind. Diese Art von Motor ermöglicht eine sehr gute Positionskontrolle und wird sowohl in der Industrie als auch in vielen Arduino-Projekten eingesetzt.

![](/Bilder/Servomotor.jpg)

### Wie funktioniert ein Servomotor?

Ein Servomotor ist ein System mit geschlossenem Regelkreis, das seine Bewegung und Endposition mithilfe von Positionsrückmeldungen steuert. Es gibt viele Arten von Servomotoren, und ihr Hauptmerkmal ist die Fähigkeit, die Position ihrer Motorwelle präzise zu steuern.

![](/Bilder/Feedbackloop.png)

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


- Code snipets to test the motors

## How to use functions (optional)
- How to define them 
- How it helps make your code more readable and lighter

## How to avoid obstacles
- ratio distance speed of the car 
- Reaction Time 


# Ultraschall Sensor HC-SR04
Ultraschallsensoren eignen sich bestens dafür, Abstände zu messen und Objekte berührungslos zu erkennen.Wichtig ist, dass der Gegenstand aus einem Material besteht, das den Schall reflektieren kann. Objekte aus Metall, Holz oder Kunststoff eignen sich für die Messung folglich sehr gut. Transparente Flüssigkeiten oder Gegenstände sind kein Hindernis für den Sensor, dasselbe gilt für Schmutz, Staubwolken oder Nebel.

Aufgrund der nützlichen Features stellen Ultraschallsensoren auch eine bedeutsame Komponente in der Automatisierungstechnik dar. In Selbstpark- und Antikollisionssystemen kommt das Werkzeug ebenso zum Einsatz wie bei der Messung von Flüssigkeitsständen, bei der Erkennung von Etiketten und bei der Positionierung von Robotern.

## Funktionsweise
Das Prinzip ist denkbar einfach: Zunächst einmal wird ein kurzer, hochfrequenter Schallimpuls abgesendet. Sobald der Impuls auf ein Objekt stößt, kommt es zu einer Reflexion. Dies wiederum ruft ein Echo hervor, das schließlich wieder beim Sensor ankommt. Anhand der Zeitspanne, die zwischen der Abgabe und dem Empfangen des Impulses liegt, errechnet der Sensor zuverlässig den Abstand zum jeweiligen Objekt.

![](Bilder/cache_3237921.jpg.png)

## Physik/Mathematik
Die Formel für gleichförmige Bewegung lautet:
$$s=v_s*t$$
- $s$ Weg $[m]$
- $v$ Geschwindigkeit Schall $[\dfrac{m}{s}]$
- $v$ Zeit $[s]$

Wir müssen die Entfernung noch durch zwei teilen da wir nicht die gesamte Strecke ausrechnen wollen, sondern nur den Hinweg bis der Ultraschall auf ein Objekt trifft. Das ist dann die benötigte Entfernung.

$$\boxed{s=\dfrac{v_s*t}{2}}$$

Jetzt haben wir eine Formel für die Entfernung zu einem Objekt. Jedoch kennen wir $t$ und $s$ nicht. $t$ wird glückerweiße von unserem Sensor ermittelt. Die Schallgeschwindigkeit $v$ müssen wir im nächsten Schritt ermitteln.

Für die Schallgeschwindigkeit in Gasen gilt:
$$v_s=\sqrt{\dfrac{\kappa*p_u}{\rho_l}}$$
- $\kappa$ Isentropenexponent $[-]$
- $p$ Druck $[Pa]$
- $\rho$ Dichte $[\dfrac{kg}{m^3}]$

Die Gleichung lässt sich aber noch etwas handlicher gestalten. Unter der Annahme das sich die Luft in der Umgebung wie ein ideales Gas verhält gilt die Gleichung für das ideale Gas:

$$p_u * V=m * R_l * T_u$$

- $m$ Masse Luft $[kg]$
- $R_l$ spezifische Gaskonstante Luft $[\dfrac{J}{kg * K}]$
- $m$ Temperatur Umgebung $[K]$

Diese Gleichung lässt sich umstellen zu:

$$\dfrac{m}{V}=\dfrac{p}{R_l * T_U}$$

Der Ausdruck $\dfrac{m}{V}$ entsprich der Dichte $\rho_l$:

$$\rho_l=\dfrac{p_u}{R_l * T_u}$$

Dies Gleichung können wir nun in unsere Schallgeschwindigkeitsgleichung einsetzen. Diese wird dann zu:

$$v_s=\sqrt{\kappa * R_l * T_u}$$

Bei den Unbekannten $R_l$ und $\kappa$ handelt sich um Stoffwerte. Diese sind für Luft bekannt:
- $R_l=287 [\dfrac{J}{kg * K}]$
- $\kappa=1,402$

Für $0°C$ ergibt das eine Schallgeschwindigkeit von:

$$v_s=\sqrt{1,402 * 287 \dfrac{J}{kg * K} * 273K}=331,2 \dfrac{m}{s}$$

Experimentell wurde bei $0°C$ eine Schallgeschwindigkeit von $331,6 \dfrac{m}{s}$ ermittelt. Dieses Ergenis können wir nutzen um unsere Formel zu verbessern:

$$\dfrac{v_s}{331,6 \dfrac{m}{s}}=\dfrac{\sqrt{\kappa * R_l * T_u}}{\sqrt{\kappa * R_l * 273 K}}$$

Die Stoffwerte lassen sich nun kürzen. Mit $T_u=\theta_u+T_0$ dabei ist $T_0=273K$. $\theta_u$ ist die Umgebungstemperatur in $[°C]$ Diese Gleichung setzen wir in unsere bisherige Formel ein. Wenn wir dann die Formel nochmal vereinfachen dann ergibt das:

$$\boxed{v_s=331,6*\sqrt{1+\dfrac{\theta_u}{273K}}}$$

Das ist die finale Gleichung für die Schallgeschwindigkeit. Zusammen mit unserer ersten Gleichung kann man den Abstand ausrechnen.

## Technische Daten des Ultraschallsensors
- Spannungsversorgung: DC 5 V
- Stromaufnahme: 15 mA
- Ruhestrom: < 2 mA
- Burstfrequenz: 40 kHz
- Messbereich: ca. 2 cm bis 45 cm
- Auflösung: 0,3 cm HC-SR04, 0,2 cm HY-SRF05
- Messkegel: ca. 15°
- Messungen pro Sekunde: max. 50 HC-SR04, max. 20 HY-SRF05
- Abmessungen: ca. 45 * 20 * 15 mm

![](/Bilder/hc-sr04.jpg)

- Vcc -> Pluspol
- Trig -> Triggersignal
- Echo -> Echosignal 
- Gnd -> Minuspol/Grund

## Aufbau der Schaltung für den Ultraschallsensor
Die Schaltung ist relativ einfach Aufbgebaut. Der Arduino dient hier als Spannungsquelle. Wie man die einzelnen Kontakte miteinander verbindet findet ihr in dem Schaltplan.

![](/Bilder/Ultraschallsensor_Steckplatine.jpg)

## Programmierung des Ultraschallsensors 
```C
/*
 * Schulprojekt
 * Code für Ultraschallsensor hc-r04
 * Aktuell für einen ESP32
 * 
 * Alexander Huss, William Lopez, Christof Schillinger
 * Campus Schwarzwald
 */

#define TRIG_PIN 23 // Pin Trigger
#define ECHO_PIN 22 // Pin Echo

//Defnition der Variablen
float duration_us, distance_cm;
//Defintion der Umgebungstemperatur
float temperature_celsius = 20.0;

//Funktion zur Berechnung der Schallgeschwindigkeit 
float calcSpeed(float temperature_celsius) {
  return (331.6 * std::sqrt(1+((temperature_celsius)/273)));
}

//Funktion zur Berechnung der Entfernung
float calcDistance(float duration_us, float temperature_celsius){
  float v = calcSpeed(temperature_celsius);
  distance_cm = (v * (duration_us))/(2*10000);
  return (distance_cm);
}

void setup() {
  // Definiton des Ports
  Serial.begin (9600);
  // Defintion des Trigger Pin aus Ausgang
  pinMode(TRIG_PIN, OUTPUT);
  // Defintion des Echo Pin als Eingang
  pinMode(ECHO_PIN, INPUT);
}

void loop() {
  // Einen Impuls von 10 microsekunden erzeugen
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  // Speichern der Zeitdauer des Schalls 
  duration_us = pulseIn(ECHO_PIN, HIGH);
  // Berechnung der Entfernung
  distance_cm = calcDistance(duration_us, temperature_celsius);
  // Auf Montitor ausgeben
  Serial.print("Entfernung: ");
  Serial.print(distance_cm);
  Serial.println(" cm");

  delay(500);
}
```




