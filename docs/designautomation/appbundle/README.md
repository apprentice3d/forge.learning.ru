# Подготовка плагина AppBundle для Design Automation

AppBundle определяет, какие операции буду выполняться в Design Automation. 
Design Automation использует .bundle так же, как Autodesk App Store. Это означает, что вам нужно создать `PackageContents.xml` и ZIP-файл с` DLL` (и другими необходимыми файлами). Для получения подробной информации о том, как их создать, перейдите на сайт [Autodesk App Store Developer Center](https://www.autodesk.com/developer-network/app-store).

В этом разделе мы создадим стандартный плагин, который обновит параметры `width` и `height` и сохранит полученный файл, а также вспомогательные файлы (`PackageContents.xml`) и структуру папок для их размещения. Создайте файл .ZIP, готовый для загрузки в Design Automation.

### Требования

- **7zip**: используется для создания .ZIP с файлами bundle. Пожалуйста, установите [его здесь](https://www.7-zip.org/). В этом руководстве предполагается, что **7zip** по умолчанию загружается в папку _C:\Program Files\7-Zip\7z.exe_.

### Дополнительные требования

Для следующего раздела вы можете использовать заранее подготовленный плагин. Если вы захотите создать его, вам потребуется:
- **Visual Studio**: Visual Studio 2017 или более новая версия. Пожалуйста, перейдите [по ссылке](https://visualstudio.microsoft.com/vs/).

- **AutoCAD, Inventor, Revit и 3ds Max**: Для разработки, тестирования и отладки вашего плагина Design Automation: [AutoCAD](https://www.autodesk.com/products/autocad/overview) | [Inventor](https://www.autodesk.com/products/inventor/overview) | [Revit](https://www.autodesk.com/products/revit/overview) | [3ds Max](https://www.autodesk.com/products/3ds-max/overview).

***

На следующем шаге выберите **Engine** (рус. движок), т.е. продукт Autodesk, в котором будет запускаться ваш плагин. Вам потребуется установить соответствующее приложение для локальной компиляции, отладки и тестирования.

Выберите движок: [AutoCAD](/designautomation/appbundle/engines/autocad) | [Inventor](/designautomation/appbundle/engines/inventor) | [Revit](/designautomation/appbundle/engines/revit) | [3ds Max](/designautomation/appbundle/engines/max)
