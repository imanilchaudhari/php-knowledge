# Regex - Regular Expressions in PHP

Regular expressions (abbreviated regex) are sequences of characters that form
search patterns. They are mainly used in pattern matching with strings.

## Brief history

* It started in 1940s-60s with lots of smart people talking about regular expressions
* 1970s g/re/p
* 1980 Perl and Henry Spencer
* 1997 PCRE (Perl Compatible Regular Expressions)
  That's where it really took off and when we talk about regex today that's what
  we're talking about. PCRE has libraries for almost every language, it looks
  the same everywhere and it is very useful.

## Common regex usage in PHP

PHP has three main regular expression PCRE functions - [`preg_match`](http://php.net/preg_match),
[`preg_match_all`](http://php.net/preg_match_all) and [`preg_replace`](http://php.net/preg_replace).

### Matching

This returns `1` if match is found, `0` if not and `false` if error occurs:

```php
int preg_match (
    string $pattern,
    string $subject [,
    array &$matches [,
    int $flags = 0 [,
    int $offset = 0
]]])
```

This returns number of matches found:

```php
int preg_match_all (
    string $pattern,
    string $subject [,
    array &$matches [,
    int $flags = PREG_PATTERN_ORDER [,
    int $offset = 0
]]])
```

### Replacing

This returns the replaced string or array (based on the $subject):

```php
mixed preg_replace (
    mixed $pattern,
    mixed $replacement,
    mixed $subject [,
    int $limit = -1 [,
    int $count
    ]])
```

## Common regex usage in JavaScript

For comparison, regular expressions in JavaScript look pretty much the same as
in PHP.

### Matching

Returns an array of matches or null if no matches were found:

```javascript
string.match(RegExp);
```

### Replacing

Returns the string with the replacements performed:

```javascript
string.replace(RegExp, replacement);
```

### Caveats of regex in JavaScript

* No "single-line" or DOTALL mode. (The dot never matches new line.)
* No lookbehind support
* Same methods for regex and non-regex matching and replacing

## Basics of regex patterns

Let's take a look at example to find email addresses in codebase.
Our goal: `/[\w.+-]+@[a-z0-9-]+(\.[a-z0-9-]+)*/i`

### Sockets analogy

Regular expressions are built from two type of characters:

* special characters: `.\[]?*+{}()^$/`
* literals

Imagine your input strings as bolts and your pattern as a set of sockets (in order).

#### Special Characters

Let's take a look at what special characters do:

* Backslash character `\\` can escape other special character in regular expression:
* The Dot and the `\w` - `.`

    Matches everything but new lines. If you want to match a dot and only a dot
    escape it like `\`, `\w` matches letters, numbers, and the underscore
* Square brackets `[]`

    Matches characters inside the brackets. Supports ranges. Some examples:

    * `[abc]` - matches any `a`, `b` or `c`.
    * `[a-z]` Lowercase letters
    * `[0-9]` Any single digit
    * `[a-zA-Z]` - matches any lower or uppercase alphabetic character
* Optional `?`

    The `?` matches 0 or 1
* The star `*`

    The star matches 0 or more
* The Plus `+`

    Matches 1 or more
* Curly brackets `{}`

    Min and Max ranges. Some examples:

    * `{1,}` at least 1
    * `{1,3}` 1 through 3
    * `{1,64}` 1 through 64

Let's put all this together to get regex for email addresses:

```
/[\w.+-]+@[a-z0-9-]+(\.[a-z0-9-]+)*/i
```

![Regex for email](https://raw.githubusercontent.com/php-earth/PHP.earth/master/assets/images/general/regex.png "Regex for email addresses")

How this looks in PHP:

```php
preg_match_all(
    "/[\w.+-]+@[a-z0-9-]+(\.[a-z0-9-]+)*/i",
    $input_lines,
    $output_array
);
```

## Using regex for validation

Problem: make sure input is what we expect
Goal 1: `/[^\[\]\w$.]/`
Goal 2: `/^[0-9]{1,2}[dwmy]$/`

Regex is great at finding things but you need to know what you're looking for.
When you validate you get to determine exactly what you want.

### When not to use regex for validation?

Many cases are better handled with PHP's `filter_var` function. For example
validating emails should be done with PHP built-in filters:

```php
filter_var(
    'bob@example.com',
    FILTER_VALIDATE_EMAIL
)
```

### Regex validation

For starting and ending regex you use anchors:

* `^` - the hat that indicates start of the string
* `$` - the dollar sign that indicates end of string

```php
if (!preg_match("%^[0-9]{1,2}[dwmy]$%", $_POST["subscription_frequency"])) {
    $isError = true;
}
```

Negated character classes

* `[^abc]` - anything except a,b, or c, including new lines.

Example that ensures input only contains alphanumeric, dash, dot, underscore

```php
if (preg_match("/[^0-9a-z-_.]/i", $productCode)) {
    $isError = true;
}
```

## Searching and replacing

Most common PCRE functions for performing search and replace are `preg_replace()` and `preg_replace_callback()`.
But there are also `preg_filter()` and `preg_replace_callback_array()` to do almost the same.
Note that `preg_replace_callback_array()` is available since PHP7.

### Replace words in the list with something
```php
$subject = 'I want to eat some apples.';
echo preg_replace('/apple|banana|orange/', 'fruit', $subject);
```

#### Output
```
I want to eat some fruits.
```

If you have subpatterns (patterns in parentheses) in your pattern,
you can use `$N` or `\N` (where N is an integer >= 1) in replacement, this is called 'backreference'.

### Swap two numbers
```php
$subject = '7/11';
echo preg_replace('/(\d+)\/(\d+)/', '$2/$1', $subject);
```

#### Output
```
11/7
```

### Change date formatting
```php
$subject = '2001-09-11';
echo preg_replace('/(\d+)\-(\d+)\-(\d+)/', '$3/$2/$1', $subject);
```

#### Output
```
11/09/2001
```

### Simple example of replacing URL with `<a>` tag
```php
$subject = 'Please visit https://php.earth/doc for more articles.';
echo preg_replace(
    '#(https?\://([^\s\./]+(?:\.[^\s\./]+)*[^\s]*))#i',
    '<a href="$1" target="_blank">$2</a>',
    $subject
);

```

#### Output
```
Please visit <a href="https://php.earth/doc" target="_blank">php.earth/doc</a> for more articles.
```

Sometimes you may want to perform complex search and replace like filtering/sanitizing before replacing.
This is a situation where `preg_replace_callback()` may come into play.

In previous example our regex can replace only URLs begin with `http` or `https`
and now we want it to be able to replace URLs begin with `www.` too.
Someone might think we can simply change `https?\://` into subpattern like  `(?:https?\://|www\.)`
but this will not work in most browsers because they will interprete `www.domain` as a relative path.

So we need to do some works before replacing, by prepending `http://` if a URL begins with `www.`.

```php
function add_protocol_if_begins_with_www($matches)
{
    $url = strtolower($matches[1]) === 'www.'
        ? 'http://' . $matches[0]
        : $matches[0];
    return "<a href=\"{$url}\">{$matches[2]}</a>";
}

$subject = 'Please visit www.php.earth/doc for more articles.';
echo preg_replace_callback(
    '#(https?\://|www\.)([^\s\./]+(?>\.[^\s\./]+)*[^\s]*)#i',
    'add_protocol_if_begins_with_www',
    $subject
);
```

#### Output
```
Please visit <a href="http://www.php.earth/doc" target="_blank">php.earth/doc</a> for more articles.
```

Problem: Link `@mentions` and `#tags`

Goal: `/\B@([\w]{2,})/i`


## See Also

* PHP.net resources:
  * [Pattern syntax](http://www.php.net/manual/en/reference.pcre.pattern.syntax.php)
  * [Modifiers](http://www.php.net/manual/en/reference.pcre.pattern.modifiers.php)
  * [Functions](http://www.php.net/manual/en/ref.pcre.php)
* Regex online tools:
  * [Debuggex](https://www.debuggex.com/) - Online regex visualization tool.
  * [PHP Live Regex](http://www.phpliveregex.com/) - Live regular expression tester for PHP.
  * [regexper](http://regexper.com/) - Regular expression visualizer using railroad diagrams.
  * [RegExr](http://www.regexr.com/) - Learn, build and test Regular Expressions.
  * [Regex101](https://regex101.com/) - Create, debug, test and have your
    expressions explained for PHP, PCRE, JavaScript and Python. The website also
    features a community where you can share useful expressions.
* Tutorials:
  * [Demystifying RegEx with Practical Examples](http://www.sitepoint.com/demystifying-regex-with-practical-examples/)
  * [The best regex trick ever](http://www.rexegg.com/regex-best-trick.html)
* [awesome-regex](https://github.com/aloisdg/awesome-regex) - A curated collection
  of awesome regex libraries, tools, frameworks and software.
* [RegexOne](https://regexone.com/) - Learn Regular Expressions with simple,
  interactive exercises.
