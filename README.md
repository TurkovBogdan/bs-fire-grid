# bs-firegrid
Минималистичная, полностью настраиваемая сетка
- Написана на SCSS, требует сборщик (webpack/gulp)
- Работает на flexbox, построение от десктопа к мобильным
- Можно настроить имена классов, колличество ячеек, брейкпоинты, отступы и многое другое 
- Опциональные вспомогательные классы для добавления отступов и изменения порядка вывода
- Набор миксинов для упрошения адаптации элементов

[Главная фича](#в-чём-главная-фича) - [Сетка](#о-сетке) - [Установка](#установка) - [Настройка](#настройка) - [Доступные миксины](#доступные-миксины)

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

После сборки:
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

## О сетке
Все правила работают от десктопа к мобильным


## Установка
Скопируйте файлы сетки в директорию стилей и подключите два файла:
```scss
@import "utils/grid-utils";   // Миксины, функции, переменные 
@import "layout/grid";        // Вывод сетки
```

## Настройка
Параметры сетки изменяются в файле ```/utils/variables/grid.scss```. По умолчанию, установлены параметры аналогичные сетке `bootstrap 4`.
### Режим отладки
```scss 
$grid-debugger: true; 
``` 
Если `true`, на экране отобразиться фиксированная панель с названием текущего брейкпоинта и полоска отображающая размер контейнера и его отступы. Очень удобно при настройке сетки и точечной докрутки адаптации. 

### Настройка имён классов и ячеек 
```scss
$grid-col-count: 12;                // Кол-во колонок

$grid-container-class: 'container'; // Имя класса для контейнера
$grid-row-class: 'row';             // Имя класса для ряда
$grid-col-prefix: '';               // Префикс для класса ячейки. Шаблон {префикс}{код брейкпоинта}-{колонка}
```

### Брейкпоинты и отступы
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

### Дополнительно
```scss
$grid-col-general-class-enable: true;   // вынести базовые стили колонок в отдельный класс
$grid-col-general-class-name: 'col';    // имя класса для базовых стилей
```
Если `true`, базовые стили колонок (отступы и т.д.) будут вынесены в отдельный класс с именем указанным в `$grid-col-general-class-name`. Это значительно сократит вес стилей, но потребует добавления `.col` к каждой колонке.


### Вспомогательные классы
#### Смещение колонки
Шаблон имени 
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

#### Изменение порядка вывода
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

## Доступные миксины
<details>
  <summary>grid-media<br>Добавить стили к определённому брейкпоинту</summary>

```scss
  @include grid-media($bp-code){
    // перечень стилей
  };
```
Пример использования:
```scss
p {
  @include grid-media(sm){
    color: red;
    font-size: 16px;
  }
}

// результат:
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

```scss
  @include add-media( ... );
```

Если требуется адаптация только одного стиля, передайте вкачестве первого атрибута название стиля, а в качестве второго мапу с набором значений. Значение прописанное для максимального брейкпоинта в сетке становиться значением по умолчанию (т.е. прописывается для тега без медиа запросса).
```scss
p {
  @include add-media(font-size, (xl:18px, lg: 16px));
}

// результат:
p {
    font-size: 18px
}

@media (max-width: 1199px) {
    p {
        font-size: 16px
    }
}
```
Если требуется адаптация нескольких стилей, передайте только один аргумент — мапу с перечнем стилей. Набор не ограничен, он не прописаны в коде.
```scss
p {
  @include add-media((
      font-size: (xl:18px, sm: 14px),
      line-height: (xl:28px, sm: 22px)
  ))
}

// результат:
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

Если стиль не требует адаптации, можете передать одиночное значение:
```scss
p {
  @include add-media((
          font-size: 16px,
          line-height: (xl:28px, lg: 24px, sm: 22px)
  ))
}

// результат:
p {
    font-size: 16px;
    line-height: 28px
}

@media (max-width: 1199px) {
    p {
        line-height: 24px
    }
}

@media (max-width: 767px) {
    p {
        line-height: 22px
    }
}
```

Очень удобно выносить параметры адаптации в переменные и редактировать их из одного файла:
```scss
$p-font-size: (xl:18px, lg: 16px, sm: 14px);
$p-line-height: (xl:28px, lg: 24px, sm: 22px);

p {
  @include add-media((
          font-size: $p-font-size,
          line-height: $p-line-height
  ))
}
```
</details>
