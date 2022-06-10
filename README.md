# DOM, JavaScript, and You

The **D**ocument **O**bject **M**odel is an abstract representation of the contents of a webpage.

The DOM is accessed and manipulated via a DOM API; while such APIs exist for basically any language out there, you will most likely be using JavaScript.

Any of the methods described below exist under the `document` namespace, ie they are acessed by `document.[methodName]`

**PLEASE NOTE:** this list of methods is by no means exhaustive! These are ony the things you will use most commonly. The entries marked with **an asterisk \*** are particularly useful.

- [`getElementById()`](#getelementbyid)
- [`getElementsByClassName()`](#getelementsbyclassname)
- [`getElementsByTagName()`](#getelementsbytagname)
- [`querySelector() *`](#queryselector-)
- [`querySelectorAll() *`](#queryselectorall-)
- [`parentElement`](#parentelement)
- [`nextElementSibling`](#nextelementsibling)
- [`children`](#children)
- [`firstElementChild`](#firstelementchild)
- [`id`](#id)
- [`classList *`](#classlist-)
- [`dataset`](#dataset)
- [`hasAttribute()`](#hasattribute)
- [`getAttribute()`](#getattribute)
- [`setAttribute()`](#setattribute)
- [`createElement()`](#createelement)
- [`innerHTML *`](#innerhtml-)
- [`innerText *`](#innertext-)
- [`outerHTML`](#outerhtml)
- [`outerText`](#outertext)
- [`insertAdjacentElement()`](#insertadjacentelement)
- [`appendChild()`](#appendchild)
- [`removeChild()`](#removechild)

## Selecting stuff

There are various ways to select elements on a webpage; here are the most common ones:

### `getElementById()`

Get a single element, given its ID

```javascript
    let logo = document.getElementById('logo');
    // <span id="logo">Logo</span>
```

### `getElementsByClassName()`

Get all elements which share a class name

```javascript
    let cells = document.getElementsByClassName('cell');
    // [div.cell, div.cell, div.cell, div.cell, div.cell]
```

### `getElementsByTagName()`

Get all elements with the same tag

```javascript
    let spans = document.getElementsByTagName('span');
    // [span#logo, span, span.strong, span, span, span.strong]
```

### `querySelector() *`

Given a CSS-like string, get *a single element* that conforms to it; may contain descendant selectors

For more on these selectors, please see [the addendum](#addendum-css-like-selectors)

```javascript
    let nav = document.querySelector('nav#navigation ul.nav');
    // <ul class="nav">...</ul>
```

### `querySelectorAll() *`

Given a CSS-like string, get *all elements* that conform to it; may contain descendant selectors

For more on these selectors, please see [the addendum](#addendum-css-like-selectors)

```javascript
    let navLinks = document.querySelector('nav#navigation ul.nav a.link');
    // [a.link, a.link, a.link.active, a.link, a.link]
```

### `parentElement`

Gets the element containing the selected element

```javascript
    let nav = document.querySelector('nav#navigation ul.nav');
    console.log(nav.parentElement);
    // <nav id="navigation"><ul class="nav">...</ul></nav>
```

### `nextElementSibling`

Gets the element immediately following the selected element 

```javascript
    let logo = document.querySelector('#logo');
    console.log(logo.nextElementSibling);
    // <nav id="navigation">...</nav>
```

### `children`

Gets the selected element's descendants

```javascript
    let nav = document.querySelector('nav#navigation ul.nav');
    console.log(nav.children);
    // [li.navlink, li.navlink, li.navlink]
```

### `firstElementChild`

Gets the selected element's first descendant

```javascript
    let nav = document.querySelector('nav#navigation ul.nav');
    console.log(nav.firstElementChild);
    // <li class="navlink">...</li>
```

## Attributes

### `id`

Sets the ID for the selected element

```javascript
logo.id = "newLogo";
// <span id="newLogo">Logo</span>
```

### `classList *`

Manipulates the classes for a given element; it is itself an object, with the following methods:


| Method | Description |
|--------|-------------|
| `item(index)` | Returns the `index`th class of the element |
| `contains(name)` | Returns `true` if the element has class `name` and `false` otherwise |
| `add(name)` | Adds class `name` to the element |
| `remove(name)` | Removes class `name` from the element |
| `toggle(name)` | Adds class `name` to the element if it's missing, and removes it if the element has it |

### `dataset`

Allows the setting and reading of `data-*` attributes

```javascript
let car = document.querySelector('#car');
// <div id="car" data-model="S" />
console.log(car.dataset.model)
// "S"
car.dataset.make = "Tesla"
// <div id="car" data-model="S" data-make="Tesla" />
```

### `hasAttribute()`

Checks if the selected element has the named attribute

```javascript
console.log(car.hasAttribute('class'));
// false
```

### `getAttribute()`

Returns the selected attribute for the current element

```javascript
let carId = car.getAttribute('id');
// carId = 'car'
```

### `setAttribute()`

For the selected element, sets the specified attribute to the specified value

```javascript
car.setAttribute('engine', 'electric');
// <div id="car" data-model="S" data-make="Tesla" engine="electric" />
```

## Element manipulation

### `createElement()`

Creates an element of the specified type

```javascript
let header = document.createElement('header');
// <header />
```

### `innerHTML *`

Sets the HTML inside an element to the specified value

```javascript
header.innerHTML = "<span>Hello!</span>";
// <header><span>Hello!</span></header>
```

### `innerText *`

Sets the text inside an element to the specified value

```javascript
header.innerText = "Hello!";
// <header>Hello!</header>
```

### `outerHTML`

Replaces the HTML of the specified element with the specified HTML

```javascript
header.outerHTML = "<span>Hello!</span>";
// <span>Hello!</span>
```

### `outerText`

Replaces the HTML of the specified element with the specified text

**THIS WILL CHANGE THE DOCUMENT STRUCTURE**

```javascript
header.outerText = "Hello!";
// Hello!
```

### `insertAdjacentElement()`

Inserts a child element in the DOM on one of these positions, relative to the target element: [`beforebegin`,`afterbegin`,`beforeend`,`afterend`]

The positions are as follows:

```html
<!-- beforebegin -->
<div id="target">
    <!-- afterbegin -->
    <div id="extra"></div>
    <!-- beforeend -->
</div>
<!-- afterend -->
```

```javascript
let inserted = document.querySelector('#inserted');
let target = document.querySelector('#target');
target.insertAdjacentElement('afterbegin', inserted);

// <div id="target">
//     <div id="inserted"></div>
//     <div id="extra"></div>
// </div>
```

### `appendChild()`

Inserts a child element as the last element of a target element; essentially functions as `targetElement.insertAdjacentElement('beforeEnd', childElement);`

### `removeChild()`

Removes a child element from a partend element;

```javascript
let inserted = document.querySelector('#inserted');
let target = document.querySelector('#target');
target.removeChild(inserted);

// <div id="target">
//     <div id="extra"></div>
// </div>
```

## Addendum: CSS-like selectors

Some basic CSS knowledge helps; lacking that, here are some things to keep in mind:

* `element`
    * Any valid element name will select all those elements on the page
* `.class`
    * Any class name will select all the elements on the pagewhich share that class
* `#id`
    * Any valid (and unique!) ID will select the element with said ID
* `.class1.class2`
    * Only those elements which have both classes
* `parent > child`
    * Only those child elements which are direct descendants of the parent
* `parent child`
    * The elements which are descendants of the parent element, no matter how deep
* `target + sibling`
    * The sibling element which is immediately next to the target element; **this selects the sibling**
* `target ~ sibling`
    * The sibling element which is *somewhere* on the same level as the target

These can be combined in any way, such that `header nav#navigation a.active + a` is a perfectly valid example (meaning "the link immediately following the active link in the nav element with the 'navigation' ID in the header"). Bear in mind that this sort of thing is overkill.

Less is more!

And always remember, [MDN is your friend!](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)

