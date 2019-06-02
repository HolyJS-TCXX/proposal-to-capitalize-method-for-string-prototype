# toCapitalize() method for String.prototype

Elena Pravdina
Stage: pre-proposal (-1)

## Contents

1. [Description](#description)
2. [Motivation / Problem](#motivation--problem)
3. [High-level API / Examples](#high-level-api--examples)
4. [Polyfill](#polyfill)
5. [Potential issues and open questions](#potential-issues-and-open-questions)
6. [Development history](#development-history)
7. [FAQ](#faq)

## Description

The `toCapitalize()` method returns the calling string value converted to capitalize.
Allows to transform the first letter of each word to upper case or the first letter of all string only.

Method does not affect the value of the string itself.

Similar with `String.prototype.toLowerCase()` and `String.prototype.toUpperCase()`.

## Motivation / Problem

The capitalize conversion is one of the common developers tasks for many popular languages. Nowadays headlines output, first/last names presentation, data normalization requires chains of several more low-level string manipulation (like splitting by words, transforming first char to uppercase, concatenating the results) or induce to third-party libraries.

The difficulty of such conversion is inconsistent with more high-level `toLowerCase()`/`toUpperCase()` opportunities.

The lack of such method is inconsistent with [CSS Level 1 `text-transform`](https://www.w3.org/TR/CSS1/#text-transform "CSS Level 1 text-transform") property providing all of the three — lowercase, uppercase and capitalize values.

Howerever CSS does not reduce the necessity of `toCapitalize()` due to cases of alerts, native modal messages, server-side and similar non-styling environments.

## High-level API / Examples

Method transforms the first letter of each word into the string to upper case. Other letters will be converted to lower case.

If the optional parameter `firstLetterOnly` is presented only first letter of all string will be transformed to upper case.
 
 ```js
str.toCapitalize([firstLetterOnly])
```

String conversion examples:

```js
const str = 'the best news ever'.toCapitalize();
console.log(srt);  // 'The Best News Ever'
```
```js
const nameRu = 'иван иванов '.toCapitalize();
console.log(nameRu);  // 'Иван Иванов'
```

```js
const diffCases = 'Mr. sMith'.toCapitalize();
console.log(diffCases);  // 'Mr. Smith'
```

```js
const firstNameEn = 'john'.toCapitalize();
console.log(firstNameEn);  // 'John'
```

First-letter conversion examples:

```js
const str = 'the best news ever'.toCapitalize(true);
console.log(srt);  // 'The best news ever'
```
```js
const firstNameEn = 'john'.toCapitalize(true);
console.log(firstNameEn);  // 'John'
```

## Polyfill

##### For `String.prototype.toCapitalize()`:

###### native
```javascript
function toCapitalize(str) {
	return str
		.toLowerCase()
		.split(' ')
		.map(word => word.charAt(0).toUpperCase() + word.substr(1))
		.join(' ');
 }
```

###### lodash
[startCase()](https://lodash.com/docs/4.17.11#startCase "startCase")

##### For `String.prototype.toCapitalize(true)`:

###### native
```javascript
function toCapitalize(str, firstLetterOnly) {
	return str.charAt(0).toUpperCase() + str.substr(1).toLowerCase();
}
```

###### lodash
[capitalize()](https://lodash.com/docs/4.17.11#capitalize "capitalize") 

## Potential issues and open questions

The interface with optional parameter `toCapitalize(true)` may looks uncertain, alternative aproach with own separate method (i.e. `toCapitalizeFirst()`) may looks more transparent and convenient.

In some cases capitalizing process may desire to except for articles, short prepositions and conjunctions (following the [Title case](https://en.wikipedia.org/wiki/Letter_case#Stylistic_or_specialised_usage "Title case")). Anoter one optional parameter or the own separate metod should be designed to extend `toCapitalize()` behaviour.

The locale case mapping may require to implement `toLocaleCapitalize()` simultaneously with the `toCapitalize()` (like  `toLocaleLowerCase()` and `toLocaleUpperCase()` do).


## Development history

24.05.2019 pre-proposal (-1)

## FAQ
- Does method take into account locale-specific case mappings?

> No, for such case another String.prototype method `toLocaleCapitalize()` should be implemented. Similar with `toLocaleLowerCase()` and `toLocaleUpperCase()`.
