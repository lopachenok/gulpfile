# Шаблон Front-End проектов

Репозиторий, служащий шаблоном для начала работ над Front-End проектами

Компиляция и сборка осуществляется с помощью Gulp

## Установка шаблона

``` sh
$ git clone https://github.com/lopachenok/gulpfile app-name
$ cd app-name
$ npm i -D
```

## Файловая структура проекта

``` sh
src/ - каталог для размещения рабочих файлов
  include/ - повторяющиеся фрагменты html (в моих проектах только секция head и секция подключение скриптов bottom)
  css/ - глобальные стили проекта и диспетчер подключений, остальные файлы стилей в папках блоков
  js - глобальные скрипты проекта и диспетчер подключений js-файлов
  blocks/ -каталог для размещения каталогов БЭМ-блоков
    block_name/ - каталог для размещения файлов, относящихся к БЭМ-блоку, содержит css-файлы, js-файлы и картинки
build/ -  каталог для размещения скомпилированной верстки
  js/ - каталог для размещения минифицированных js файлов
  css/ - каталог для размещения минифицированных, скомпилированных css файлов
  img/ - каталог для размещения обработанных картинок
```

## Задачи Gulp

### Компиляция CSS

Задача `$ gulp css`

Файл `src/css/vendor.css` является диспетчером подключений для библиотечных css-файлов.

Файл `src/css/style.css` является диспетчером подключений для всех прочих css-файлов.  

Компиляция происходит с помощью PostCSS плагина precss.
При компиляции происходит объединение всех файлов, проверка на наличие ошибок, изменение путей для картинок, добавление вендорных префиксов, минификация и запись sourcemaps.

При запуске задачи с флагом `NODE_ENV=production gulp css` sourcemaps генерироваться не будут. 

### Компиляция JS

Задача `$ gulp js`

Файл `src/js/vendor.js` является диспетчером подключений библиотечных js файлов.

Файл `src/js/main.js` является диспетчером подключений собственных js файлов.

При компиляции происходит объединение всех файлов, проверка на наличие ошибок и запись sourcemaps.

При запуске задачи с флагом `NODE_ENV=production gulp js` sourcemaps генерироваться не будут, файл минифицируется. 

### Копирование и оптимизация картинок

Задача `$ gulp img`

Все изображения в формате png и jpg оптимизируются и копируются в папку сборки.

### Создание svg-спрайтов

Задача `$ gulp svgsprite`

Все изображения в формате svg собираются в спрайт и копируются в папку сборки. Вместе со спрайтом генерируется файл `src/css/sprite.css`, содержащий стили для изображений спрайта. Файл нужно подключить в диспетчер подключений `src/css/style.css`.

### Сборка HTML

Задача `$ gulp html`

Страницы, которые необходимо сверстать размещаются в корне каталога `src/`. Файлы html повторяющихся блоков размещаются в каталоге `src/include/` и в каталоге `blocks/название блока`. В `src/include/` рекомендуется держать файл `head.html` содержащий секцию `<head>` и файл `bottom.html` содержащий подключения js-скриптов. Остальные файлы располагаются в папке своего блока.

Файлы подключаются в html с помощью директивы `@include('путь в файлу')`. После компиляции в результирующий html вставляется содержимое всех файлов в него подключенных.


### Отчистка каталога сборки 

Задача `$ gulp clean`

При выполнении задачи полностью удаляется содержимое каталога `build/`.

### Сборка проекта

Задача `$ gulp build`

Последовательно запускает задачи `clean`, `svgsprite` и параллельно `css`, `img`, `js`.

### Livereload и отслеживание изменений

Задача `$ gulp server`

При выполнении задачи запускается локальный веб-сервер BrowserSync и открыватся index.html проекта.  

[Подробнее о BrowserSync](http://www.browsersync.io/ "Подробнее о BrowserSync")  

Сервер использует каталог `build/` в качестве корня проекта.

Также запускается отслеживание всех изменений в соответствующих каталогах для соответствующих задач и выполняется пересборка проекта.

### Публикация на Github Pages

Задача `$ gulp deploy`

При запуске задачи будет создана ветка gh-pages и в нее залито текущее состояние каталога `build/`.

### Задача по умолчанию

Задача `$ gulp`

Запускает задачу `$ gulp server`.

## Создание нового блока

Команда `$ node createBlock.js [имя блока] [доп. расширения через пробел]` создаст новую папку с указанным именем. Внутри будут располагаться файлы с тем же именем и указанными расширениями. По умолчанию создается только файл `.css`. Так же в диспетчерах подключений css и js файлов будут прописаны соответствующие импорты. 