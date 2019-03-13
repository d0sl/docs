# Процесс сборки проекта MPS

## Проблемы при конфигурировании зависимостей в MPS

> I don't know of any mechanism to get MPS itself to download Maven libraries.

В MPS существует проблема с подключением зависимостей, поскольку MPS не работает с пакетными менеджерами типа maven и gradle.

Поэтому приходится каждый jar подключать в проект индивидуально, в некий специальный solution для внешних библиотек.

Вторая проблема, что если изменить имя jar-файла, то solution необходимо переконфигурировать внутри проекта MPS, что невозможно в случае автоматической сборки.

## Предлагаемое решение

> You could create a "setup" task in Gradle/Maven to download the dependency and copy it somewhere (stripping the version number in the process). Then configure a solution in MPS to load the copied JAR as stubs.

> I assumed you have some sort of command-line build for your project using Ant/Maven/Gradle or some other build tool, for the purposes of continuous integration, so you would place this task into that build and you would run it before opening the project in MPS.

Делаем внутри проекта MPS специальный solution с внешними библиотеками, при этом названия библиотек должны быть без названий версий, чтобы не менять конфигурацию при замене.

В git репозиторий проекта включать jar-файлы из этого solution не надо (добавить в .gitignore)

Настраиваем build.gradle таким образом, чтобы он:
+ скачивал нужные зависимости
+ убирал из названий файлов номера версий
+ копировал полученные файлы в нужный каталог внутри нашего solution с зависимостями

> Что касается, наших артефактов, необходимых для проекта (например, ядро d0sl), их предлагается хранить в https://artifactory.eyeline.mobi

> Процесс сборки проекта MPS будет примерно таким
> + git clone repo (или git pull)
> + запуск gradle
>   - скачивает артефакты, преобразует имена, складывает куда надо
>   - запускает задачи ant (ant generate build)
>   - далее можно открывать проект в MPS


