# QT

<!-- MarkdownTOC autolink=true -->

- [Qt Modules](#qt-modules)
- [.pro](#pro)
- [Concrete QWidget](#concrete-qwidget)
- [UIC](#uic)
- [MOC \(meta-object compiler\)](#moc-meta-object-compiler)
	- [Signal](#signal)
	- [Slots](#slots)
	- [slot/signal in Qt 4](#slotsignal-in-qt-4)
	- [QObject](#qobject)
- [UI](#ui)
	- [Stylesheet](#stylesheet)
	- [Layout](#layout)
		- [1. Anchor](#1-anchor)
		- [2. Layout](#2-layout)
- [Event](#event)
	- [event filter](#event-filter)
- [Paint](#paint)
	- [Device](#device)
- [File](#file)

<!-- /MarkdownTOC -->

## qtchooser

```
vi /usr/lib/x86_64-linux-gun/qt-chooser/default.conf
  <qt>/bin
  <qt>/lib
```


## Qt Modules

- Basic
  - Qt Core
  - Qt GUI
  - Qt Quick
  - Qt QML
  - Qt Test
  - Qt Network
  - Qt Multimedia
  - Qt SQL
  - Qt Webkit
  - Qt WebKit Widgets
  - Qt Widgets
- Extended
  - Qt 3D / Qt Bluetooth / Qt Contact
  - Qt Location / Qt Organizer / Qt Sensors / Qt Publis and Subscribe
  - Qt Service Framework / System Info / JSON DB / Versit / Wayland / Feekback

## .pro

```
CONFIG += C++11
```

## Concrete QWidget 

```
class MyWidget: public QWidget {
public:
	MyWidget() : QWidget() {}
	~MyWidget() {}

public slots:	// optional parameters, dependents on signal functions, // void return, need implementation
	void button_click();

signals: 	// no return value, optional parameters // only declare, no implementation
	void show_hide_signal(int a);
};
```

## UIC

- *MOC* is not the only code generator. Another code generator that Qt uses is *uic* (User Interface Compiler).
- *UIC* reads user interface described in XML and creates C++ code that sets up the form.


## MOC (meta-object compiler)

- The program that handles Qt's C++ extensions.
  - *moc* reads C++ source, if it contains `Q_OBJECT` macor, it produces another C++ file which contain the meta object code for those classes.



### Signal

- one signal func can connect to multiple slots function, **invoke order is undefined**
- multiple signal can connect to one slot
- one signal can connect to another signal
  - the slot func of the signal can `emit` another signal

### Slots

- member func, static func, global func, lambda func(since QT5)
  - Lambda
    ```
    int a = 20;
    [=]() mutable -> int {
    	a = 100;
    	return a;
    }
    ```
- slots function parameter could be less than the parameter in signal func
  - if slots func has parameters, it should be the same order / type with signal func
- when one object is deleted, QT disconnect all slots connected to the object automatically

### slot/signal in Qt 4

- `QObject::connect(&button, SIGNAL(clicked()), &app, SLOT(quit()));`

- Qt5
  - `QObject::connect(&button, &QPushButton::clicked, &app, &QApplication::quit);`

- Lambda
  - `QObject::connect(button, &QPushButton::clicked, secondWindow, [secondWindow]() {
  		secondWindow->setText("clicked");
  	});`
  - the 3rd parameter `secondWindow` is optional, but to specific it would prevent crash issue once `secondWindow` is deleted earlier than current connect.

### QObject

- adds features to C++ (MOC)
  - signals and slots
  - Properties
  - Event handlong
  - Memory Management
    - destruct all children first in its destructor

## Q_OBJECT v.s. Q_GADGET

Q_GADGET is lighter version of Q_OBJECT, it doesn't have signal and slot.

```
The Q_GADGET macro is a lighter version of the Q_OBJECT macro for classes that do not inherit from QObject but still want to use some of the reflection capabilities offered by QMetaObject. Just like the Q_OBJECT macro, it must appear in the private section of a class definition.
Q_GADGETs can have Q_ENUM, Q_PROPERTY and Q_INVOKABLE, but they cannot have signals or slots.
Q_GADGET makes a class member, staticMetaObject, available. staticMetaObject is of type QMetaObject and provides access to the enums declared with Q_ENUMS.
```

## UI

- relative path used in UI is related to Makefile
  - not UI file, not source file, not exe
- when adding resource/image to resouce file, it must be inside the folder of project

### Stylesheet

### Layout

#### 1. Anchor

- `x, y, width, height` could be affected by anchor
```
Rectangle {
	id: bg

	Rectangle {
		width: 40; height: 40
		y: 20
		anchor.right: bg.right
		anchor.top: bg.top 		// override y:20
	}
}
```

#### 2. Layout


## Event

- `e.ignore()` generally is not used in override event functions, it may used in dialog close event.

```
class MyButton: public QPushButton {
protected:
	void MyButton::mousePressedEvent(MouseEvent *e) {
		if (e->button() == Qt::LeftButton) {
			qDebug() << "left button pressed";
			// e.ignore(); // this would pass the event to parent widget, not super class (inhereted class)
		}
		QPushButton::mousePressedEvent(e);
	}

/// event filter

private: 
	MyLabel *label;
	/*
	label->installEventFilter(this);

	bool eventFilter(QObject *obj, QEvent *e) {
		if (obj == lable) {
			if (e->type() == QEvent::MouseMove) {
				// ...
			}
		}
		return QWidget::eventFilter(obj, e);
	}
	*/
};
```

### event filter

- In `QObject`
- return `true` : it won't forward anymore; return `false` : it will continue forward
- **event filter and the object/widget it filters MUST be in same thread**

## Paint


### Device

1. QPixmap: optimized for scree, platform-related, cannot modify image
2. QImage : platfor-unrelated, can modify pixes of image, can be drawn in thread
3. QPicture : save paiting status in binary

## File

- 
```
QFileInfo q(path);
qDebug << q.fileName().utf8().data(); // multil-bytes
```














