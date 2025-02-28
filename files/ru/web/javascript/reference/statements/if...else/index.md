---
title: if...else
slug: Web/JavaScript/Reference/Statements/if...else
tags:
  - Выражение
  - Условие
translation_of: Web/JavaScript/Reference/Statements/if...else
---
{{jsSidebar("Statements")}}

**Инструкция if** выполняет инструкцию, если указанное условие выполняется (истинно). Если условие не выполняется (ложно), то может быть выполнена другая инструкция.

## Синтаксис

```
if (условие)
   инструкция1
[else
   инструкция2]
```

- `условие`
  - : [Выражение](/ru/docs/Web/JavaScript/Guide/Expressions_and_Operators), которое является либо истинным, либо ложным.

<!---->

- `инструкция1`
  - : Инструкция, выполняемая в случае, если значение `"условиe"` истинно (`true`). Может быть любой инструкцией в том числе и вложенным `if`. Для группировки нескольких инструкций используется блок (`{...}`), Когда никакого действия не требуется, может использоваться [пустая](/ru/docs/Web/JavaScript/Reference/Statements/Empty) инструкция.

<!---->

- `инструкция2`
  - : Инструкция, выполняемая в случае, если значение `"условиe"` ложно (`false`). Может быть любой инструкцией, в том числе и вложенным `if`. Инструкции тоже можно группировать в блок.

## Описание

Несколько команд if ... else могут быть вложены для создания условия `else if`. Обратите внимание, что в JavaScript нет ключевого слова `elseif` (в одно слово).

```
if (условие1)
   инструкция1
else if (условие2)
   инструкция2
else if (условие3)
   инструкция3
...
else
   инструкция
```

Чтобы увидеть, как это работает, ниже представлен пример правильного вложения с отступами:

```
if (условие1)
   инструкция1
else
   if (условие2)
      инструкция2
   else
      if (условие3)
...
```

Чтобы выполнить несколько инструкций в условии, используйте блочный оператор ({...}) для группирования этих инструкций. В общем, хорошей практикой всегда является использование блочных операторов, особенно в коде, включающем вложенные операторы `if`:

```
if (условие) {
   инструкции1
} else {
   инструкции2
}
```

Не путайте примитивные логические значения `true` и `false` с правдивостью или ложностью булева объекта. Любое значение, которое не `undefined`, `null`, `0`, `NaN` или пустая строка (""), и любой объект, включая объект Boolean, значение которого является ложным, считается правдивым при использовании в качестве условия. Например:

```js
var b = new Boolean(false);
if (b) // это условие истинно
```

## Примеры

### Использование `if...else`

```js
if (cipher_char === from_char) {
   result = result + to_char;
   x++;
} else {
   result = result + clear_char;
}
```

### Использование `else if`

Обратите внимание, что в JavaScript нет синтаксиса `elseif`. Однако вы можете записать его с пробелом между `else` и `if`:

```js
if (x > 5) {

} else if (x > 50) {

} else {

}
```

### Присваивание в условном выражении

Целесообразно не использовать простые присваивания в условном выражении, потому что при взгляде на код присваивание можно путать с равенством. Например, не используйте следующий код:

```js
if (x = y) {
   /* сделать что-то */
}
```

Если вам нужно использовать присваивание в условном выражении, обычной практикой является размещение дополнительных скобок вокруг присваивания. Например:

```js
if ((x = y)) {
   /* сделать что-то */
}
```

## Спецификации

{{Specifications}}

## Совместимость браузеров

{{Compat}}

## Смотрите также

- {{jsxref("Statements/block", "block")}}
- {{jsxref("Statements/switch", "switch")}}
