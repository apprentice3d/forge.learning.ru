# Docking Panel

This section uses the **basic skeleton** from previous section, but let's rename the **MyAwesomeExtension** to **ModelSummaryExtension**. 

## Создание расширения

Каждое расширение должно быть файлом JavaScript и реализовывать, по крайней мере, функции .load и .unload. Создайте файл в папке UI **/js/dockingpanelextension.js* и скопируйте код ниже. **/js/dockingpanelextension.js** and copy the following content (который совпадает с базовой структурой, но имеет другое название): 

```javascript
class ModelSummaryExtension extends Autodesk.Viewing.Extension {
    constructor(viewer, options) {
        super(viewer, options);
        this._group = null;
        this._button = null;
    }

    load() {
        console.log('ModelSummaryExtension has been loaded');
        return true;
    }

    unload() {
        // Clean our UI elements if we added any
        if (this._group) {
            this._group.removeControl(this._button);
            if (this._group.getNumberOfControls() === 0) {
                this.viewer.toolbar.removeControl(this._group);
            }
        }
        console.log('ModelSummaryExtension has been unloaded');
        return true;
    }

    onToolbarCreated() {
        // Create a new toolbar group if it doesn't exist
        this._group = this.viewer.toolbar.getControl('allMyAwesomeExtensionsToolbar');
        if (!this._group) {
            this._group = new Autodesk.Viewing.UI.ControlGroup('allMyAwesomeExtensionsToolbar');
            this.viewer.toolbar.addControl(this._group);
        }

        // Add a new button to the toolbar group
        this._button = new Autodesk.Viewing.UI.Button('ModelSummaryExtensionButton');
        this._button.onClick = (ev) => {
            // Execute an action here
        };
        this._button.setToolTip('Model Summary Extension');
        this._button.addClass('modelSummaryExtensionIcon');
        this._group.addControl(this._button);
    }
}

Autodesk.Viewing.theExtensionManager.registerExtension('ModelSummaryExtension', ModelSummaryExtension);
```

## Панель инструментов CSS

Как и в базовой структуре, панель инструментов использует форматирование CSS (см. вызов функции `.addClass` в коде). В /css/main.css добавьте: 

```css
.modelSummaryExtensionIcon {
  background-image: url(https://github.com/encharm/Font-Awesome-SVG-PNG/raw/master/white/png/24/dashboard.png);
  background-size: 24px;
  background-repeat: no-repeat;
  background-position: center;
}
```

## Загрузка расширения

[Загрузите расширение](/viewer/extensions/skeleton?id=loading-the-extension), используя тот же код, как и в разделе базовая структура (конечно, изменив название). Для справки, необходимо внести 2 изменения: включить `<script>` в **index.html** и включить расширение при настройке Viewer: 

```html
<script src="/js/dockingpanelextension.js"></script>
```

```javascript
viewer = new Autodesk.Viewing.GuiViewer3D(document.getElementById('forgeViewer'), { extensions: ['ModelSummaryExtension'] });
```

> Note how `extensions` is an array, so you can load multiple extensions! For instance, to load the previous selection sample and this, just use `['HandleSelectionExtension', 'ModelSummaryExtension']` instead! Cool, right?

На этом этапе расширение должно загрузиться, и на панели инструментов появится кнопка, но она не будет работать.

## Enumerate leaf nodes

