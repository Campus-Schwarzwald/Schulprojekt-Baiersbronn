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

![](/Bilder/Antrieb_Steckplatine.jpg)

## Programmierung des Antriebs
```C
/*
 * Schulprojekt
 * Code für den Antrieb
 * 
 * Alexander Huss, William Lopez, Christof Schillinger
 * Campus Schwarzwald
 */

//Defintion der Pins
#define ENA_PIN 10 //
#define IN1_PIN 9 //
#define IN2_PIN 8 //

//Funktion zur Berechung der Motordrehzahl 
float velocity(float percent){
  //Berechen der Motordrehzahl aus der Prozentangabe
  return (percent/100)*255;
}

void setup()
{
  //Die Pins als Output definieren
  pinMode(ENA_PIN, OUTPUT);    
  pinMode(IN1_PIN, OUTPUT);
  pinMode(IN2_PIN, OUTPUT);
}
void loop()
{
  //Motor soll sich mit 50% der Geschwindigkeit bewegen
  analogWrite(ENA_PIN,velocity(50));
  //Motor beginnt sich vorwärts zu drehen
  digitalWrite(IN1_PIN, HIGH); 
  digitalWrite(IN2_PIN, LOW);

  //Pause von 2,5 Sekunden
  delay(2500);

  //Motor soll sich mit 100% der Geschwindigkeit drehen
  analogWrite(ENA_PIN, velocity(100));
  //Motor beginnt sich vorwärts zu drehen
  digitalWrite(IN1_PIN, HIGH); 
  digitalWrite(IN2_PIN, LOW);

  //Pause von 2,5 Sekunden
  delay(2500);

  //Motor soll sich mit 25% der Geschwindigkeit bewegen
  analogWrite(ENA_PIN, velocity(25));
  //Motor beginnt sich rückwärts zu drehen
  digitalWrite(IN1_PIN, LOW); 
  digitalWrite(IN2_PIN, HIGH);

//Pause von 2,5 Sekunden 
  delay(2000);
}
```