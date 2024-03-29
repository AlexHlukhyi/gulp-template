Большое спасибо Владимиру (https://github.com/vaban-ru) за предоставленный шаблон.

## Подготовка к работе

1. `git clone git@github.com:AlexHlukhyi/gulp-template.git`
2. `cd gulp-template`
3. `npm install`
4. `npm start`

## Структура проекта

```
gulp-template
├── src
│   ├── css
│   ├── fonts
│   ├── img
│   ├── js
│   ├── scss
│   ├── public
│   ├── templates
│   └── pages
├── package.json
├── README.md
├── gulpfile.js
├── .babelrc
├── .browserslistrc
├── .prettierrc
├── .prettierignore
└── .gitignore
```

* Корень проекта:
    * ```.babelrc``` — настройки Babel
    * ```.prettierrc``` — настройки Prettier
    * ```.prettierignore``` — запрет изменения файлов Prettier
    * ```.gitignore``` – запрет на отслеживание файлов Git'ом
    * ```package.json``` — список зависимостей
    * ```README.md``` — описание проекта
    * ```gulpfile.js``` — файл конфигурации Gulp
    * ```.browserslistrc``` — файл конфигурации поддерживаемых версий браузеров
    
* Папка ```src``` - используется во время разработки:
    * ```css``` — директория для файлов css библиотек, изначально тут лежит файл сброса стилей
    * ```fonts``` – директория для шрифтов
    * ```img``` — директория для изображений
    * ```js``` — директория для js библиотек
    * ```scss``` — директория для scss файлов
    * ```public``` — директория для пользовательских файлов, все файлы из неё будут скопированы в корень собранного проекта
    * ```templates``` — директория для html файлов которые добавляются в проекте
    * ```pages``` — директория для html страниц
 
 ## Верстка
Команды для сборки:
 - `npm run start` запускает сборку и локальный сервер с Hot Reloading
 - `npm run build` запускает сборку и на выходе получаем собранные файлы app.js и app.css
 
## PostHTML

Для расстановки правильных переносов используется плагин [PostHTML Richtypo](https://github.com/Grawl/posthtml-richtypo). Для блока в котором вы хотите отформатировать текст необходимо указать атрибут `data-typo`:
```
<p data-typo>Тут текст</p>
```

Для шаблонизации в проекте используется [Gulp PostHTML](https://github.com/posthtml/gulp-posthtml) с плагинами [PostHTML Include](https://github.com/posthtml/posthtml-include) и [PostHTML Expressions](https://github.com/posthtml/posthtml-expressions)

### Добавление файлов
Что бы просто вставить один файл в другой используется конструкция `<include>`, пример кода:
```
<include src="app/templates/header.html"></include>
```

### Компоненты
Для того что бы извне передать в вставляемый файл какие либо данные необходимо использовать директиву `locals`, и передать туда данные в виде JSON объекта, пример кода:
```
<include src="app/templates/head.html" locals='{"title": "Главная страница"}'></include>
```

### Условия
Внутри любого файла можно использовать разные условия, пример кода:
```
<if condition="foo === 'bar'">
  <p>Foo really is bar! Revolutionary!</p>
</if>

<elseif condition="foo === 'wow'">
  <p>Foo is wow, oh man.</p>
</elseif>

<else>
  <p>Foo is probably just foo in the end.</p>
</else>
```

Так же можно использовать конструкцию `switch/case`, пример кода:
```
<switch expression="foo">
  <case n="'bar'">
    <p>Foo really is bar! Revolutionary!</p>
  </case>
  <case n="'wow'">
    <p>Foo is wow, oh man.</p>
  </case>
  <default>
    <p>Foo is probably just foo in the end.</p>
  </default>
</switch>
```

### Циклы
В любом файле так же можно перебирать данные (массивы или объекты) с помощью цикла, пример кода:
#### Массив
```
<each loop="item, index in array">
  <p>{{ index }}: {{ item }}</p>
</each>
```

#### Объект
```
<each loop="value, key in anObject">
  <p>{{ key }}: {{ value }}</p>
</each>
```

Так же не обязательно передавать данные через переменную, их просто можно написать в цикл, пример кода:
```
<each loop="item in [1,2,3]">
  <p>{{ item }}</p>
</each>
```

В цикле вы можете использовать уже готовые переменные для выборки определенных элементов:
* `loop.index` - текущий индекс элемента, начинается с 0
* `loop.remaining` - количество оставшихся до конца итераций
* `loop.first` - булевый указатель, что элемент первый
* `loop.last` - булевый указатель, что элемент последний
* `loop.length` - количество элементов
