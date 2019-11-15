# fire-grid
Минималистичная, гибко настраиваемая сетка с вспомогательными миксинами для быстрого добавления правил адаптации. 
- Написана на SCSS, потребуется сборщик (webpack/gulp)
- Работает на flexbox, построение от десктопа к мобильным
- Можно настроить имена классов, колличество ячеек, брейкпоинты, отступы и многое другое 
- Опциональные вспомогательные классы для добавления отступов и изменения порядка вывода

# Установка
Скопируйте файлы сетки в директорию стилей и подключите два файла:
```scss
@import "utils/grid-utils";   // Миксины, функции, переменные 
@import "layout/grid";        // Вывод сетки
```

## Настройка
Параметры сетки изменяются в файле ```/utils/variables/grid.scss```
### Режим отладки
```scss
$grid-debugger: true;
```
Если параметр включен, на экране отобразиться фиксированная панель с названием текущего брейкпоинта и полоска отображающая 
размер контейнера и его отступы. Очень удобно при настройке сетки и точечной докрутки адаптации. 

### Настройка имён классов и ячеек 
```scss
$grid-container-class: 'container';
$grid-container-padding: 15px;
$grid-row-class: 'row';

$grid-col-count: 12;
$grid-col-prefix: '';
$grid-col-padding: 15px;
```

### Брейкпоинты
```scss
$grid-breakpoints: (
        xl: (min: 1200px, container: 1140px, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
        lg: (min: 992px, container: 960px, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
        md: (min: 768px, container: 720px, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
        sm: (min: 576px, container: 540px, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
        xs: (min: 0px, container: 100%, container-padding: $grid-container-padding, col-padding: $grid-col-padding),
);
```

### Создание общего класса ячеек
```scss
// Вынести основные стили ячеек в общий класс
$grid-col-general-class: true;
$grid-col-general-class-name: 'col';
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
