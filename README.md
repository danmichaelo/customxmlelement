QuiteSimpleXMLElement
===============

[![Build Status](http://img.shields.io/travis/danmichaelo/quitesimplexmlelement.svg?style=flat-square)](https://travis-ci.org/danmichaelo/quitesimplexmlelement)
[![Coverage Status](http://img.shields.io/coveralls/danmichaelo/quitesimplexmlelement.svg?style=flat-square)](https://coveralls.io/r/danmichaelo/quitesimplexmlelement?branch=master)
[![Code Quality](http://img.shields.io/scrutinizer/g/danmichaelo/quitesimplexmlelement/master.svg?style=flat-square)](https://scrutinizer-ci.com/g/danmichaelo/quitesimplexmlelement/?branch=master)
[![Latest Stable Version](http://img.shields.io/packagist/v/danmichaelo/quitesimplexmlelement.svg?style=flat-square)](https://packagist.org/packages/danmichaelo/quitesimplexmlelement)
[![Total Downloads](http://img.shields.io/packagist/dt/danmichaelo/quitesimplexmlelement.svg?style=flat-square)](https://packagist.org/packages/danmichaelo/quitesimplexmlelement)


The `QuiteSimpleXMLElement` class is a small wrapper around the `SimpleXMLElement` class. It was formerly known as `CustomXMLElement`. The main reason for developing the class was to let objects returned by the `xpath()`
method inherit namespaces from the original object.

Taking an example document,

```php
$xml = '<root xmlns:dc="http://purl.org/dc/elements/1.1/">
    <dc:a>
      <dc:b >
        1.9
      </dc:a>
    </dc:b>
  </root>';
```

Using SimpleXMLElement I found myself having to register namespaces over and over again:

```php
$root = new SimpleXMLElement($xml);
$root->registerXPathNamespace('d', 'http://purl.org/dc/elements/1.1/');
$a = $root->xpath('d:a');
$a[0]->registerXPathNamespace('d', 'http://purl.org/dc/elements/1.1/');
$b = $a[0]->xpath('d:b');
echo trim((string)$b[0]);
```

QuiteSimpleXMLElement allows for slightly less typing:

```php
$root = new QuiteSimpleXMLElement($xml);
$root->registerXPathNamespace('d', 'http://purl.org/dc/elements/1.1/');
$a = $root->xpath('d:a');
$b = $a->xpath('d:b');
echo trim((string)$b[0]);
```

A note on the design: I would have preferred to extend the original SimpleXMLElement class, but the constructor is static, which is why I wrote a wrapper instead.

### Convenience methods

The library defines some new methods to support less typing and cleaner code.

#### attr($name)

Returns the value of an attribute as a string

```php
echo $node->attr('id');
```

#### text($xpath)

Returns the text content of the node

```php
echo $node->text('d:a/d:b');
```

#### first($xpath)

Returns the first node that matches the given path, or null if none.

```php
$node = $node->first('d:a/d:b');
```

#### all($xpath)

Returns all nodes that matches the given path, or an empty array if none.

```php
$node = $node->all('d:a/d:b');
```

#### has($xpath)

Returns true if the node exists, false if not

```php
if ($node->has('d:a/d:b') {
	…
}
```

#### count($namespace=null)

Returns the number of child nodes, optionally within a given namespace.

```php
if ($node->count('d') > 0) {
  …
}
```
