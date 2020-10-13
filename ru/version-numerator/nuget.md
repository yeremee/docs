---
layout: page

title: Version Numerator
description: Автоматический нумератор версий для Visual Studio

permalink: ru/version-numerator/nuget/
pl_language: ru
pl_path: version-numerator/nuget
---


# {{ page.description }} #


## Использование nuget.exe в проектах с AssemblyInfo ##

Если вы хотите использовать сборщик пакетов Nuget в проектах старого типа (тех, которые используют файл `AssemblyInfo` для задания атрибутов сборки), то для синхронизации версии пакета с версией сборки нужно сделать следующее.

1. [Загрузить](https://www.nuget.org/downloads) файл `nuget.exe` и поместить его в одну из папок переменной `PATH`, например в `%USERPROFILE%\.dotnet\tools`.

2. Добавить в поле **_Командная строка события после сборки_** эти строки:
   ```
   chdir $(ProjectDir)
   nuget.exe spec
   nuget.exe pack $(ProjectPath) -OutputDirectory $(TargetDir) -Properties Configuration=$(ConfigurationName)
   VersionNumerator.exe $(ProjectPath) $(ConfigurationName)
   ```

3. После первой сборки с такими настройками, нужно отредактировать сгенерированный файл манифеста (с расширением `.nuspec`), находящийся в папке проекта. Без допустимых значений в манифесте пакет создаваться не будет!

   Дополнительные сведения о манифесте пакета Nuget доступны по [этому адресу](https://docs.microsoft.com/nuget/reference/nuspec).

