# bs-fire-grid
Минималистичная scss сетка на flexbox

[Главная фича](#в-чём-главная-фича) - [Сетка](#о-сетке) - [Установка](#установка) - [Настройка](#настройка) - [Миксины](#доступные-миксины)

## В чём главная фича?
Вам доступно несколько крутых миксинов для упрошения адаптации элементов. К примеру, вот так можно прописать 
адаптацию параграфа: 

```scss
p {
  @include add-media(font-size, (xl:18px, lg: 16px, sm: 14px));
}
```
Да, всего одной строчкой. Набор ключей в скобках — обычный ассоциативный массив, в sass он называется [maps](https://sass-lang.com/documentation/values/maps). 

Первое время такая конструкция кажется «грязной», но когда привыкаешь к ней, оторваться невозможно — объём стилей получается значительно меньше, а значит упрошается навигация и читаемость кода.

Нужно адаптировать несколько свойств? Нет проблем:
```scss
p {
  @include add-media((
      font-weight: 400,
      font-size: (xl:18px, lg: 16px, sm: 14px),
      line-height: (xl:28px, lg: 24px, sm: 22px)
  ))
}
```

<details>
  <summary>Собранные стили</summary>
    
```css
p {
    font-weight: 400;
    font-size: 18px;
    line-height: 28px
}

@media (max-width: 1199px) {
    p {
        font-size: 16px;
        line-height: 24px
    }
}

@media (max-width: 767px) {
    p {
        font-size: 14px;
        line-height: 22px
    }
}
```
</details>

## О сетке
- Работает на flexbox
- Все правила работают от десктопа к мобильным
- Написана на SCSS, потребует сборщик (webpack/gulp)
- Можно настроить имена классов, колличество ячеек, брейкпоинты, отступы и другое 
- Опциональные вспомогательные классы для добавления отступов и изменения порядка вывода
- Набор миксинов для упрошения адаптации элементов


## Установка
Используя git
```
git clone https://github.com/TurkovBogdan/bs-fire-grid.git
```

или bower:
```
bower install bs-fire-grid --save
```

После получения файлов, подключите в шаблоне стилей два файла:
```scss
// Миксины, функции, переменные. Не выводит никаких стилей, но даёт доступ к миксинам
@import "bs-fire-grid/utils/grid-utils";

// Выводит стили сетки
@import "layout/grid";
```
Параметры изменяются в файле `/bs-fire-grid/utils/variables/grid.scss`. По умолчанию установлены параметры аналогичные сетке `bootstrap 4`.

Если вы не хотите изменять оригинальные файл, скопируйте настройки в отдельный файл, и подключите до утилит:
```scss
@import "custom-grid-settings"; // пользовательские настройки сетки

@import "bs-fire-grid/utils/grid-utils";
@import "bs-fire-gridlayout/grid";
```

## Настройки
<details>
  <summary>Режим отладки</summary>
  
```scss 
$grid-debugger: true; 
``` 
Если `true`, на экране отобразиться фиксированная панель с названием текущего брейкпоинта и полоска отображающая размер контейнера и его отступы. Очень удобно при настройке сетки и точечной докрутки адаптации. 
</details>

<details>
  <summary>Настройка имён классов и ячеек </summary>
  
```scss
$grid-col-count: 12;                // Кол-во колонок

$grid-container-class: 'container'; // Имя класса для контейнера
$grid-row-class: 'row';             // Имя класса для ряда
$grid-col-prefix: '';               // Префикс для класса ячейки. Шаблон {префикс}{код брейкпоинта}-{колонка}
```
</details>

<details>
  <summary>Брейкпоинты и отступы</summary>
  
```scss
$grid-container-padding: 15px;      // Отступ у контейнера
$grid-col-padding: 15px;            // Отступ у колонки
```

Многоуровневая мапа с перечнем брейкпоинтов. Брейкпоинты нужно перечислять от больших к меньшим
```scss
$grid-breakpoints: (
        xl: (min: 1200px, container: 1140px, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
        lg: (min: 992px, container: 960px, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
        md: (min: 768px, container: 720px, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
        sm: (min: 576px, container: 540px, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
        xs: (min: 0px, container: 100%, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
);
```
Для каждого брейкпоинта можно указать:
```scss
$grid-breakpoints: (
        xl: (
             min: 1200px,             // минимальное разрешение экрана
             container: 1140px,       // размер контейнера
             container-padding: 15px, // отступ у контейнера
             col-padding: 15px        // отступ у колонки
             ),
        // ...
);
```
</details>

<details>
  <summary>Дополнительно</summary>
  
  ```scss
$grid-col-general-class-enable: true;   // вынести базовые стили колонок в отдельный класс
$grid-col-general-class-name: 'col';    // имя класса для базовых стилей
```
Если `true`, базовые стили колонок (отступы и т.д.) будут вынесены в отдельный класс с именем указанным в `$grid-col-general-class-name`. Это значительно сократит вес стилей, но потребует добавления `.col` к каждой колонке.
</details>


### Вспомогательные классы
<details>
  <summary>Смещение колонок</summary>

```scss
$grid-offset-classes-enable: true;
$grid-offset-classes-prefix: 'offset-';
$grid-offset-classes-postfix: '';
$grid-offset-max: 11;
```
Шаблон имени класса: `{префикс}{код брейкпоинта}{постфикс}-{смещение}`. Использование при значениях по умолчанию:
```html
    <div class="row">
        <div class="col xl-6"> ... </div>
        <div class="col xl-4 offset-xl-2"> ... </div>
    </div>
```
</details>

<details>
  <summary>Изменение порядка вывода</summary>
  
```scss
$enable-grid-order-classes: true;             
$grid-order-classes-prefix: 'order-';
$grid-order-classes-postfix: '';
$grid-order-max: 6;
```

Шаблон имени класса: `{префикс}{код брейкпоинта}{постфикс}-{порядок}`. Использование при значениях по умолчанию:
```html
    <div class="row">
        <div class="col xl-8 order-xl-2"> ... </div>
        <div class="col xl-4 order-xl-1"> ... </div>
    </div>

    <div class="row">
        <div class="col xl-8 order-xl-last"> ... </div>
        <div class="col xl-4 order-xl-first"> ... </div>
    </div>
```
</details>


## Доступные миксины
<details>
  <summary>grid-media<br>Добавить стили к определённому брейкпоинту</summary>

### Синтаксис

```scss
  @include grid-media($bp-code){
    // перечень стилей
  };
```
`$bp-code` — код брейкпоинта из настроек

### Примеры
Добавить стили к разрешению `sm` параграфа:
```scss
p {
  @include grid-media(sm){
    color: red;
    font-size: 16px;
  }
}
```
Стили после сборки:
```scss
@media (max-width: 767px) {
    p {
        color: red;
        font-size: 16px
    }
}
```
</details>

<details>
  <summary>add-media<br>Добавление стилей с указанием набора значений для каждого брейкпоинта</summary>

### Синтаксис
```scss
  @include add-media( $styleName, $styleMap );
```

`$styleName` — имя стили или многоуровневая мапа с перечнем стилей и значений.

`$styleMap` — мапа с перечнем значений стиля для разных брейкпоинтов. Используется только если в `$styleName` передали имя стиля а не мапу.

Если требуется прописать адаптацию только одного стиля, используется два аргумента:
```scss
  @include add-media('имя свойства', (
          'код брейкпоинта': 'значение стиля',
          'код брейкпоинта': 'значение стиля',
          // ...
  ));
```

Для множества стилей передаётся один аргумент с мапой:
```scss
  @include add-media((
      'имя свойства': (
          'код брейкпоинта': 'значение стиля',
          'код брейкпоинта': 'значение стиля',
          // ...
      ),
      'имя свойства': (
          'код брейкпоинта': 'значение стиля',
          'код брейкпоинта': 'значение стиля',
          // ...
      ),
  ));
```

### Примеры
#### Прописываем адаптацию размера шрифта параграфа с значением по умолчанию.
Максимальный брейкпоинт `xl`. По умолчанию, нам нужен размер шрифта `18px`, а начиная с `lg` и ниже `16px`:

```scss
p {
  @include add-media(font-size, (xl:18px, lg: 16px));
}
```
Миксин устанавливает в качестве значения по умолчанию (т.е. прописывает стиль для тега без медиа запроса) значение установленное для максимального брейкпоинта.
 
Стили после сборки:
```scss
p {
    font-size: 18px
}

@media (max-width: 1199px) {
    p {
        font-size: 16px
    }
}
```

#### Прописываем адаптацию размера шрифта параграфа без значения по умолчанию.
Максимальный брейкпоинт `xl`, но мы не будем устанавливать значение для этого брейкпоинта. На `lg` нужен кегель `18px`, а на `sm` `16px`:
```scss
p {
  @include add-media(font-size, (lg:18px, sm: 16px));
}
```

Стили после сборки:
```scss
@media (max-width: 1199px) {
    p {
        font-size: 18px
    }
}

@media (max-width: 767px) {
    p {
        font-size: 16px
    }
}
```

#### Прописываем правила адаптации размера кегеля и интерлиньяжа для параграфа
Если требуется адаптировать несколько свойств, в миксин передаёться только один аргумент — мапа с перечнем стилей. 
```scss
p {
  @include add-media((
      font-size: (xl:18px, sm: 14px),
      line-height: (xl:28px, sm: 22px)
  ))
}
```

Стили после сборки:
```scss
p {
    font-size: 18px;
    line-height: 28px
}

@media (max-width: 767px) {
    p {
        font-size: 14px;
        line-height: 22px
    }
}
```

#### Сложный сценарий, миксин всеяден
Вы можете прописать базовые стили прямо в теге и подключить миксин с перечнем адаптивных стилей. Или прописывать все стили только в миксине. Или комбинировать. 

Ниже мы прописываем часть стилей прямо в теге, задаём цвет по умолчанию и жирность в миксине и прописываем несколько правил адаптации:
```scss
p {
  font-size: 18px;
  line-height: 28px;
  @include add-media((
          color: (xl: black, sm: white),
          font-size: (lg: 14px),
          line-height: (lg: 22px),
          font-weight: 400,
  ))
}
```
Стили после сборки:
```css

p {
    font-size: 18px;
    line-height: 28px;
    font-weight: 400
}

@media (max-width: 1199px) {
    p {
        font-size: 14px;
        line-height: 22px
    }
}

@media (max-width: 767px) {
    p {
        color: #fff
    }
}
```
</details>


<details>
  <summary>add-media-background<br>Подмена background от разрешения экрана</summary>
  
  ### Синтаксис
  ```scss
  @include add-media-background( $imageMap );
  ```
  
  `$imageMap` — мапа, ключи это разрешение экрана а значения сссылки на изображение. Разрешения необходимо указывать по убыванию
  
  ```scss
  @include add-media-background((
          'разрешеие': 'ссылка на изображение',
          // или, если нужна поддержка ретины
          'разрешеие': (x1: 'ссылка на изображение x1', x2:'ссылка на изображение x2'),
  ));
  ```

### Примеры
Этот миксин идеально подходит для первого экрана с фоновым изображением. Мы можем подготовить изображения разных размеров и загружать только подходящие под экран пользователя:

```scss
.first-screen-bg{
  @include add-media-background((
          2560: "../../img/2560@1x.jpg",
          1920: "../../img/1920@1x.jpg",
          1300: "../../img/1300@1x.jpg",
          762: (x1: "../../img/762@1x.jpg", x2:"../../img/762@2x.jpg"),
          545: (x1: "../../img/545@1x.jpg", x2:"../../img/545@2x.jpg"),
          440: (x1: "../../img/440@1x.jpg", x2:"../../img/440@2x.jpg"),
  ));
}
```

Собирается в:
```scss

.first-screen-bg {
    width: 100%;
    height: 100vh;
    background-position: top;
    background-size: 440px auto;
    background-image: url(../img/440@1x.jpg)
}

@media (-webkit-min-device-pixel-ratio: 2),(min-resolution: 192dpi) {
    .first-screen-bg {
        background-image: url(../img/440@2x.jpg)
    }
}

@media screen and (min-width: 441px) {
    .first-screen-bg {
        background-size: 545px auto;
        background-image: url(../img/545@1x.jpg)
    }
}

@media (min-width: 441px) and (-webkit-min-device-pixel-ratio: 2),(min-width: 441px) and (min-resolution: 192dpi) {
    .first-screen-bg {
        background-image: url(../img/545@2x.jpg)
    }
}

@media screen and (min-width: 546px) {
    .first-screen-bg {
        background-size: 762px auto;
        background-image: url(../img/762@1x.jpg)
    }
}

@media (min-width: 546px) and (-webkit-min-device-pixel-ratio: 2),(min-width: 546px) and (min-resolution: 192dpi) {
    .first-screen-bg {
        background-image: url(../img/762@2x.jpg)
    }
}

@media screen and (min-width: 763px) {
    .first-screen-bg {
        background-size: 1300px auto;
        background-image: url(../img/1300@1x.jpg)
    }
}

@media screen and (min-width: 1301px) {
    .first-screen-bg {
        background-size: 1920px auto;
        background-image: url(../img/1920@1x.jpg)
    }
}

@media screen and (min-width: 1921px) {
    .first-screen-bg {
        background-size: 2560px auto;
        background-image: url(../img/2560@1x.jpg)
    }
}
```


</details>
