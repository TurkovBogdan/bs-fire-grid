# bs-firegrid
Минималистичная, полностью настраиваемая сетка
- Написана на SCSS, требует сборщик (webpack/gulp)
- Работает на flexbox, построение от десктопа к мобильным
- Можно настроить имена классов, колличество ячеек, брейкпоинты, отступы и многое другое 
- Опциональные вспомогательные классы для добавления отступов и изменения порядка вывода
- Набор миксинов для упрошения адаптации элементов

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
$grid-col-general-class: true;        // вынести базовые стили колонок в отдельный класс
$grid-col-general-class-name: 'col';  // имя класса для базовых стилей
```


### Вспомогательные классы
#### Смещение ячеек. Добавляет отступ слева
```scss
$grid-offset-classes-enable: true;
$grid-offset-classes-prefix: 'offset-';
$grid-offset-classes-postfix: '';
$grid-offset-max: 11;
```

#### Изменение порядка вывода
```scss
$enable-grid-order-classes: true;
$grid-order-classes-prefix: 'order-';
$grid-order-classes-postfix: '';
$grid-order-max: 6;
```
