# Аккаунт Autodesk

Для использования платформы Autodesk Forge вам нужно создать аккаунт Forge. 

## Создание аккаунта Forge

Перейдите на сайт платформы [Autodesk Forge](https://forge.autodesk.com/), нажмите “SIGN UP” для создания нового аккаунта или "SIGN IN" для входа в уже существующий. Если вы создаете новый аккаунт, не забудьте подтвердить вашу электронную почту, пройдя по отправленной вам ссылке. 

![](/_media/forge/dev_portal_home.png)

## Activate subscription

Before using any of the paid APIs, like **Model Derivative**, you need to activate your trial. On the top-right, you'll see your name. Click to expand the menu and go to **My Subscription**. On the page that opens, click on **START FREE TRIAL**. That's it.

![](_media/account/activate_sub.png)

## Create an app

On the top-right, you'll see your name. Click to expand the menu and go to **My Apps**. Click the “CREATE APP” button.

Select APIs you are going to use (you can safely select all for now). Enter your application name and description, then enter a callback URL: `http://localhost:3000/api/forge/callback/oauth` (this tutorial will not use this callback, but that's the URL used on other Autodesk Forge samples)

Once you set up an application, you will see a Client ID and Client Secret in your newly created app page. You will need these in all other OAuth flows and, by extension, to complete every other tutorial on this site!

![](_media/account/create_app.gif)

!> **DO NOT** share your Client Secret, this should be kept confidential.

You are now good to go!

Далее: [Tools](environment/tools/)
