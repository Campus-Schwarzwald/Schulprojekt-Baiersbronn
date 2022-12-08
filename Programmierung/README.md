# Arduino Programmierung
The Goal is to build knowledge inside of the Team as well as to provide a basis on which we can rely to assist the students.

## Arduino IDE
- Intro to how to upload a sketch and the buttons
- Intro to the Serial Monitor and how to setup a serial communication 

## How to read Ultrasonic sensor Data
- explain how the sensor works 
- Fritzing schematics
- Code snipets
- display on serial monitor 

## How to use a DC motor controller 
- explain how it works (PWM)
- Explain how to use it with an arduino 
- schematics
- Code snipets to test the motors

## How to use a servo motor
- Explain how it works
- Explain how to use it with an arduino 
- schematics
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
# Antrieb
Der Antrieb des selbstfahrenden Autos besteht aus meheren Komponeten. Dazu gehört ein DC Motor, ein Motortreiber sowie der Aufhängung für den Antriebsstrang. Wir konzentrieren uns in diesem Teil auf die elektronischen Bauteile. Sprich den DC Motor mit Bürsten und den Motortreiber. Der Motortreiber wird über den Microkontroller mithilfe von einem PWM-Signal angesteuert. Der Motortreiber regelt wiederum den DC Motor.

## Was ist ein DC Motor?
Ein Gleichstrommotor, auch DC-Motor genannt, nimmt elektrischen Strom in Form von Gleichstrom (englisch direct current) auf und wandelt diesen in eine mechanische Drehbewegung (Rotation) um. Dabei macht er sich die durch den Stromfluss entstehenden Magnetfelder zunutze, um einen auf der Motorwelle sitzenden Rotor, genannt Anker, in eine Drehbewegung zu versetzen. Das Antriebsdrehmoment und die Antriebsgeschwindigkeit hängen sowohl vom Eingangsstrom als auch der Konstruktionsweise des Motors ab.

Die Bezeichnung „Gleichstrommotor“ bezieht sich auf sämtliche Rotationsmaschinen, die elektrischen Gleichstrom in mechanische Energie umsetzen. Es gibt Gleichstrommotoren in unterschiedlichen Größen und mit unterschiedlicher Leistung – von kleinen Motoren für Spielzeug und Haushaltsgeräte bis hin zu großen Konstruktionen, die Fahrzeuge, Aufzüge und Hubwerke sowie Stahlwalzwerke antreiben.

## Aufbau und Funktionsweise
Gleichstrommotoren bestehen im Wesentlichen aus zwei Komponenten: einem Feldmagneten (Stator) und einem sogenannten Anker (Rotor). Der Stator ist der unbewegliche Teil des Motors, der Anker dreht sich. Bei einem Gleichstrommotor erzeugt der Stator ein rotierendes Magnetfeld, das den Anker in eine Drehbewegung versetzt.

![](/Bilder/Bild1_neu-1024x576.jpg)

Die Grundform besteht aus einem unbeweglichen Satz Magneten im Stator und einer Drahtspule, durch die ein Strom fließt. Dadurch wird ein elektromagnetisches Feld erzeugt, das durch den Spulenkern verläuft. Eine oder mehrere Wicklungen von isoliertem Draht sind um den Kern des Motors gewickelt, um das Magnetfeld zu konzentrieren.

Die Wicklungen des isolierten Drahts sind an einen Kommutator (auch Kollektor, Stromwender oder Polwender genannt) angeschlossen, über den ein elektrischer Strom an die Wicklungen angelegt wird. Durch den Kommutator werden die Ankerspulen nacheinander erregt, wodurch eine gleichmäßige Drehkraft (Drehmoment) entsteht.

Wenn die Spulen nacheinander ein- und ausgeschaltet werden, entsteht ein wechselndes Magnetfeld, das mit den Feldkräften der stationären Feldmagneten im Stator interagiert und so ein Drehmoment erzeugt, das den Stator in eine Drehbewegung versetzt. Dieses Wirkprinzip von Gleichstrommotoren sorgt dafür, dass die elektrische Energie des Gleichstroms in mechanische Energie, die Drehbewegung, umgewandelt wird, die dann für den Antrieb von Objekten genutzt werden kann.

![](/Bilder/Animation_einer_Gleichstrommaschine_(Variante).gif)

## Technische Daten DC Motor
- Motorart: DC Motor mit Bürsten
- Anschlussspannung: 12V
- Drehzahl: 500rpm (Umdrehungen pro Minute)

## Was ist ein Motortreiber
Im Allgemeinen reichen die Stromstärken, die ein Mikrocontroller liefern kann, nicht aus, um einen Motor direkt anzusteuern. Deshalb gibt es unterschiedliche Arten von sogenannten Motortreibern. Diese werden eingangsseitig am Mikrocontroller und ausgangsseitig am Motor angeschlossen. Sie übersetzen die Kommandos des Mikrocontrollers in die vom Motor benötigten Stromstärken. Gleichzeitig wird der Mikrocontroller von etwaigen Kurzschlüssen oder Überspannungen, die seitens des Motors entstehen könnten, geschützt.

![](/Bilder/RBS10092_2.jpg)

## Technische Daten Motortreiber L298N
- Chip: L298N (ST NEU)
- Logik Spannung: 5V
- Treiber Spannung: 5V-35V
- Logik Strom: 0mA-36mA
- Treiber Strom: 2A (MAX single bridge)
- Maximale Leistung: 25W
- Abmessungen:43 x 43 x 26mm

## Steuerung des Motortreibers mittels PWM
Um die Drehzahl des DC Motors zu steueren benötigen wir einen Motortreiber, da der DC Motor eine Spannung von 12V benötigt. Diese kann nicht von dem Microcontroller bereitgestellt werden. Der Microkontroller steuert jedoch den Motortreiber und welche Spannung von dem Motortreiber an den DC Motor weitergegeben wird. Diese Spannung gibt die Drehzahl des Motors vor.

Zur Steuerung des Motortreibers wird ein PWM-Signal verwendet. PWM steht für Pulsweitenmodulation (engl. Pulse Width Modulation). Das ist ein Rechtecksignal das eine Spannung zwischen 0V und 5V annehmen kann. Fachlich gesprochen wird bei der Pulweitenmodulation, das Verhältnis zwischen der Einschaltzeit und Periodendauer eines Rechtecksignals bei fester Grundfrequenz variiert. 

![](/Bilder/Bild_PWM.jpg)

Die Einschaltzeit $t\textsubscript{ein}$





