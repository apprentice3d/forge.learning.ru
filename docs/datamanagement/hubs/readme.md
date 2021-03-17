# Репозитории данных (англ. hubs) и проекты

[Data Management API](https://developer.autodesk.com/en/docs/data/v2/overview/) обеспечивает единый доступ к данным из BIM 360 Hub, Fusion Team (ранее известный как A360 Team), BIM 360 Docs и A360 Personal.

![](_media/datamanagement/entities_and_domains.png)

Для навигации и доступа к данным BIM 360, Fusion Team, A360 Personal и Object Storage Service (OSS) необходимо знать следующую терминологию:

- `hubs`:  репозитории данных BIM 360 Team, Fusion Team, аккаунт BIM 360 Docs или A360 Personal
- `projects`: проекты BIM 360 Team, Fusion Team, BIM 360 Docs или A360 Personal
- `folders`: логическая организация элементов внутри одного проекта - папки 
- `items`: одна или несколько версий файлов, например, dwg, pdf или Fusion designs and drawings - элементы
- `versions`: состояние элемента; аналог конкретной версии файла - версии 
- `buckets`: контейнеры для объектов с уникальными именами - бакеты
- `objects`: двоичные данные, идентифицированные URN или ключом и хранящиеся в определенном контейнере - объекты 

> Каждый аккаунт **BIM 360 Docs** - это репозиторий, к которому текущий пользователь имеет доступ. Чтобы идентифицировать эти репозитории, тип `attribute.extension.type` должен быть **hubs:autodesk.bim360:Account** (или проверьте приставку `b.` перед **id**). 

![](_media/datamanagement/hub_extension_types.png)

В этом разделе мы создадим конечную точку для возврата списка **Hubs**, **Projects**, **Folders**, **Items** (файлов) и соответсвующих **Versions** (которые можно отобразить во Viewer).
 
Выберите ваш язык: [Node.js](datamanagement/hubs/nodejs) | [.NET Framework](datamanagement/hubs/net) | [.NET Core](datamanagement/hubs/netcore)
