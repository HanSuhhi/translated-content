---
title: Прототипы объектов
slug: Learn/JavaScript/Objects/Object_prototypes
tags:
  - JavaScript
  - create()
  - Конструктор
  - Начинающий
  - ООП
  - Обучение
  - Объект
  - Статья
  - прототип
translation_of: Learn/JavaScript/Objects/Object_prototypes
original_slug: Learn/JavaScript/Объекты/Object_prototypes
---
{{LearnSidebar}}{{PreviousMenuNext("Learn/JavaScript/Objects/Object-oriented_JS", "Learn/JavaScript/Objects/Inheritance", "Learn/JavaScript/Objects")}}Прототипы - это механизм, с помощью которого объекты JavaScript наследуют свойства друг от друга. В этой статье мы объясним, как работают цепочки прототипов, и рассмотрим, как свойство prototype можно использовать для добавления методов к существующим конструкторам.

| Необходимые знания: | Базовая компьютерная грамотность, базовое понимание HTML и CSS, знакомство с основами JavaScript (см. [Первые шаги](/ru/docs/Learn/JavaScript/%D0%9F%D0%B5%D1%80%D0%B2%D1%8B%D0%B5_%D1%88%D0%B0%D0%B3%D0%B8) и [Строительные блоки](ru/docs/Learn/JavaScript/Building_blocks)) и основы OOJS (см. [Введение в объекты](/ru/docs/Learn/JavaScript/%D0%9E%D0%B1%D1%8A%D0%B5%D0%BA%D1%82%D1%8B)). |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Цель:               | Понять прототипы объектов JavaScript, как работают прототипные цепочки и как добавить новые методы в `prototype` свойство.                                                                                                                                                                                                                                                                     |

## Язык основанный на прототипах?

JavaScript часто описывают как язык **прототипного наследования** — каждый объект, имеет **объект-прототип**, который выступает как шаблон, от которого объект наследует методы и свойства. Объект-прототип так же может иметь свой прототип и наследовать его свойства и методы и так далее. Это часто называется **цепочкой прототипов** и объясняет почему одним объектам доступны свойства и методы которые определены в других объектах.

Точнее, свойства и методы определяются в свойстве `prototype` функции-конструктора объектов, а не в самих объектах.

В JavaScript создаётся связь между экземпляром объекта и его прототипом (свойство `__proto__`, которое является производным от свойства `prototype` конструктора), а свойства и методы обнаруживаются при переходе по цепочке прототипов.

