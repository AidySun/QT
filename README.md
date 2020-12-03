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

### Slots

- member func, static func, global func, lambda func
- slots function parameter could be less than the parameter in signal func
  - if slots func has parameters, it should be the same order / type with signal func