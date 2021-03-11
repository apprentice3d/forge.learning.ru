# Примеры

Мы создали автономные расширения для Viewer, которые вы с легкостью можете использовать повторно:

- [GitHub Repo](https://github.com/Autodesk-Forge/forge-extensions)
- [Demo Link](https://forge-extensions.autodesk.io/)

Вот еще несколько примеров расширений, основанных на каркасном подходе:

- [Change color](https://forge.autodesk.com/blog/happy-easter-setthemingcolor-model-material): добавляет 3 кнопки на панель инструментов для изменения цвета выбранных элементов модели.
Расширение может взаимодействовать с сервером для реализации более сложных функций, как и любой другой код JavaScript. Примеры ниже демонстрируют это:

**Node.js**

- [Share Viewer state](https://forge.autodesk.com/blog/share-viewer-state-websockets): uses websocket to share state between 2+ instances of the Viewer. использует websocket для обмена состоянием между 2+ экземплярами Viewer.

**.NET**

- [Custom properties](https://forge.autodesk.com/blog/custom-properties-viewer-net-lambda-dynamodb): stores custom properties on a database (AWS DynamoDB) and uses a .NET WebAPI code to serve via REST endpoints. хранит настраиваемые свойства в базе данных (AWS DynamoDB) и использует код .NET WebAPI для обслуживания через конечные точки REST.

Далее: [Развертывание](deployment/)
