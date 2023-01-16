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