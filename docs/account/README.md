# Аккаунт Autodesk

Для использования платформы Autodesk Forge вам нужно создать аккаунт на сайте Forge https://forge.autodesk.com/. 

## Создание аккаунта Forge

Перейдите на сайт платформы [Autodesk Forge](https://forge.autodesk.com/), нажмите “SIGN UP” для создания нового аккаунта или "SIGN IN" для входа в уже существующий. Если вы создаете новый аккаунт, не забудьте подтвердить вашу электронную почту, пройдя по отправленной вам ссылке. 

![](/_media/forge/dev_portal_home.png)

## Приобретение подписки

Обратите внимание, что при первой авторизации на сайте, вы автоматически получаете доступ к бесплатному пробному периоду (Trial), который длится 90 дней и включает в себя приветственный пакет из 100 cloud credits. При этом вы получаете полный доступ ко всем компонентам платформы Forge (включая платные, например,**Model Derivative**).

![](_media/account/activate_sub.png)

## Создание приложения

Наведите курсор на правый верхний угол страницы - вы увидите ваше имя и меню вашего аккаунта. Перейдите в раздел **My Apps** (рус. мои приложения) и нажмите на “CREATE APP” (рус. создать приложение). 

Select APIs you are going to use (you can safely select all for now). Enter your application name and description, then enter a callback URL: `http://localhost:3000/api/forge/callback/oauth` (this tutorial will not use this callback, but that's the URL used on other Autodesk Forge samples)

Once you set up an application, you will see a Client ID and Client Secret in your newly created app page. You will need these in all other OAuth flows and, by extension, to complete every other tutorial on this site!

![](_media/account/create_app.gif)

!> **DO NOT** share your Client Secret, this should be kept confidential.

You are now good to go!

Далее: [Tools](environment/tools/)
