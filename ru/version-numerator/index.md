---
layout: page

title: Version Numerator
description: Автоматический нумератор версий для Visual Studio
project_startpage: true

permalink: ru/version-numerator/
pl_language: ru
pl_path: version-numerator
---


# {{ page.description }} #


## Что делает ##

Последовательно увеличивает различные части номера версии при каждом построении проекта в зависимости от выбранной сборочной конфигурации, таким образом обеспечивая *непрерывную сквозную нумерацию версий* на протяжении всего процесса разработки.


### Состав номера версии ###

Используемый номер версии имеет вид `A.B.C.D`, где
- `A` – номер *major* версии (старший),
- `B` – номер *minor* (младший),
- `C` – *build* (сборка),
- `D` – *revision* (пересмотр).

Он состоит из двух независимых частей — `A.B` и `C.D` — в которых увеличение номера `A` влечёт обнуление номера `B`, а увеличение `C` — обнуление `D`.


## Как использовать ##

1. Поместите программу в одну из папок, указанных в переменной среды **Path**, например в `%USERPROFILE%\.dotnet\tools`.

2. Откройте свойства проекта, перейдите в раздел **_События сборки_**, и добавьте следующую строку в поле **_Командная строка события после сборки_**:
   ```
   VersionNumerator.exe $(ProjectPath) $(ConfigurationName)
   ```
   Если вы хотите использовать сборщик пакетов Nuget в проектах с `AssemblyInfo`, то потребуются [дополнительные команды](nuget).

3. Выполняйте сборку проекта в конфигурации **Debug** до тех пор, пока не будет достигнут нужный функционал.

   После каждой сборки в конфигурации **Debug** увеличивается номер *build* версии и обнуляется номер *revision*: `A.B.C⁺¹.D⁼⁰`

4. Когда функционал достигнут, выполните сборку проекта в конфигурации **Release**.

   После каждой сборки в конфигурации **Release** увеличиваются номера *build* и *minor* версии и обнуляется номер *revision*: `A.B⁺¹.C⁺¹.D⁼⁰`

5. При внесении новых изменений, опять собирайте проект в конфигурации **Debug** до достижения нужного качества, а затем — в конфигурации **Release**.

   Если внесённые изменения были столь значительны, что необходимо увеличить номер *major* версии, то делать это следует вручную, одновременно обнуляя номер *minor*: `A⁺¹.B⁼⁰.C.D`