The Viewer contains all elements on the model, including categories (e.g. families or part definition), so we need to enumerate the leaf nodes, meaning actual instances on the model. The following `getAllLeafComponents()` function should be added to our extension class. This is based on [this blog post](https://forge.autodesk.com/blog/enumerating-leaf-nodes-viewer). 

```javascript
getAllLeafComponents(callback) {
    this.viewer.getObjectTree(function (tree) {
        let leaves = [];
        tree.enumNodeChildren(tree.getRootId(), function (dbId) {
            if (tree.getChildCount(dbId) === 0) {
                leaves.push(dbId);
            }
        }, true);
        callback(leaves);
    });
}
```

## Docking panel

The extension will show the results on a Viewer [property panel](https://forge.autodesk.com/en/docs/viewer/v7/reference/UI/PropertyPanel/). Copy the content to your extension **.js** file (anywhere on the file, outside other functions).

```javascript
class ModelSummaryPanel extends Autodesk.Viewing.UI.PropertyPanel {
    constructor(viewer, container, id, title, options) {
        super(container, id, title, options);
        this.viewer = viewer;
    }
}
```

## Implement .onClick function

Now it's time to replace the `Execute an action here` placeholder inside the `onClick` function. For this sample, let's first show the property panel, then enumerate leaf nodes, then get a specific set of properties for leaf nodes, finally count ocurrences of those properties and show results on the panel. 

!> In the code below you **MUST** adjust `filteredProps` to the property names that applies to your models. For instance, as **Material** exists on almost all models, you can try with `const filteredProps = ['Material'];`

Copy the following content to your extension **.js** file inside the `onClick` function of the extension's button:

```javascript
// Check if the panel is created or not
if (this._panel == null) {
    this._panel = new ModelSummaryPanel(this.viewer, this.viewer.container, 'modelSummaryPanel', 'Model Summary');
}
// Show/hide docking panel
this._panel.setVisible(!this._panel.isVisible());

// If panel is NOT visible, exit the function
if (!this._panel.isVisible())
    return;

// First, the viewer contains all elements on the model, including
// categories (e.g. families or part definition), so we need to enumerate
// the leaf nodes, meaning actual instances of the model. The following
// getAllLeafComponents function is defined at the bottom
this.getAllLeafComponents((dbIds) => {
    // Now for leaf components, let's get some properties and count occurrences of each value
    const filteredProps = ['PropertyNameA', 'PropertyNameB'];
    // Get only the properties we need for the leaf dbIds
    this.viewer.model.getBulkProperties(dbIds, filteredProps, (items) => {
        // Iterate through the elements we found
        items.forEach((item) => {
            // and iterate through each property
            item.properties.forEach(function (prop) {
                // Use the filteredProps to store the count as a subarray
                if (filteredProps[prop.displayName] === undefined)
                    filteredProps[prop.displayName] = {};
                // Start counting: if first time finding it, set as 1, else +1
                if (filteredProps[prop.displayName][prop.displayValue] === undefined)
                    filteredProps[prop.displayName][prop.displayValue] = 1;
                else
                    filteredProps[prop.displayName][prop.displayValue] += 1;
            });
        });
        // Now ready to show!
        // The PropertyPanel has the .addProperty that receives the name, value
        // and category, that simple! So just iterate through the list and add them
        filteredProps.forEach((prop) => {
            if (filteredProps[prop] === undefined) return;
            Object.keys(filteredProps[prop]).forEach((val) => {
                this._panel.addProperty(val, filteredProps[prop][val], prop);
            });
        });
    });
});
```

## Conclusion

At this point the extension should load and show a toolbar button. Click on the button and the panel should appear. The following video demonstrate its behaviour.

![](_media/javascript/js_dockingpanel.gif)

> As mentioned, you need to define the **filteredProps** appropriate for your models. The above video used `['Material', 'Design Status', 'Type Name'];` which works for both models.

Ключевые функции:

- **.getObjectTree()** даёт доступ к структуре модели, а с **.getChildCount()** и **.enumNodeChildren()** можно повторно перебирать дерево объекта
- **.getBulkProperties()** это асинхронный метод, который возвращает определенный набор свойств для массива dbIds через обратный вызов, который широко используется в Viewer, [подробнее о callback](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
- **.addProperty()** панельный метод добавляет свойства (имя, значение) в категорию

Дополнительные функции:

- **.forEach()** для перебора коллекции, это функция JavaScript, [узнайте больше](https://www.w3schools.com/jsref/jsref_forEach.asp)

Далее: [Примеры](viewer/extensions/examples)
