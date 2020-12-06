# QT

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


## UI

- relative path used in UI is related to Makefile
  - not UI file, not source file, not exe
- when adding resource/image to resouce file, it must be inside the folder of project

### Stylesheet

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



















