# QML

<!-- MarkdownTOC autolink="true" -->

- [qmldir](#qmldir)
	- [directory listing qmldir file](#directory-listing-qmldir-file)
	- [module definition qmldir file](#module-definition-qmldir-file)
- [Dynamic QML Object Creation from JavaScript](#dynamic-qml-object-creation-from-javascript)
- [Layout Managers in QML](#layout-managers-in-qml)
	- [Anchor Layout](#anchor-layout)
- [Qt Quick](#qt-quick)
- [Color](#color)
	- [Ways to be specified](#ways-to-be-specified)
	- [Gradient](#gradient)
- [Binding Loop](#binding-loop)
- [Assignment v.s. Declaration](#assignment-vs-declaration)
- [Loader](#loader)
	- [`objectName` v.s. `id`](#objectname-vs-id)

<!-- /MarkdownTOC -->


1. qmlscence.exe xxx.qml
2. xxx
```
!env qml
```
3. qmake xxx.pro
  - nmake/make

## qmldir

### directory listing qmldir file

- [internal] Object type declaration
  - *internal* can only be seen by qml in same dir, cannot be seen by qml files that import this dir
- Javascript resource declaration
```
RoundedButton RoundBtn.qml
internal HightlightButton HLBtn.qml
MathFunctions mathFuncs.js
```

### module definition qmldir file

```
module MyModule
MyButton 1.0 MyButton.qml
MyButton 1.1 MyButton11.qml
```
- It's recommended to use diff directories for each major version.




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
    Component.onDestruction: console.log("Destruction Beginning!")
} 
```

- `completed()` 
  - Emitted after the object has been instantiated. This can be used to execute script code at startup, once the full QML environment has been established.
  - The `onCompleted` signal handler can be declared on any object. **The order of running the handlers is undefined.**
  - same to `onDestruction`

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

- one solution	: **implicitWidth**

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


## Component & Customize Item

There are two ways to create reusable user interface components

1. Components
  - Defined with `Component` item
  - Used as templates for items
  - Used with models and view
  - Used with generated content
  - Can be instantiated dynamically
  ```
  Component {
  	id: myComponent
  	Text { ... }
  }
  ```
  - It's not directly instantiated, but it can be used with
    - Loader
    - Repeater
    - Views
    - Scripting
2. Custom Items / Loader?
  - Defined in sparated files
  - One main element per file
  - Used in the same was as standard items
  - Can have an associated version number
  ```
  // MyRecoangle.qml
  Rectangle {
  	color: "white"
  	TextInput { ... }
  }
  ```
  - File name **MUST** start with a capital letter
    - Remember the convention as when we define any standard element, e.g. `Rectangle`, `Text` etc. They are all capitalized.

### Loader

Can dynamicly load QML component or file. 
`onLoaded` signal handler is invoked on completion; `item` property holds root item

- load file (with `source` property)
```
Item {
	Loader {
		id: dynamicLoader
		// source: "" // set source = "" to unload the qml
	}

	Rectangle {
		MouseArea {
			anchors.fill: parent
			onClicked: dynamicLoader.source = "dynamicItem.qml"
		}
	}

	// Singles emmitted by loaded object can be received using the **Connections** type
	Connections {
		target: dynamicLoader.item
		onMessage: console.log(msg)
	}
}
```

- load component (with `sourceComponent` property)
```
Component {
	id: myComp
	Item {
		...
	}
}

Loader {
	id: componentLoader
	anchor.fill: parent
	sourceComponent: myComp
}
```

####  binding and connection of loader

```
Loader {
	id: loader
	source: "LineEdit.qml"
}

Connections {
	target: loader.item
	onReturePressed: currentTxt.text = "Return " + text
}
Binding {
	target: loader.item
	property: "text"
	value: "Hello world!"
}
```

### `objectName` v.s. `id`

- Every QML object can be assigned an id and an objectName that other objects can use to refer to the object.
- The difference between the two is that the id is for referencing the object within QML, while the objectName is required for referencing the object from C++.


## Size & Layout

### implicit size v.s. size

- implicitWidth : real
  Defines the natural width or height of the Item if no width or height is specified.
  The default implicit size for most items is 0x0, however some items have an inherent implicit size which cannot be overridden, for example, Image and Text.
  - Generally, usage of implicitHeight/Width only makes sense within reusable components. 

- width
  Defines the item's position and size.

- I use them as follows: 
  - When I create a new item and it can be resized, I set an implicit size inside the object2. When I'm using the object, I often set the real size explicitly from the outside.
  - The implicit size of an object can be overridden by setting height and width.

## QML Widgets

- `WebView` vs `WebEngineView`
  - `WebView` uses a native web view if available.


## Integration with C++

- https://doc.qt.io/qt-5/qtqml-cppintegration-topic.html

```
#ifndef BACKEND_H
#define BACKEND_H

#include <QObject>
#include <QString>
#include <qqml.h>

class BackEnd : public QObject
{
    Q_OBJECT
    Q_PROPERTY(QString userName READ userName WRITE setUserName NOTIFY userNameChanged)
    QML_ELEMENT

public:
    explicit BackEnd(QObject *parent = nullptr);

    QString userName();
    void setUserName(const QString &userName);

signals:
    void userNameChanged();

private:
    QString m_userName;
};

#endif // BACKEND_H
```
- The `QML_ELEMENT` macro makes the BackEnd class available in QML.


## QML with CMake









