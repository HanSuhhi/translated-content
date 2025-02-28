---
title: Document.evaluate()
slug: Web/API/Document/evaluate
translation_of: Web/API/Document/evaluate
---
{{ ApiRef("DOM") }}

Возвращает экземпляр объекта типа [`XPathResult`](/en-US/docs/XPathResult) исходя из данного [XPath](/ru/docs/XPath) и других входных параметров.

## Синтаксис

```
var xpathResult = document.evaluate(
 xpathExpression,
 contextNode,
 namespaceResolver,
 resultType,
 result
);
```

- `xpathExpression` - строка, описывающая XPath, который должен быть исполнен.
- `contextNode` указывает*контекстный узел* для запроса (см. \[<http://www.w3.org/TR/xpath> спецификация XPath). В качестве данного аргумента может быть задан объект _document_.
- `namespaceResolver` - функция, которой будут переданы все префиксы пространств имён. Она должна возвращать строку, описывающую URI, ассоциированный с данным префиксом. It will be used to resolve prefixes within the XPath itself, so that they can be matched with the document. `null` is common for HTML documents or when no namespace prefixes are used.
- `resultType` - число, описывающее тип возвращаемого `XPathResult` (см. ниже). Используйте [именные свойства-константы](#Result_types) конструктора класса `XPathResult` (эквивалентно численным значениям от 0 до 9), как например `XPathResult.ANY_TYPE`.
- `result` - экземпляр объекта типа `XPathResult`, используемого для хранения результатов поиска по данному `xpathExpression`. Может принимать значение `null`

## Пример

```js
var headings = document.evaluate("/html/body//h2", document, null, XPathResult.ANY_TYPE, null);
/* Найти в документе все элементы h2
 * В качестве результата будет получен узловой итератор. */
var thisHeading = headings.iterateNext();
var alertText = "В данном документе заголовками 2-го уровня являются:\n";
while (thisHeading) {
  alertText += thisHeading.textContent + "\n";
  thisHeading = headings.iterateNext();
}
alert(alertText); // Показывает alert со всеми найденными элементами h2
```

Note, in the above example, a more verbose XPath is preferred over common shortcuts such as `//h2`. Generally, more specific XPath selectors as in the above example usually gives a significant performance improvement, especially on very large documents. This is because the evaluation of the query spends does not waste time visiting unnecessary nodes. Using // is generally slow as it visits _every_ node from the root and all subnodes looking for possible matches.

Further optimization can be achieved by careful use of the context parameter. For example, if you know the content you are looking for is somewhere inside the body tag, you can use this:

```js
document.evaluate(".//h2", document.body, null, XPathResult.ANY_TYPE, null);
```

Notice in the above `document.body` has been used as the context instead of `document` so the XPath starts from the body element. (In this example, the `"."` is important to indicate that the querying should start from the context node, document.body. If the "." was left out (leaving `//h2`) the query would start from the root node (`html`) which would be more wasteful.)

Более детально данный материал описан в статье [Introduction to using XPath in JavaScript](/ru/docs/Introduction_to_using_XPath_in_JavaScript).

## Примечания

- Выражения XPath могут быть интерпретированы в HTML- и XML-документах.
- While using document.evaluate() works in FF2, in FF3 one must use someXMLDoc.evaluate() if evaluating against something other than the current document.

## Типы возвращаемых данных

These are supported values for the `resultType` parameter of the `evaluate` method:

| Result Type                    | Value | Description                                                                                                                                                                |
| ------------------------------ | ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ANY_TYPE`                     | 0     | Whatever type naturally results from the given expression.                                                                                                                 |
| `NUMBER_TYPE`                  | 1     | A result set containing a single number. Useful, for example, in an XPath expression using the `count()` function.                                                         |
| `STRING_TYPE`                  | 2     | A result set containing a single string.                                                                                                                                   |
| `BOOLEAN_TYPE`                 | 3     | A result set containing a single boolean value. Useful, for example, an an XPath expression using the `not()` function.                                                    |
| `UNORDERED_NODE_ITERATOR_TYPE` | 4     | A result set containing all the nodes matching the expression. The nodes in the result set are not necessarily in the same order they appear in the document.              |
| `ORDERED_NODE_ITERATOR_TYPE`   | 5     | A result set containing all the nodes matching the expression. The nodes in the result set are in the same order they appear in the document.                              |
| `UNORDERED_NODE_SNAPSHOT_TYPE` | 6     | A result set containing snapshots of all the nodes matching the expression. The nodes in the result set are not necessarily in the same order they appear in the document. |
| `ORDERED_NODE_SNAPSHOT_TYPE`   | 7     | A result set containing snapshots of all the nodes matching the expression. The nodes in the result set are in the same order they appear in the document.                 |
| `ANY_UNORDERED_NODE_TYPE`      | 8     | A result set containing any single node that matches the expression. The node is not necessarily the first node in the document that matches the expression.               |
| `FIRST_ORDERED_NODE_TYPE`      | 9     | A result set containing the first node in the document that matches the expression.                                                                                        |

Results of `NODE_ITERATOR` types contain references to nodes in the document. Modifying a node will invalidate the iterator. After modifying a node, attempting to iterate through the results will result in an error.

Results of `NODE_SNAPSHOT` types are snapshots, which are essentially lists of matched nodes. You can make changes to the document by altering snapshot nodes. Modifying the document doesn't invalidate the snapshot; however, if the document is changed, the snapshot may not correspond to the current state of the document, since nodes may have moved, been changed, added, or removed.

## Спецификации

{{Specifications}}

## Совместимость с браузерами

{{Compat}}

## Смотрите также

- [DOM:document.createExpression](/ru/docs/DOM/document.createExpression)
- [XPath Code Snippets](/ru/docs/Code_snippets/XPath)
- [Check for browser support](http://codepen.io/johan/full/ckFgn)
