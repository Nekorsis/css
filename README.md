# Airbnb CSS / Sass Советы по стилю кода

*Наиболее разумный подход к CSS и Sass*

## Содержание

  1. [Терминология](#terminology)
    - [Правила декларации](#rule-declaration)
    - [Селекторы](#selectors)
    - [Свойства](#properties)
  1. [CSS](#css)
    - [Форматирование](#formatting)
    - [Комментарии](#comments)
    - [Объектно-ориентированный CSS и БЭМ](#oocss-and-bem)
    - [ID Селектор](#id-selectors)
    - [Хуки JavaScript](#javascript-hooks)
    - [Границы](#border)
  1. [Sass](#sass)
    - [Синтаксис](#syntax)
    - [Порядок объявления свойств](#ordering-of-property-declarations)
    - [Переменные](#variables)
    - [Миксины](#mixins)
    - [Extend directive](#extend-directive)
    - [Вложенные селекторы](#nested-selectors)
  1. [Переводы](#translation)

## Терминология

### Объявление правил

"Объявление правил" это имя данное селектору (или группе селекторов) с сопутствующими ему свойствами. Например:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Селекторы

В объявлении правил, "селекторы",  это части которые определяют к какому элементу DOM дерева будут применены правила стилей. Селекторы могут соответствовать HTML элементу, а так-же классу элемента, ID или любому другому атрибуту этого элемента. Вот несколько примеров:


```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Свойства

И наконец свойства, которые придают выбранным элементам их стиль. Свойства объявляются в виде пары "ключ-значение" и объявления правил могут содержать одно или несколько свойств. Объявление свойств может выглядеть так:


```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### Форматирование

* Используйте 2 пробела для отступов.
* Предпочитайте подчеркивание CamelCase'у в именах классов.
  - Подчеркивания и PascalCasing допустимы, если вы используете БЭМ (смотри [Объектно-ориентированный CSS и БЭМ](#oocss-and-bem) далее)
* Не используйте селекторы по ID.
* Используя несколько селекторов в объявлении правила, переносите каждый селектор на отдельную строку.
<!--* Ставьте пробел перед открывающей скобкой `{`.
* В свойствах ставьте пробел после двоеточия `:`, но не перед.
* После объявления свойстав переносите закрывающую скобку `}` на новую строку. 
* Делайте отступ в одну строку между объявлениями правил.

**Плохо**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Хорошо**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Комментарии

* Предпочитайте однострочные (`//`) комментарии многострочным .
* Рекомендуется писать комментарии в отдельные строки. Старайтесь избегать комментариев в конце строки.
* Write detailed comments for code that isn't self-documenting:
* Пишите детальные комментарии для незадокументированных частей кода:
  - Использование z-index
  - Совместимость с браузерами или CSS-хаки

### Объектно-ориентированный CSS и БЭМ

Мы рекомендуем комбинировать Объектно-ориентированный CSS и БЭМ по следующим причинам:

  * Это помогает создать чистую, строгую связь между CSS и HTML.
  * Помогает создавать многоразовые, составные компоненты.
  * Меньше вложенностей, низкая специфичность правил.
  * Способствует созданию масштабируемых таблиц стилей.


**OOCSS**, или "Объектно-ориентированный CSS", это подход к написанию CSS, который призывает думать о таблице стилей, как о коллекции "объектов": многоразовых, повторяемых фрагментах кода, которые могут использоваться независимо друг от друга на всём сайте.
  * Nicole Sullivan [OOCSS вики](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine [Введение в Объектно-ориентированный CSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**БЭМ**, или "Блок-Элемент-Модификатор", это соглашение об именовании классов в HTML и CSS. Разработано Яндексом с прицелом на большие объёмы кода и масштабируемость. Может послужить как солидный набор правил для использования OOCSS.
  * CSS Trick's [БЭМ 101](https://css-tricks.com/bem-101/)
  * Harry Roberts [Введение в БЭМ](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

Мы рекомендуем вариант БЭМ в котором используется PascalCased "блоки", отлично работающие в связке с компонентами (например React).
Подчеркивания и тире по-прежнему используются для модификаторов и элементов.
**Примеры**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

  * `.ListingCard` является "блоком" и представляет родительский компонент
  * `.ListingCard__title` является "элементом" и представляет дочерний компонент `.ListingCard` который позволяет составить блок в целом.
  * `.ListingCard--featured` является "модификатором" и представляет разные состояния `.ListingCard`.

### Селекторы по ID

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.
Возможность выбирать элементы по ID в CSS является, как правило,  плохой практикой. ID селекторы предоставляют неоправданно высокий уровень специфичности и невозможность многоразового использования.
Более подробная информация по этому вопросу: [Статья CSS Wizardry](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/)

### JavaScript хуки

Избегайте использования одинаковых имён классов в CSS и JavaScript. Использование одинаковых имён классов может привести, как минимум, к трате времени при рефакторинге, и как максимум к боязне разработчика сломать функционал вводом изменений.

Мы реккомендуем создавать отдельные имена классов для JavaScript используя префикс `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

Use `0` instead of `none` to specify that a style has no border.

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```

## Sass

### Синтаксис

* Всегда используйте `.scss` синтаксис, и никогда, оригинальный `.sass` синтаксис.
* Упорядочивайте обычный CSS и `@include`-объявления логически.

### Порядок объявления свойств

1. Объявления свойств

    Перечислите все стандартные объявления свойств, всё что не является `@include`-объявлением или вложенным селектором.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include`-объявления

    Группирование `@include`-объявлений в конце правила делает его более читаемым.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Вложенные селекторы

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.
    Вложенные селекторы, _если необходимо_, идут последними, и ничего не должно быть после них. Добавьте пробел между объявлением правила и вложенным селектором, а так-же между смежными вложенными селекторами. Примените те же принципы, что и  для ваших вложенных селекторов.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Переменные

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).
Отдавайте предпочтение именам переменных разделенных тире (например `$my-variable`). Допускается использование подчеркивания в виде префикса для имён которые будут использоваться в пределах одного файла (например `$_my-variable`).
### Миксины

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.
Миксины должны использоваться для поддержания чистоты и ясности кода, или абстрактной сложности, во многом так же, как и хорошо названные функции. Миксины не принимающие никаких аргументов могут быть полезны для этого, но нужно иметь в виду что если вы не сжимаете свои файлы (например gzip), это может привести к лишнему повторению кода.
### Extend directive допилить

`@extend` should be avoided because it has unintuitive and potentially dangerous behavior, especially when used with nested selectors. Even extending top-level placeholder selectors can cause problems if the order of selectors ends up changing later (e.g. if they are in other files and the order the files are loaded shifts). Gzipping should handle most of the savings you would have gained by using `@extend`, and you can DRY up your stylesheets nicely with mixins.
Использование `@extend` необходимо избегать из-за его  неинтуитивного поведения и потенциальной опасности в поведение, особенно при использование вместе с вложенными селекторами. Даже *допилить* селекторов верхнего уровня может создать проблемы, если в будущем будет изменён порядок селекторов. Сжатие должно обрабатывать большую часть сбережений вы получили бы с помощью *допилить*.
### Вложенные селекторы

**Do not nest selectors more than three levels deep!**
**Вложенные селекторы не должны быть глубже трёх вложений**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

When selectors become this long, you're likely writing CSS that is:
Когда селекторы становятся слишком длинными, скорее всего вы пишите CSS который:

* Strongly coupled to the HTML (fragile) *—OR—*
* Слишком сильно привязан к HTML (хрупкий) *—OR—*
* Overly specific (powerful) *—OR—*
* Слишком специфичен *—OR—*
* Not reusable
* Не многоразовый *—OR—*


Again: **never nest ID selectors!**
И вновь: **никогда не используйте ID селекторы!**
If you must use an ID selector in the first place (and you should really try not to), they should never be nested. If you find yourself doing this, you need to revisit your markup, or figure out why such strong specificity is needed. If you are writing well formed HTML and CSS, you should **never** need to do this.
Если вы должны использовать ID селектор (вы действительно должно постараться этого не делать), он никогда не должен быть вложен. Если вы обнаружили это в своём коде, вам нужно пересмотреть разметку, или выяснить, почему нужна такая сильная специфика. Если вы пишете хорошо сформированные HTML и CSS, вы **никогда** не должны делать это.  
## Перевод

  This style guide is also available in other languages:
  Этот гид по стилю так-же доступен в других языках:

  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [Zhangjd/css-style-guide](https://github.com/Zhangjd/css-style-guide)
