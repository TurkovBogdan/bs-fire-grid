# bs-firegrid
Минималистичная, гибко настраиваемая сетка с вспомогательными миксинами для быстрого добавления правил адаптации. 
- Написана на SCSS, требует сборщик (webpack/gulp)
- Работает на flexbox, построение от десктопа к мобильным
- Можно настроить имена классов, колличество ячеек, брейкпоинты, отступы и многое другое 
- Опциональные вспомогательные классы для добавления отступов и изменения порядка вывода

# В чём главная фича?
Вам доступно несколько очень крутых миксинов для упрошения адаптации элементов. К примеру, вот так можно прописать 
адаптацию параграфа: 

```scss
// bs-firegrid
p {
  @include add-media(font-size, (xl:18px, lg: 16px, sm: 14px));
}
```
Да, всего одной строчкой. Набор ключей в скобках — обычный ассоциативный массив, в sass он называется [maps](https://sass-lang.com/documentation/values/maps). 
Такая конструкция кажется перегруженной, но она очень краткая. Сравните её с классическим миксином из `bootstrap`:
```scss
// bootstrap
p {
  font-size: 18px;
  @include media-breakpoint-down(lg) {
    font-size: 16px;
  }
  @include media-breakpoint-down(sm) {
    font-size: 14px;
  }
}
```

Нужно адаптировать несколько свойств? Нет проблем:
```scss
p {
  @include add-media((
      font-size: (xl:18px, lg: 16px, sm: 14px),
      line-height: (xl:28px, lg: 24px, sm: 22px)
  ))
}
```

Хотите вынести параметры типографики в переменные? Сказка:
```scss
$p-font-weight: 400; 
$p-font-size: (xl:18px, lg: 16px, sm: 14px);
$p-line-height: (xl:28px, lg: 24px, sm: 22px);

p {
  @include add-media((
      font-weight: $p-font-weight,      
      font-size: $p-font-size,
      line-height: $p-line-height, 
  ))
}
```

Пример выше собирается вот в такой css:
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
