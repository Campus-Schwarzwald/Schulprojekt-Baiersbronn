# :computer: Programmierung


> **Warning**
> Bevor ihr beginnt, vergewissert euch, dass die folgenden Bibliotheken auf eurem Raspberry Pi Zero W installiert sind:
> - GPIO
> - gpiozero
> - time

# Raspberry Pi Zero

![](/Bilder/RpiPinout.png)

# Servomotor
Als Servomotor bezeichnet man alle Arten von Elektromotoren, die mit einem Positionssensor für den Rotor ausgestattet sind. Diese Art von Motor ermöglicht eine sehr gute Positionskontrolle und wird sowohl in der Industrie als auch in vielen Arduino-Projekten eingesetzt.

![](/Bilder/Servomotor.jpg)

## Wie funktioniert ein Servomotor?

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
![](/Bilder/RpiServo.png)

    ```Python
    ###
    # Schulprojekt 2023
    # Code für SG90 Servomotor und raspberry pi Zero W
    # 
    # Alexander Huss, William Lopez, Christof Schillinger
    # Campus Schwarzwald
    ###

    from gpiozero import Servo
    from time import sleep

    # Servo-Objekt erstellen, Pin-Nummer angeben (hier GPIO17)
    servo = Servo(17)

    while True:
        # Servo auf die minimale Position bewegen 0°
        servo.min()

        # 1 Sekunde warten
        sleep(1)

        # Servo auf die maximale Position bewegen 180°
        servo.max()

        # 1 Sekunde warten
        sleep(1)

        # Servo auf die mittlere Position bewegen 90°
        servo.mid()

        # 1 Sekunde warten
        sleep(1)

        # Servo von 10° bewegen 
        servo.step = 10

        # 1 Sekunde warten
        sleep(1)

        # Servo auf eine benutzerdefinierte Position bewegen (hier: 0.5)
        servo.value = 0.5

        # 1 Sekunde warten
        sleep(1)

        # Servo auf die mittlere Position zurückbewegen
        servo.mid()

        # GPIO-Schnittstelle bereinigen
        servo.close()
    ```


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

## Raspberry Pi
![](/Bilder/Raspberry_Pi_Ultraschallsensor_Steckplatine.png)

