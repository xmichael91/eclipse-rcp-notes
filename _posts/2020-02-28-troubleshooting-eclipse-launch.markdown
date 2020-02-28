---
layout: post
title:  "Решение проблем запуска приложения Eclipse"
categories: eclipse
---
![Ошибка запуска приложения]({{site.baseurl}}/assets/eclipse/launch/error.png)

В процессе разработки плагина возникает необходимость протестировать его в составе приложения на базе Eclipse или в составе Eclipse IDE. Для этого создается соответствующая Конфигурация запуска (Run/debug configuration), в которой настраивается запускаемое приложение. Изначально Eclipse предложит произвести запуск приложения со всеми плагинами, присутствующими в Workspace и Target Platform. Это не всегда приемлимо из-за длительности запуска и некоторых других причин, поэтому можно попытаться включить в Конфигурацию запуска только необходимые плагины. К сожалению, это оказывается нетривиальной задачей, которая потребует времени и поиска решения в Интернете. Решение сводится к добавлению в Конфигурацию запуска нескольких плагинов.

Рассмотрим несколько типичных проблем и их решение, а в конце заметки подведем итог.

***

#### Проблема

При нажатии кнопки "Validate Plug-ins" или при запуске приложения, проверка зависимостей показывает недостающую функциональность:

`Missing Constraint: Require-Capability: osgi.extender; ...`

#### Решение

В списке плагинов вручную отметьте плагин `org.eclipse.equinox.ds` и его зависимости ("Add Required Plug-ins")

***

#### Проблема

При запуске приложения происходит ошибка:

`The application could not start`

В консоли stack trace указывает:

```
java.lang.NullPointerException
	at org.eclipse.e4.ui.internal.workbench.ModelServiceImpl.<init>(ModelServiceImpl.java:122)
```

#### Решение

В списке плагинов вручную отметьте плагин `org.eclipse.equinox.event`

***

#### Проблема

При запуске приложения происходит ошибка:

`The application could not start`

В консоли текст ошибки следующий:

`Application "org.eclipse.ui.ide.workbench" could not be found in the registry.`

#### Решение

В списке плагинов вручную отметьте плагин `org.eclipse.ui.ide.application` и его зависимости ("Add Required Plug-ins")

***

# Корректный запуск по-шагам

Если обобщить рассмотренные проблемы, то корректный запуск приложения будет выглядеть так:

* на основной вкладке выберите приложение для запуска, например Eclipse IDE
* на вкладке Plug-ins снимите все отметки и выберите плагины из Workspace, которые нужно запустить
* добавьте конкретные плагины из Target Platform, если необходимо (например egit, если в приложении нужен git)
* также добавьте следующие плагины:
  - `org.eclipse.equinox.ds`
  - `org.eclipse.equinox.event`
  - `org.eclipse.ui.ide.application` - если вы запускаете приложение Eclipse IDE
* под списком плагинов снимите галочку "Include optional dependencies when computing required Plug-ins"
* нажмите кнопку "Add Required Plug-ins"
