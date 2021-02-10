# Аутентификация (OAuth)

Аутентификация(OAuth), specifically OAuth2, is the open standard used across the Forge Platform for token-based authentication and authorization.

## Двухфакторная (англ. 2-legged) vs Трёхфакторная (англ. 3-legged)

Узнайте больше про получение [2-legged токена](https://developer.autodesk.com/en/docs/oauth/v2/tutorials/get-2-legged-token/) для OAuth Autodesk Forge, который используется в руководстве [Визуализация моделей](tutorials/viewmodels), и про получение [3-legged токена](https://developer.autodesk.com/en/docs/oauth/v2/tutorials/get-3-legged-token/), который используется в руководстве [Просмотр моделей из репозиториев Autodesk BIM 360 & Fusion 360](tutorials/viewhubmodels).

## Область действия (англ. scopes)

Область действия - это разрешение, установленное для токена, границы, в которых этот токен может действовать. Например, токен с областью  _data:read_ может считывать данные в экосистеме Forge и использоваться на тех конечных точках (англ. endpoints), которым требуется эта область. Токенам без области действия будет отказано в доступе к таким конечным точкам. (На справочных страницах отдельных конечных точек перечислены требуемые области.)

Области применения выполняют две основные функции:

- **Конфиденциальность и контроль**: При трёхфакторной аутентификации они действуют как механизм для запроса и обеспечения разрешения действовать от имени конечного пользователя определенными способами.
- **Безопасность**: При двух- и трёхфакторной аутентификации они гарантируют, что при потере контроля над своим токеном его нельзя будет неправильно использовать для доступа к ресурсам, для которых он не предназначен.

[Узнайте больше](https://developer.autodesk.com/en/docs/oauth/v2/overview/scopes/)

## Public and Internal tokens

This tutorial will use 2 types of access tokens: public and internal. The **public** token is used for the Forge Viewer which runs and requires an access token on the client. There is a special scope for this scenario: **viewables:read**. 

Now on the server-side we need write access, so the **internal** token will use **bucket:create**, **bucket:read**, **data:read**, **data:create**, and **data:write** scopes.

> Don't know which tutorial to follow? 
> 
> Answer this: where are the files you want to access and view? 
> 
> If on your computer or in some other place, then **View your models**. If the models are on any BIM 360 (Team, Design or Docs) or Fusion Team, then **View BIM 360 & Fusion models**.
>
> If you want to modify models, then say no more, check the **Modify your models** tutorial.

Next: [View your models](tutorials/viewmodels) or [View BIM 360 & Fusion models](tutorials/viewhubmodels) or [Modify your models](tutorials/modifymodels)
