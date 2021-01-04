# QML


1. qmlscence.exe xxx.qml

2. 
```
!env qml
```
3. qmake xxx.pro
nmake/make

## [Dynamic QML Object Creation from JavaScript](https://doc.qt.io/qt-5/qtqml-javascript-dynamicobjectcreation.html)


There are two ways to create objects dynamically from JavaScript.
You can either call Qt.createComponent() to dynamically create a Component object, 
or use Qt.createQmlObject() to create an object from a string of QML.

1. `object createComponent(url, mode, parent)`

```
import qtquick 2.0

Item {
    id: container
    width: 300; height: 300

    function loadButton() {
        var component = Qt.createComponent("Button.qml");
        if (component.status == Component.Ready) {
            var button = component.createObject(container);
            button.color = "red";
        }
    }

    Component.onCompleted: loadButton()
}
```

1. `object createQmlObject(string qml, object parent, string filepath)`

```
var newObject = Qt.createQmlObject('import QtQuick 2.0; Rectangle {color: "red"; width: 20; height: 20}',
                                   parentItem,
                                   "dynamicSnippet1");
```


## Layout Managers in QML

1. Anchor Layout
1. Posintioners. `row/column/grid`
1. Qt Quick controls layout
1. Manually with literal `x, y, width, height`

### Anchor Layout

- Two special anchors
  - `centerIn`
  - `fill`


## Qt Quick

The Qt Quick module is the standard library for writing QML applications. It provides:
  - QML API: supplies QML types for creating user interfaces with QML
    - Javascript
  - C++ API: for extanding QML applications with C++ code

## Color

### Ways to be specified

1. color name
2. `#<aa><rr><gg><bb>`. e.g. "#00ff0000"
3. `Qt.rgba(0, 0, 255, 0)`

### Gradient

```
gradient: Gradient {
	GradientStop {
		position: 0.0
		color: "yellow"
	}

	GradientStop {
		position: 1.0
		color: "blue"
	}
}
```

## Binding Loop

```
Rectangle {
	width: child.width
	height: child.height

	Rectangle {
		id: child
		anchors.fill: parent
		anchors.margins: 5
	}
}
```

- one solution

```
Rectangle {
	implicitWidth: child.implicitWidth
	implicitHeight: child.implicitHeight

	Rectangle {
		id: child
		anchors.fill: parent
		anchors.margins: 5
	}
}
```

## Assignment v.s. Declaration

It's the diff between imperative programming by assiging value to properties and declarative programming with property binding.

- Assignment to a property would overwrite the declaration of the property
```
Item {
	Rectangle {
		color: rec2.

	}
	Rectangle {
		id: rec2
		MouseArea {
			anchors.fill: parent
			onPositionChanged: 

		}
	}
}
```





