## Programmierung des Ultraschallsensors 
```Python
"""
Schulprojekt
Code für Ultraschallsensor hc-r04
Aktuell für einen Raspberry Pi
 
Alexander Huss, William Lopez, Christof Schillinger
Campus Schwarzwald
"""
import RPi.GPIO as GPIO
import time
import math

# Setze die Pin-Nummern für den Ultraschallsensor fest
TRIG_PIN = 17
ECHO_PIN = 18

# Festlegen der Umgbungstemperatur
temperature_ambient = 23 

# Initialisiere die GPIO-Pins
GPIO.setmode(GPIO.BCM)
GPIO.setup(TRIG_PIN, GPIO.OUT)
GPIO.setup(ECHO_PIN, GPIO.IN)

# Funktion zur Berechnung der Entfernung auf Basis der Umgegbungstemperatur
def calculateDistance(temperature_ambient, pulse_duration):
  v = 311.6 * math.sqrt(1 + (temperature_ambient/273))
  distance_cm = (v * (duration_us))/(2*10000)
  return round(distance_cm, 2)

def distance():
    # Sende einen Trigger-Impuls von 10us an den Sensor
    GPIO.output(TRIG_PIN, GPIO.HIGH)
    time.sleep(0.00001)
    GPIO.output(TRIG_PIN, GPIO.LOW)
    
    # Warte auf das ECHO-Signal und berechne die Dauer
    pulse_start = time.time()
    while GPIO.input(ECHO_PIN) == GPIO.LOW:
        pulse_start = time.time()
    pulse_end = time.time()
    while GPIO.input(ECHO_PIN) == GPIO.HIGH:
        pulse_end = time.time()
    pulse_duration = pulse_end - pulse_start
    
    # Berechne die Entfernung 
    distance = calculateDistance(temperature_ambient, pulse_duration)
    
    return distance

# Endlosschleife zur kontinuierlichen Messung
while True:
    dist = distance()
    print("Distance:", dist, "cm")
    time.sleep(1)

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

Die Einschaltzeit $t_{ein}$ und die Periodendauer $T=t_{ein}+t_{aus}$ wird als Tastenverhältnis $p$ bezeichnet. Das Tastenverhältnis kann Werte von $0-100\percent$ annehmen.

$$p=\dfrac{t_{ein}}{T}=\dfrac{t_{ein}}{t_{ein}+t_{aus}}$$

Der zeitliche Mittelwert der Spannung $U$ im Intervall $[0,T]$ berechent sich wie folgt:

$$U_m=U_{aus}+(U_{ein}-U_{aus}) * \dfrac{t_{ein}}{t_{ein}+t_{aus}}$$

Davon ausgehend das $U_{aus}=0V$ lässt sich die Formel zu folgendem Ausdruck vereinfachen:

$$U_m=U_{ein} * \dfrac{t_{ein}}{t_{ein}+t_{aus}}=U_{ein} * p$$

Die Mittlere Spanung $U_m$ ist die Spannung zwischen Microcontroller und Motortreiber. Abhänig von dieser Spannung steuert der Motortreiber die Spannung von dem Motor. Einfach gesagt: Je höher die Spannung $U_m$ ist desto schneller dreht sich der Motor.

## Aufbau Schaltung für den Antrieb
Das Bild zeigt die Schaltung des Antriebs

![](/Bilder/RpiL298n.png)

## Programmierung des Antriebs
```Python
###
    # Schulprojekt 2023
    # Code für DC-Motor und raspberry pi Zero W
    # 
    # Alexander Huss, William Lopez, Christof Schillinger
    # Campus Schwarzwald
###

import RPi.GPIO as GPIO
from time import sleep

# Definition der Pins
ENA = 18
IN1 = 23
IN2 = 24
ENB = 19
IN3 = 20
IN4 = 21

# GPIO-Modus festlegen
GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)

# Pins als Ausgänge konfigurieren
GPIO.setup(ENA, GPIO.OUT)
GPIO.setup(IN1, GPIO.OUT)
GPIO.setup(IN2, GPIO.OUT)
GPIO.setup(ENB, GPIO.OUT)
GPIO.setup(IN3, GPIO.OUT)
GPIO.setup(IN4, GPIO.OUT)

# PWM-Frequenz festlegen
pwm_freq = 500

# PWM-Objekte erstellen
pwm_a = GPIO.PWM(ENA, pwm_freq)
pwm_b = GPIO.PWM(ENB, pwm_freq)

# Funktionen zur Motorsteuerung definieren
def motor_a(speed, direction):
    GPIO.output(IN1, direction)
    GPIO.output(IN2, not direction)
    pwm_a.start(speed)

def motor_b(speed, direction):
    GPIO.output(IN3, direction)
    GPIO.output(IN4, not direction)
    pwm_b.start(speed)

def stop_motors():
    pwm_a.stop()
    pwm_b.stop()

# Beispielbewegungen durchführen
motor_a(50, True)   # Motor A mit 50% Geschwindigkeit vorwärts bewegen
motor_b(50, False)  # Motor B mit 50% Geschwindigkeit rückwärts bewegen
sleep(2)
stop_motors()       # beide Motoren stoppen

motor_a(75, False)  # Motor A mit 75% Geschwindigkeit rückwärts bewegen
motor_b(75, True)   # Motor B mit 75% Geschwindigkeit vorwärts bewegen
sleep(2)
stop_motors()       # beide Motoren stoppen

# GPIO-Schnittstelle bereinigen
GPIO.cleanup()
```
