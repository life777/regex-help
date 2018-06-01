# Regex Cheatsheet (JavaScript)

|Menu                                   |
|---------------------------------------|
|[Amount of symbols](#amount-of-symbols)|
|[Greedy/non-greedy search](#greedynon-greedy-search)              |
|[Beginning or end of the string](#beginning-or-end-of-the-string)       |
|[Lookahead and lookbehind](#lookahead-and-lookbehind)             |
|[Sets of symbols](#sets-of-symbols)    |
|[Special symbols](#special-symbols)    |
|[Named capture groups](#named-capture-groups)                                        |
|[JavaScript Regex flags](#javascript-regex-flags)               |

## Amount of symbols

|Symbol |Description                        |
|-------|-----------------------------------|
|?      |1 or 0 times (the same as {0,1})   |
|+      |1 or many times (the same as {1,}) |
|*      |0 or many times (the same as {0,}) |
|{n,m}  |n <= k <= m times                  |

**Examples**

> All examples use flag `g` for global search.

In string `Hello world` will be found:

    /le?/ - 3 letters "l"
    /el?/ - part "el"

    /le+/ - nothing
    /el+/ - part "ell"

    /le*/ - 3 letters "l"

    /ll{0,1}/ - parts "ll" and "l"

## Greedy/non-greedy search

By default all Regexes are greedy. To make non-greedy search `?` should be used after  any of the quantifiers *, +, ?, or {}.

**Examples**

In string `Hello world` will be found:

    /el?/ - part "el"
    /el??/ - part "e"

    /el+/ - part "ell"
    /el+?/ - part "el"

    /el*/ - part "ell"
    /el*?/ - letter "e"

    /l{1,2}/ - parts "ll" and "l"
    /l{1,2}?/ - 3 letters "l"

## Beginning or end of the string

|Symbol |Description                        |
|-------|-----------------------------------|
|^      |Matches beginning of the string    |
|$      |Matches end of the string          |

**Examples**

In string `hello hard world` will be found:

    /h/ - 2 letters "h"
    /^h/ - first letter "h"

    /d/ - 2 letters "d"
    /d$/ - last letter "d"

## Lookahead and lookbehind

|Symbol |Description                        |
|-------|-----------------------------------|
|(?=)   |Positive lookahead                 |
|(!?)   |Negative lookahead                 |
|(?<=)  |Positive lookbehind                |
|(?<!)  |Negative lookbehind                |

> Lookbehind was added only in ES2018, be careful with using it

**Examples**

In string `a1ba2ba3b` will be found:

    /b(?=a2|a3)/ - 2 letters "b"
    /b(?!a2|a3)/ - last letter "b"

    /(?<=a1|a2)b/ - 2 letters "d"
    /(?<!a1|a2)b/ - last letter "d"

## Sets of symbols

|Symbol |Description                        |
|-------|-----------------------------------|
|()     |Group set of symbols               |
|(?:)   |Group set of symbols, but don't remember|
|[]     |Enumerate possible symbols (or their absence)|
|.      |Any symbol                         |
|\|     |Operator OR                        |

**Examples**

In string `barfoooooobar` will be found:

    /foo{1,2}/ - part "fooo"
    /(foo){1,2}/ - part "foo"
    /(?:foo){1,2}/ - part "foo"

    /[^a-o]/ - 2 letters "r"
    /[abc]/ - 4 letters "b", "a", "b" and "a" 

## Special symbols

|Symbol |Description                        |
|-------|-----------------------------------|
|.      |Any symbol                         |
|\b     |Word boundary                      |
|\B     |Non-word boundary                  |
|\cX    |Control character                  |
|\d     |Digit character                    |
|\D     |Non-digit character                |
|\w     |Alphanumeric character including the underscore                                  |
|\W     |Non-word character                 |
|\s     |single white space character, including space, tab, form feed, line feed  |
|\S     |Single character other than white space   |
|\t     |Tab                                |
|\v     |Vertical tab                       |
|\f     |Line feed                          |
|\n     |New line                           |
|\r     |Сarriage return                    |
|\1     |Back reference to first () group (can be used any integer)                   |
|\xhh   |Character with the code hh         |
|\uhhhh |Character with the code hhhh       |
|\u{hhhhh}|*Character with the Unicode value hhhhh   |
|\p{X}  |*Symbols that are included in group X       |

> *Works only with u flag and in ES2018

**Examples**

In string `Hello World_1` will be found:

    /\bW/ - letter "W"
    /\w\d/ - part "_1"
    /\Bd/ - letter "d"

In string `💏🤳` selfie emoji can be found with `u` flag: 

    /\u{1f933}/ - emoji "🤳"

In string `πüé HelloWorld` will be found with `u` flag: 

    /\p{White_Space}/ - space " "
    /\p{Letter}/ - all letters in phrase
    /\p{Script=Greek}/ - letter "π"
    /\p{Script=Latin}/ - letters "ü", "é", "H", "e", "l", "l", "o", "W", "o", "r", "l", "d"

> To see all possible aliases for different groups of symbols [go here](http://unicode.org/Public/UNIDATA/PropertyValueAliases.txt)


## Named capture groups

To give a name to the group inside your Regex you should use this syntax: (?\<SomeName\>), to use this group inside the same Regex use this syntax: \k\<SomeName\>.

**Examples**

In string `2018-05-22` will be found:

    /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/ - year, month and day and can be reached by the same names.

In string `abc!abc` will be found:

    /^(?<word>\w+)!\k<word>$/ - whole phrase
    /^(\w+)!\1$/ - whole phrase

## JavaScript Regex flags

|Symbol |Description                        |
|-------|-----------------------------------|
|g      |Global search                      |
|m      |Multiline search                   |
|i      |Case insensitive search            |
|u*     |Full unicode support               |
|y      |Sticky mode (starts from lastIndex)|
|s*     |Dot matches all                    |

> *Works only in ES2018

**Examples**

In string `foo💩bar` will be found:

    /foo.bar/ - nothing
    /foo.bar/u - whole string
    /[💩-💫]/ - error (it can't be compiled)
    /[💩-💫]/u - 💩 symbol

In string `abc!abc` will be found:

    /abc/ - first part "abc"
    /abc/g - all parts "abc"

In string `abc\nabc` will be found:

    /abc./ - nothing ("." is any symbol, but new line)
    /abc./s - first part "abc\n"