> **Примечание:** Важно понимать, что существует различие между прототипом объекта (который доступен через [`Object.getPrototypeOf(obj)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf) или через устаревшее свойство [`__proto__`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)) и свойством `prototype` в функциях-конструкторах. Первое свойство является свойством каждого экземпляра, а второе - свойством конструктора. То есть `Object.getPrototypeOf(new Foobar())` относится к тому же объекту, что и `Foobar.prototype`.

Давайте посмотрим на пример, чтобы стало понятнее.

## Понимание прототипа объектов

Вернёмся к примеру, когда мы закончили писать наш конструктор `Person()`- загрузите пример в свой браузер. Если у вас ещё нет работы от последней статьи, используйте наш пример [oojs-class-further-exercises.html](http://mdn.github.io/learning-area/javascript/oojs/introduction/oojs-class-further-exercises.html) (см. Также [исходный код](https://github.com/mdn/learning-area/blob/master/javascript/oojs/introduction/oojs-class-further-exercises.html)).

В этом примере мы определили конструктору функцию, например:

```js
function Person(first, last, age, gender, interests) {

  // Определения методов и свойств
  this.name = {
    'first': first,
    'last' : last
  };
  this.age = age;
  this.gender = gender;
  //...см. Введение в объекты для полного определения
}
```

Затем мы создаём экземпляр объекта следующим образом:

```js
var person1 = new Person('Bob', 'Smith', 32, 'male', ['music', 'skiing']);
```

Если вы наберёте «`person1.`» в вашей консоли JavaScript, вы должны увидеть, что браузер пытается автоматически заполнить это с именами участников, доступных на этом объекте:

![](https://mdn.mozillademos.org/files/13853/object-available-members.png)

В этом списке вы увидите элементы, определённые в конструкторе person 1 — Person() — `name`, `age`, `gender`, `interests`, `bio`, и `greeting`. Однако вы также увидите некоторые другие элементы — `watch`, `valueOf`и т. д. — они определены в объекте прототипа Person (), который является [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object).

![](https://mdn.mozillademos.org/files/13891/MDN-Graphics-person-person-object-2.png)

Итак, что произойдёт, если вы вызываете метод в `person1`, который фактически определён в `Object`? Например:

```js
person1.valueOf()
```

Этот метод — [`Object.valueOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf)наследуется `person1`, потому что его конструктором является `Person()`, а прототипом `Person()` является `Object()`. `valueOf()` возвращает значение вызываемого объекта — попробуйте и убедитесь! В этом случае происходит следующее:

- Сначала браузер проверяет, имеет ли объект `person1` доступный в нем метод `valueOf()`, как определено в его конструкторе `Person()`.
- Это не так, поэтому следующим шагом браузер проверяет, имеет ли прототип объекта (`Object()`) конструктора `Person()` доступный в нем метод `valueOf()`. Так оно и есть, поэтому он вызывается, и все хорошо!

> **Примечание:** Мы хотим повторить, что методы и свойства **не** копируются из одного объекта в другой в цепочке прототипов - к ним обращаются, поднимаясь по цепочке, как описано выше.

> **Примечание:** Официально нет способа получить доступ к объекту прототипа объекта напрямую - «ссылки» между элементами в цепочке определены во внутреннем свойстве, называемом `[[prototype]]` в спецификации для языка JavaScript ( см. {{glossary("ECMAScript")}}). Однако у большинства современных браузеров есть свойство, доступное для них под названием [`__proto__`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)[ ](/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/proto)(это 2 подчёркивания с обеих сторон), который содержит объект-прототип объекта-конструктора. Например, попробуйте `person1.__proto__` и `person1.__proto__.__proto__`, чтобы увидеть, как выглядит цепочка в коде!
>
> С ECMAScript 2015 вы можете косвенно обращаться к объекту прототипа объекта `Object.getPrototypeOf (obj)`.

## Свойство prototype: Где определены унаследованные экземпляры

Итак, где определены наследуемые свойства и методы? Если вы посмотрите на страницу со ссылкой [`Object`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object), вы увидите в левой части большое количество свойств и методов - это намного больше, чем количество унаследованных членов, доступных для объекта `person1`. Некоторые из них унаследованы, а некоторые нет - почему это?

Как упоминалось выше, наследованные свойства это те, что определены в свойстве `prototype` (вы можете называть это подпространством имён), то есть те, которые начинаются с `Object.prototype.`, а не те, которые начинаются с простого `Object`. Значение свойства `prototype` - это объект, который в основном представляет собой контейнер для хранения свойств и методов, которые мы хотим наследовать объектами, расположенными дальше по цепочке прототипов.

Таким образом [`Object.prototype.watch()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/watch), [`Object.prototype.valueOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/valueOf) и т. д. доступны для любых типов объектов, которые наследуются от `Object.prototype`, включая новые экземпляры объектов, созданные из конструктора `Person()` .

[`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is), [`Object.keys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) и другие члены, не определённые в контейнере `prototype`, не наследуются экземплярами объектов или типами объектов, которые наследуются от `Object.prototype`. Это методы / свойства, доступные только в конструкторе `Object()`.

> **Примечание:** Это кажется странным - как у вас есть метод, определённый для конструктора, который сам по себе является функцией? Ну, функция также является типом объекта - см. Ссылку на конструктор [`Function()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function), если вы нам не верите.

1.  Вы можете проверить существующие свойства прототипа для себя - вернитесь к нашему предыдущему примеру и попробуйте ввести следующее в консоль JavaScript:

    ```js
    Person.prototype
    ```

2.  Результат покажет вам не много, ведь мы ничего не определили в прототипе нашего конструктора! По умолчанию `prototype` конструктора всегда пуст. Теперь попробуйте следующее:

    ```js
    Object.prototype
    ```

Вы увидите большое количество методов, определённых для свойства `prototype` `Object`'а , которые затем доступны для объектов, которые наследуются от `Object`, как показано выше.

Вы увидите другие примеры наследования цепочек прототипов по всему JavaScript - попробуйте найти методы и свойства, определённые на прототипе глобальных объектов [`String`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String), [`Date`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date), [`Number`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) и [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array), например. Все они имеют несколько элементов, определённых на их прототипе, поэтому, например, когда вы создаёте строку, вот так:

```js
var myString = 'This is my string.';
```

В `myString` сразу есть множество полезных методов, таких как [`split()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split), [`indexOf()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf), [`replace()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace) и т. д.

> **Предупреждение:** **Важно**: Свойство `prototype` является одной из наиболее противоречивых названий частей JavaScript - вы можете подумать, что `this` указывает на объект прототипа текущего объекта, но это не так (это внутренний объект, к которому можно получить доступ `__proto__`, помните ?). `prototype` вместо этого - свойство, содержащее объект, на котором вы определяете членов, которые вы хотите наследовать.

## Снова create()

Ранее мы показали, как метод [`Object.create()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) может использоваться для создания нового экземпляра объекта.

1.  Например, попробуйте это в консоли JavaScript предыдущего примера:

    ```js
    var person2 = Object.create(person1);
    ```

2.  На самом деле `create()`создаёт новый объект из указанного объекта-прототипа. Здесь `person2` создаётся с помощью `person1` в качестве объекта-прототипа. Это можно проверить, введя в консоли следующее:

    ```js
    person2.__proto__
    ```

Это вернёт объект person1.

## Свойство constructor

Каждая функция-конструктор имеет свойство `prototype`, значением которого является объект, содержащий свойство [`constructor`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor). Это свойство `constructor` указывает на исходную функцию-конструктор. Как вы увидите в следующем разделе, свойства, определённые в свойстве `Person.prototype` (или в общем случае в качестве свойства прототипа функции конструктора, который является объектом, как указано в предыдущем разделе) становятся доступными для всех объектов экземпляра, созданных с помощью конструктор `Person()`. Следовательно, свойство конструктора также доступно для объектов `person1` и `person2`.

1.  Например, попробуйте эти команды в консоли:

    ```js
    person1.constructor
    person2.constructor
    ```

    Они должны возвращать конструктор `Person()`, поскольку он содержит исходное определение этих экземпляров.

    Хитрый трюк заключается в том, что вы можете поместить круглые скобки в конец свойства `constructor` (содержащие любые требуемые параметры) для создания другого экземпляра объекта из этого конструктора. Конструктор - это функция в конце концов, поэтому её можно вызвать с помощью круглых скобок; вам просто нужно включить ключевое слово `new`, чтобы указать, что вы хотите использовать эту функцию в качестве конструктора.

2.  Попробуйте это в консоли:

    ```js
    var person3 = new person1.constructor('Karen', 'Stephenson', 26, 'female', ['playing drums', 'mountain climbing']);
    ```

3.  Теперь попробуйте получить доступ к функциям вашего нового объекта, например:

    ```js
    person3.name.first
    person3.age
    person3.bio()
    ```

Это хорошо работает. Вам не нужно будет использовать его часто, но это может быть действительно полезно, если вы хотите создать новый экземпляр и не имеете ссылки на исходный конструктор, который легко доступен по какой-либо причине.

Свойство [`constructor`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/constructor) имеет другие применения. Например, если у вас есть экземпляр объекта и вы хотите вернуть имя конструктора этого экземпляра, вы можете использовать следующее:

```js
instanceName.constructor.name
```

Например, попробуйте это:

```js
person1.constructor.name
```

> **Примечание:** Значение `constructor.name` может измениться (из-за прототипического наследования, привязки, препроцессоров, транспилеров и т. д.), Поэтому для более сложных примеров вы захотите использовать оператор [`instanceof`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/instanceof).

## Изменение прототипов

Давайте рассмотрим пример изменения свойства `prototype` функции-конструктора — методы, добавленные в прототип, затем доступны для всех экземпляров объектов, созданных из конструктора.

1.  Вернитесь к нашему примеру [oojs-class-further-exercises.html](http://mdn.github.io/learning-area/javascript/oojs/introduction/oojs-class-further-exercises.html) и создайте локальную копию [исходного кода](https://github.com/mdn/learning-area/blob/master/javascript/oojs/introduction/oojs-class-further-exercises.html). Ниже существующего JavaScript добавьте следующий код, который добавляет новый метод в свойство `prototype` конструктора:

    ```js
    Person.prototype.farewell = function() {
      alert(this.name.first + ' has left the building. Bye for now!');
    };
    ```

2.  Сохраните код и загрузите страницу в браузере и попробуйте ввести следующее в текстовый ввод:

    ```js
    person1.farewell();
    ```

Должно появиться всплывающее окно, с именем пользователя, определённым в конструкторе. Это действительно полезно, но ещё более полезно то, что вся цепочка наследования обновляется динамически, автоматически делая этот новый метод доступным для всех экземпляров объектов, полученных из конструктора.

Подумайте об этом на мгновение. В нашем коде мы определяем конструктор, затем мы создаём экземпляр объекта из конструктора, _затем_ добавляем новый метод к прототипу конструктора:

```js
function Person(first, last, age, gender, interests) {

  // определения свойств и методов

}

var person1 = new Person('Tammi', 'Smith', 32, 'neutral', ['music', 'skiing', 'kickboxing']);

Person.prototype.farewell = function() {
  alert(this.name.first + ' has left the building. Bye for now!');
};
```

Но метод `farewell()` _по-прежнему_ доступен в экземпляре объекта `person1` - его элементы были автоматически обновлены, чтобы включить недавно определённый метод `farewell()`.

> **Примечание:** Если у вас возникли проблемы с получением этого примера для работы, посмотрите на наш пример [oojs-class-prototype.html](https://github.com/mdn/learning-area/blob/master/javascript/oojs/advanced/oojs-class-prototype.html) (см. также это [running live](http://mdn.github.io/learning-area/javascript/oojs/advanced/oojs-class-prototype.html)).

Вы редко увидите свойства, определённые в свойстве `prototype`, потому что они не очень гибки при таком определении. Например, вы можете добавить свойство следующим образом:

```js
Person.prototype.fullName = 'Bob Smith';
```

Это не очень гибко, так как человека нельзя назвать так. Было бы намного лучше сделать это, создав `fullName` из `name.first` и `name.last`:

```js
Person.prototype.fullName = this.name.first + ' ' + this.name.last;
```

Однако это не работает, поскольку в этом случае `this` будет ссылаться на глобальную область, а не на область функции. Вызов этого свойства вернёт `undefined undefined`. Это отлично работало с методом, который мы определили ранее в прототипе, потому что он находится внутри области функций, которая будет успешно перенесена в область экземпляра объекта. Таким образом, вы можете определить постоянные свойства прототипа (т. е. те, которые никогда не нуждаются в изменении), но обычно лучше определять свойства внутри конструктора.

Фактически, довольно распространённый шаблон для большего количества определений объектов - это определение свойств внутри конструктора и методов в прототипе. Это упрощает чтение кода, поскольку конструктор содержит только определения свойств, а методы разделены на отдельные блоки. Например:

```js
// Определение конструктора и его свойств

function Test(a, b, c, d) {
  // определение свойств...
}

// Определение первого метода

Test.prototype.x = function() { ... };

// Определение второго метода

Test.prototype.y = function() { ... };

//...и так далее
```

Этот образец можно увидеть в действии в примере [приложения плана школы ](https://github.com/zalun/school-plan-app/blob/master/stage9/js/index.js)Петра Залевы.

## Резюме

В этой статье рассмотрены прототипы объектов JavaScript (в том числе и то, как прототип цепочки объектов позволяет объектам наследовать функции друг от друга), свойство прототипа и как его можно использовать для добавления методов к конструкторам и другие связанные с этой статьёй темы.

В следующей статье мы рассмотрим то, как вы можете реализовать наследование функциональности между двумя собственными настраиваемыми объектами.

{{PreviousMenuNext("Learn/JavaScript/Objects/Object-oriented_JS", "Learn/JavaScript/Objects/Inheritance", "Learn/JavaScript/Objects")}}

## В этом модуле

- [Основы объекта](/ru/docs/Learn/JavaScript/Объекты/Основы)
- [Объектно-ориентированный JavaScript для начинающих](/ru/docs/Learn/JavaScript/Объекты/Object-oriented_JS)
- [Прототипы объектов](/ru/docs/Learn/JavaScript/Объекты/Object_prototypes)
- [Наследование в JavaScript](/ru/docs/Learn/JavaScript/Объекты/Inheritance)
- [Работа с данными JSON](/ru/docs/Learn/JavaScript/Объекты/JSON)
- [Практика построения объектов](/ru/docs/Learn/JavaScript/Объекты/Object_building_practice)
- [Добавление функций в нашу демонстрацию прыгающих шаров](/ru/docs/Learn/JavaScript/Объекты/Adding_bouncing_balls_features)
