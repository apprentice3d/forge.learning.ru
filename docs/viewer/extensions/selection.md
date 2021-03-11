# Selection

Этот раздел использует **базовый каркас** из предыдущего раздела, но давайте изменим название с **MyAwesomeExtension** на **HandleSelectionExtension**. 

## Создание расширения

As each extension should be a separeted JavaScript file, create a file in the UI folder **/js/handleselectionextension.js** and copy the following content (which is same as the basic skeleton, except with a different name): 
Поскольку каждое расширение должно быть отдельным файлом JavaScript, создайте файл в папке UI ** / js / handleselectionextension.js ** и скопируйте следующий код (который совпадает с базовым каркасом, но имеет другое название):

```javascript
class HandleSelectionExtension extends Autodesk.Viewing.Extension {
    constructor(viewer, options) {
        super(viewer, options);
        this._group = null;
        this._button = null;
    }

    load() {
        console.log('HandleSelectionExtension has been loaded');
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
        console.log('HandleSelectionExtension has been unloaded');
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
        this._button = new Autodesk.Viewing.UI.Button('handleSelectionExtensionButton');
        this._button.onClick = (ev) => {
            // Execute an action here
        };
        this._button.setToolTip('Handle Selection Extension');
        this._button.addClass('handleSelectionExtensionIcon');
        this._group.addControl(this._button);
    }
}

Autodesk.Viewing.theExtensionManager.registerExtension('HandleSelectionExtension', HandleSelectionExtension);
```
## Панель инструментов CSS

Как и в базовом каркасе, панель инструментов использует форматирование **CSS**. В **/css/main.css** добавьте:

```css
.handleSelectionExtensionIcon {
    background-image: url(https://github.com/encharm/Font-Awesome-SVG-PNG/raw/master/white/png/24/object-group.png);
    background-size: 24px;
    background-repeat: no-repeat;
    background-position: center;
}
```

> You can use your own images or from a library, in this case let's use [Font Awesome](https://fontawesome.com/) icons in PNG format. Вы можете использовать свои собственные изображения или изображения из библиотеки, в этом случае используйте значки [Font Awesome] (https://fontawesome.com/) в формате PNG.

## Загрузка расширения

[Загрузите расширение](/viewer/extensions/skeleton?id=loading-the-extension), используя тот же код, как и в разделе **базовый каркас** (конечно, изменив название). For your reference, here are the 2 changes needed: include the `<script>` on **index.html** and include the extension on viewer creation: Для справки, необходимо внести 2 изменения: включить `<script>` в ** index.html ** и включить расширение при настройке Viewer:

 Откройте файл **/index.html** и добавьте следующую строчку:

```html
<script src="/js/handleselectionextension.js"></script>
```

В **/www/js/ForgeViewer.js** найдите строку:

```javascript
viewer = new Autodesk.Viewing.GuiViewer3D(document.getElementById('forgeViewer'));
```

И замениье её на:

```javascript
viewer = new Autodesk.Viewing.GuiViewer3D(document.getElementById('forgeViewer'), { extensions: ['HandleSelectionExtension'] });
```

Важно :- Если одно расширение уже загружено, тогда HandleSelectionExtension может быть добавлено с использованием **запятой (',')** в множестве:

```javascript
viewer = new Autodesk.Viewing.GuiViewer3D(document.getElementById('forgeViewer'), { extensions['MyAwesomeExtension','HandleSelectionExtension'] }); 
```

На этом этапе расширение должно загрузиться, и на панели инструментов появится кнопка, но она не будет работать.

## Добавление функции .onClick 

Now it's time to replace the `Execute an action here` placeholder inside the `.onClick` function. For this sample, let's isolate the selection. Copy the following content to your extension **.js** file inside the `.onClick` function: Теперь пора заменить `Execute an action here` внутри функции` .onClick`. Для этого примера давайте будем скрывать выделенные элементы. Скопируйте следующий код в файл вашего расширения **.js** внутри функции `.onClick`:

```javascript
// Get current selection
const selection = this.viewer.getSelection();
this.viewer.clearSelection();
// Anything selected?
if (selection.length > 0) {
    let isolated = [];
    // Iterate through the list of selected dbIds
    selection.forEach((dbId) => {
        // Get properties of each dbId
        this.viewer.getProperties(dbId, (props) => {
            // Output properties to console
            console.log(props);
            // Ask if want to isolate
            if (confirm(`Isolate ${props.name} (${props.externalId})?`)) {
                isolated.push(dbId);
                this.viewer.isolate(isolated);
            }
        });
    });
} else {
    // If nothing selected, restore
    this.viewer.isolate(0);
}
```

## Завершение

На этом этапе расширение должно загрузиться и отобразить кнопку на панели инструментов. Выберите один или несколько объектов и нажмите кнопку, чтобы подтвердить, какие элементы нужно скрыть. Следующее видео демонстрирует то, как это должно выглядеть.

![](_media/javascript/js_isolate.gif)

> The browser console is essential for web development and debug. Learn more on how to use it for [Chrome](https://developers.google.com/web/tools/chrome-devtools/console/), [Edge](https://docs.microsoft.com/en-us/microsoft-edge/devtools-guide/console), [Firefox](https://developer.mozilla.org/en-US/docs/Tools/Web_Console/Opening_the_Web_Console) and [Safari](https://developer.apple.com/safari/tools/).

Key learning points:

- **.getSelection()** returns an array of **dbId** from the model, and **.clearSelection()**
- **.getProperties()** is an asynchronous method that returns all properties for a given dbId via callback, which is widelly used on Viewer, [learn more about callbacks](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
- **.isolate()** method makes all other elements transparent ("ghosted")

Additional learning points:

- **.forEach()** to iterate through a collection, this is a JavaScript feature, [learn more](https://www.w3schools.com/jsref/jsref_forEach.asp)
- **.push()** to to include items on an array, [learn more](https://www.w3schools.com/jsref/jsref_push.asp)

Next: [Docking panel](viewer/extensions/panel)
