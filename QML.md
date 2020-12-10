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