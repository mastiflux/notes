# Graphische Benutzeroberflächen

- können auf Basis folgender Bibliotheken gebildet werden:
	- java.awt
	- javax.swing
	- javafx

[Zum Nachlesen](https://openjfx.io/)

---
## GUI-Elemente

**Bestandteile:**

1. _Stage_

- Basisobjekt, in dem Applikationen angezeigt werden können
- Top-Level-JavaFX-Container
- _primaryStage_ wird von Plattform erzeugtund _start_ Methode übergeben   (_show_ Methode zur Darstellung)

2. _Scene_

- Applikation, die auf der Stage angezeigt wird
- Container für Inhalte, Layouts, Widgets als hierarchischer Graph von Nodes

3. _Layout pane_

- ermöglicht Anordnung vers. Komponenten 
- "Layout Node"

4. _Widgets_

- GUI-Elemente
- Textfelder, Buttons, Checkboxen, ...

---
## Skelett für JavaFX-Anwendung

```java
import javafx.application.Application; 
import javafx.scene.Scene; 
import javafx.scene.control.Button; 
import javafx.stage.Stage; 

public class Main extends Application { 
	public static void main(String[] args) { 
		launch(args); 
	} 
	@Override public void start(Stage primaryStage) { 
		Button btn = new Button("Hello World"); 
		StackPane root = new StackPane(); 
		root.getChildren().add(btn); 
		Scene scene = new Scene(root, 300, 200); 
		primaryStage.setTitle("Hello World!"); 
		primaryStage.setScene(scene); primaryStage.show(); 
	} 
}
```

![[Pasted image 20260422112629.png]]

---
## Struktur

- JavaFX GUIs werden von abstrakter Klasse _javafx.application.Application_ abgeleitet
- abstrakte _start_ Methode überschrieben -> bekommt _stage_ als Parameter -> _scene_ wird erzeugt
- Inhalt der _scene_ in Form eines Baums dargestellt (Scene Graph)       -> GUI-Komponenten werden organisiert

![[Pasted image 20260422122132.png]]

**Scene Graph**
- hierarchischer Graph, der Inhalte der _scene_ enthält
- Wurzelknoten, Verzweigungsknoten und Blattknoten

![[Pasted image 20260422122612.png]]

---
## Schritte zur eigenen JavaFX-Anwendung

1. eigene Anwendungsklasse schreiben (von _Application_ abgeleitet)
2. abstrakte Methode _start_ überschreiben
3. Scene Graph mit gewünschten Elementen (Nodes) erzeugen
4. Scene erzeugen und Wurzelknoten des Scene Graph hinzufügen
5. Scene der _primaryStage_ zuordnen und darstellen (_show_)
6. _main_-Methode schreiben, die _launch_ aufruft
7. _launch_ ruft _start_ auf

--- 

**Beispiel**

```java
public void start(Stage primaryStage) { 

BorderPane root = new BorderPane(); 
Label topLabel = new Label("Top part of root node (border pane)"); root.setTop(topLabel); 
ScrollBar leftScrollBar = new ScrollBar(); leftScrollBar.setOrientation(Orientation.VERTICAL); 
root.setLeft(leftScrollBar); 
Slider slider = new Slider(); 
root.setBottom(slider); 

VBox vbox = new VBox(); 
root.setCenter(vbox); 
Label label = new Label("Center part of root node with hierarchical levels"); HBox hbox = new HBox(); 
Button button = new Button("Start");
 
RadioButton radioButton1 = new RadioButton("Player 1"); 
RadioButton radioButton2 = new RadioButton("Player 2"); 
RadioButton radioButton3 = new RadioButton("Player 3"); hbox.getChildren().add(radioButton1); 
hbox.getChildren().add(radioButton2); 
hbox.getChildren().add(radioButton3); 
vbox.getChildren().add(label); 
vbox.getChildren().add(hbox); 
vbox.getChildren().add(button); 

Scene scene = new Scene(root, 450, 200); 
primaryStage.setScene(scene); 
primaryStage.show(); 
} 
```

![[Pasted image 20260422124018.png]]

---
## JavaFX Node

=> Basisklasse für alle Elemente im Scene-Graphen

*Wichtige Unterklassen:*
- Parent: alle Nodes, die Kinderelemente haben können
	- Region: LayoutPanes und Controls für Benutzerinteraktionen, Chart, Axis
	- Group: Kollektiver Knoten, enthält ObservableList von Kindknoten
- Shape: geometrische Formen
- Canvas: Zeichnen 2D Grafiken
- ImageView: Darstellung Bilder
- MediaView: Integration Audio, Video

**Eigenschaften:**

- Transformationen (Translation, Rotation, Skalierung)
- Bounding Rectangle
- CSS

![[Pasted image 20260422124839.png]]

---
## Layout Panes

-> zur Gruppierung mehrerer GUI-Elementen

- Anordnung und Gestaltung konfigurieren
- zum einfachen Aufbau und Management klassischer Layouts (Zeilen, Spalten, ...)
- können verschachtelt werden

_Beispiele:_

- BorderPane
- HBox
- VBox
- GridPane
- FlowPane
- StackPane

[Weiter siehe hier...](docs.oracle.com/javase/8/javafx/layout-tutorial/builtin_layouts.html)

---
_Vorbereitung Buttons erzeugen_
```java
Button[] buttonArr = new Button[16]; 
for (int i = 0; i < buttonArr.length; i++) { 
	buttonArr[i] = new Button(i + ""); 
	buttonArr[i].setPrefSize(100, 20); 
}
```
---

**VBox und HBox**

```java
HBox hbox = new HBox(); hbox.getChildren().addAll(buttonArr[0], buttonArr[1]); hbox.setStyle("-fx-background-color: yellow;"); hbox.setPadding(new Insets(15,12,15,12)); // Abstände zum umgebenden Element 
hbox.setSpacing(10); // Abstand zwischen Knoten 

VBox vbox = new VBox(); 
vbox.getChildren().addAll(buttonArr[2], buttonArr[3]); 
vbox.setStyle("-fx-background-color: green;"); // Hintergrundfarbe Übersicht
vbox.setPadding(new Insets(15,12,15,12)); 
vbox.setSpacing(10);
```
![[Pasted image 20260422130621.png]]

---

**GridPane**

-> add(n, spalte, zeile)
```java
GridPane grid = new GridPane(); grid.setHgap(10); // horizontaler Abstand der Spalten 
grid.setVgap(10); // vertikaler Abstand der Zeilen 
grid.setStyle("-fx-background-color: blue;"); 
grid.setPadding(new Insets(15,12,15,12)); 
grid.add(buttonArr[4], 0, 0); 
grid.add(buttonArr[5], 0, 1); 
grid.add(buttonArr[6], 1, 0); 
grid.add(buttonArr[7], 1, 1); 
```
![[Pasted image 20260422130831.png]]

---

**FlowPane**

```java
FlowPane flow = new FlowPane(Orientation.VERTICAL); 
flow.setStyle("-fx-background-color: red;"); 
flow.setVgap(10); // vertikaler Space zwischen Knoten 
flow.setHgap(10); //horizontaler Space zwischen Knoten 
flow.setPadding(new Insets(15,12,15,12)); // Abstände zum umgebenden Element 
for (int i = 8; i < 16; i++) { 
	flow.getChildren().add(buttonArr[i]); 
} 
```
![[Pasted image 20260422131022.png]]

---

**StackPane**

-> Elemente werden überlagert

```java
StackPane stack = new StackPane(); 
stack.getChildren().addAll(new Circle(100,Color.LIGHTSEAGREEN), new Rectangle(50, 25, Color.LEMONCHIFFON), new Label("Go!"));
```
![[Pasted image 20260422131222.png]]

---
**_Gesamt:_**
![[Pasted image 20260422131559.png]]

---
## GUI-Elemente


- Button
- Label
- CheckBox
- TextField
- MenuBar
- Circle
- Rectangle

[Weiter siehe hier...](docs.oracle.com/javase/8/javafx/user-interface-tutorial/ui_controls.html)

![[Pasted image 20260422131927.png]]

---

_Beispiele:_
![[Pasted image 20260422132205.png]]

**Button**
```java
Button button = new Button(); 
button.setText("Knopf"); 
button.setStyle("-fx-background-color: lightblue;"); 

Label label = new Label("Label"); 

CheckBox check = new CheckBox("Please check"); 

TextField textfield = new TextField(); 
textfield.setPromptText("Please write"); 
textfield.setStyle("-fx-background-color: lightyellow;"); 

MenuBar menubar = new MenuBar(); 
Menu start = new Menu("start"); 
Menu load = new Menu("load"); 
Menu save = new Menu("save"); 
menubar.getMenus().addAll(start, load, save);
```

**VBox**
```java
VBox pane = new VBox(); 

pane.getChildren().addAll(button, label, check, textfield, menubar); pane.setStyle("-fx-background-color: lightgreen;"); 
pane.setPadding(new Insets(15,12,15,12)); 
pane.setSpacing(10); Scene scene = new Scene(pane, 300,250); 

primaryStage.setTitle("UI Elements"); 
primaryStage.setScene(scene); 
primaryStage.show();
```

---
## Entwurf einer Anwendung mit GUI

- praktisch alle Anwendungen mit GUI werden nach dem ==Model-View-Controller-Pattern== aufgebaut

**Design Pattern**

- beschreibt einen bewährten Ansatz zur Lösung einer Software-Entwurfsaufgabe, die wiederholt und in verschiedenen Kontexten auftritt
- geben häufig verwendeten Lösungen einen Namen und erleichtern damit die Kommunikation
- beschreiben Details einer Lösung
- dokumentieren das Erfahrungswissen zu den Vor- und Nachteilen eines Lösungsansatzes

==Single Responsibility Principle==: Jede Entität im Entwurf hat eine einzige Aufgabe

*Beispiel Observer Pattern*

	Es gibt ein Objekt, dessen Zustandsänderungen für mehrere andere Objekte von Bedeutung sind, da diese im Falle einer Änderung ebenfalls Aktualisierungen vornehmen müssen. Die Objekte sollen aber möglichst unabhängig voneinander entwickelt werden.

- Single Responsibility Principle
- Verantwortlichkeiten
	- Modell: Fachliche Klassen: Daten und Logik („Subjekt“, „Observable“)
	- Ansicht: Darstellung der Daten („Observer“)
- Abhängigkeiten
	- Ansicht muss auf Modell zugreifen 
	- Modell kann unanbhängig von Ansicht entwickelt werden
	- vers. GUIs können auf selbem Modell arbeiten und vers. Aspekte darstellen

![[Pasted image 20260427121718.png]]

- Subject bietet Abonenntendienst an, bei dem sich Observer an- und abmelden können
- Subject schickt Benachrichtigung an alle Abonennten bei Änderungen

**Beispiel Observer Pattern in Java**

```java
public interface Subject { 
	public void registerObserver(Observer o); 
	public void removeObserver(Observer o); 
	public void notifyObserver(); 
}

public interface Observer { 
	public void update(float temp, float humidity, float pressure); 
}
```

*Wetterdaten Beispiel*

```java
public class WeatherData implements Subject { 
	
	private List<Observer> observers; 
	private float pressure; 
	private float temperature; 
	private float humidity; 
	
	public WeatherData() { 
		observers = new ArrayList<>(); 
	} 
	
	@Override 
	public void registerObserver(Observer o) { 
		observers.add(o); 
	} 
	
	@Override 
	public void removeObserver(Observer o) { 
		observers.remove(o); 
	}
	
	@Override public void notifyObserver() { 
		for (Observer obs : observers) { 
			obs.update(temperature, humidity, pressure); 
			} 
	} 
	
	public void setMeasurements(float tmp, float hum, float pres) { 
		this.temperature = tmp; 
		this.humidity = hum; 
		this.pressure = pres; 
		notifyObserver(); 
	}
}
```

```java
public class WeatherStation extends Application{ 
	
	public static void main(String[] args) { 
		launch(args); 
	} 
	
	@Override 
	public void start(Stage primaryStage) { 
		WeatherData data = new WeatherData(); 
		new TextWeatherDisplay(data); 
		new GUIWeatherDisplay(data,primaryStage); 
		for (int i = 0; i < 10; i++) { 
			delay(1000*i, data, 20+i,50*i%100,1025+2*i); 
		} 
	}
	
	public void delay(long m, WeatherData data, float f1, float f2, float f3) { 
		Task<Void> sleeper = new Task<Void>() { 
		
		@Override 
		protected Void call() throws Exception { 
			try { 
				Thread.sleep(m); 
				} catch (InterruptedException e) { 
				} 
				return null; 
			} 
		}; 
		
		sleeper.setOnSucceeded(new EventHandler<WorkerStateEvent>() { 
			@Override 
			public void handle(WorkerStateEvent event) { 
				data.setMeasurements(f1,f2,f3); 
			} 
		}); 
		
		new Thread(sleeper).start(); 
	} 
}
```

---
### Model-View-Controller (MVC)

- Aufteilung des Codes in: 
	- allgemeinen, oberflächen-unabhängigen Teil -> ==Modell== 
	- oberflächenspezifischen Teil -> ==View==

==Keine JavaFX Importe im Modell!===

**Abhängigkeiten:**

- View zeigt nur geeignete Sicht an (Auswahl der Daten des Modells)
- View agiert als Observer des Modells
- Benutzereingaben im View lösen Ereignisse aus, die der Controller behandelt 
- Controller veranlasst Änderungen am Modell
- Controller dient als Schnittstelle und agiert als Observer des Modells

![[Pasted image 20260427123911.png]]

---
#### Variante 1 - Model-View-Presenter:

- stärkere Trennung von View und Modell
- View wird nur über Presenter aktualisiert

![[Pasted image 20260427124107.png]]

#### Variante 2 - Model-View-ViewModel

![[Pasted image 20260427124157.png]]

- View und ViewModel müssen stehts synchron gehalten werden
- unabhängige Entwicklung von View (ohne Logik) und ViewModel (ohne Design)
---
## Properties

	Wrapper Klassen um normale Java-Datentypen

- IntegerProperty, StringProperty, BooleanProperty -> generische ObjectProperty
- erlauben Observer-Mechanismus über die Registrierung von Listenern, mit denen eine Reaktion auf eine Wertänderung einer Property festgelegt werden kann
- ermöglichen Synchronisation über Bindungen 
- können Events aussenden, wenn Werte sich ändern -> Event-Handler verknüpfbar

**JavaFX-Controls**

- Eigenschaften als Properties vorhanden
-> bspw. von TextField, Label usw.
	textProperty(): StringProperty
	disableProperty(): BooleanProperty 
	rotateProperty(): DoubleProperty

---
### Properties definieren

- Ersetzung primitiver Datentyp durch zugehörige Property  
- get- und set-Methode für den gekapselten Wert / Objekt definieren
- get-Methode für die Property selbst definieren (Namenskonvention)

```java
public class Raum { 
	
	private DoubleProperty temp = new SimpleDoubleProperty(); 
	
	public double getTemp() {
		return temp.get(); 
	} 
	
	public void setTemp(double temp) { 
		this.temp.set(temp); 
	} 
	
	public DoubleProperty tempProperty() { 
		return this.temp; 
	}
}
```

---
### Listener

- lassen sich an Properties registrieren
- lösen Reaktionen auf Änderungen aus (Observer-Prinzip)
- anonyme innere Klasse

```java
raum.tempProperty().addListener(new ChangeListener<Object>() { 
	@Override 
	public void changed(ObservableValue<?> wert, Object alt, Object neu) 
	{ // reagiert auf Temperaturänderung 
	}
}); 
```

---

-> Beispiel siehe [[Prog_11_JavaFX.pdf]]

---
### Bindungen

- Properties können aneinander gebunden werden "==PropertyBinding=="
- ermöglicht Synchronisation von Werten ohne Listener
- Unidirektional (a hat Einfluss auf b aber nicht andersherum) oder Driektional (a und b haben Einfluss aufeinander) möglich

**Beispiel Bruttorechner**

```java
public class BruttoRechnerViewModel { 

public BruttoRechnerViewModel(BruttoRechnerView view) { 
view.bruttoLabel.disableProperty().bind(view.nettoInput.textProperty().isEmpty()); 
view.bruttoOutput.disableProperty().bind(view.nettoInput.textProperty().isEmpty()); 

StringConverter<Double> converter = new StringConverter<Double>() { 

@Override 
public Double fromString(String arg0) { 
	Double d = 0d; 
	try { 
		d = Double.valueOf(arg0); 
	} catch (Exception e) { 
	} 
	return d; 
} 
@Override 
public String toString(Double arg0) { 
	if (arg0 == null) { 
		return ""; 
	} 
	return arg0.toString(); 
}
};
 
TextFormatter<Double> formatter = new TextFormatter<Double>(converter); 
view.nettoInput.setTextFormatter(formatter); 
DoubleProperty nettoprop = new SimpleDoubleProperty(); nettoprop.bind(formatter.valueProperty()); 
DoubleProperty taxprop = new SimpleDoubleProperty(); taxprop.bind(view.cbUmsatzsteuer.valueProperty()); 
NumberBinding brutto = Bindings.add(nettoprop,Bindings.multiply(nettoprop, taxprop).divide(100)); 
view.bruttoOutput.textProperty().bind(brutto.asString()); 

} 
}
```

![[Pasted image 20260427131320.png]]
