# Compact extended Backus Naur Form
## Lilly H. St Claire

### Goal

The goal of CBNF is to be an extension to
backus naur form that has few features while
still being rather elegant, it stems from
my love of BNF and my distaste to some of
the changes in EBNF, while liking the feature
set of ABNF. It is, admittedly a personal
failure of mine that I cannot use EBNF well.
But a failure that can be solved nonetheless.

### Rule definition

#### Rule naming

The name of a rule is simply the identifier used
to refer to the rule within other rules. A rule
name is encased in angle brackets (`<` and `>`)
and contain any character falling within the
ASCII codes 0x30 and 0x39 or 0x41 and 0x5A or
0x61 and 0x7A (or in the case of non ASCII
encodings their symbolic equivalents).

It should be noted that rule names cannot start
with space.

#### Rule form

A rule is defined as follows (in BNF):

```BNF
<rule> ::= <rule name> "::=" <operations> <NL>
```

where \<operations\> is the series of rule names
terminals, as well as branches and macros, and
\<NL\> is the line feed character (0xA in ASCII).

#### Terminal tokens

CBNF contains the following terminal tokens:

- integers: a number value with a chosen base representing
            a single character
- strings:  a number of characters enclosed between double
            quotes.
- ranges:   a character which numeric representation is between
            two integers

Integers are defined as the following:

```BNF
<integer> ::= <decimal> | <hex> | <binary>
<decimal> ::= <digit> | <digit> <decimal>
<hex>     ::= "$" <hexp>
<hexp>    ::= <hexdig> | <hexdig> <hexp>
<binary>  ::= "b$" <binp>
<binp>    ::= <bit> | <bit> <binp>
<digit>   ::= "0" | "1" | "2" | "3" | "4" |
              "5" | "6" | "7" | "8" | "9"
<hexdig>  ::= "a" | "b" | "c" | "d" | "e" | "f" |
              "A" | "B" | "C" | "D" | "E" | "F" | <digit>
<bit>     ::= "0" | "1"
```

And strings as so:

```BNF
<string> ::= '"' <mchar> '"'
<mchar>  ::= <char> | <char> <mchar>
<char>   ::= <letter> | <digit> | <symbol>
<letter> ::= "A" | "B" | "C" | "D" | "E" |
             "F" | "G" | "H" | "I" | "J" |
             "K" | "L" | "M" | "N" | "O" |
             "P" | "Q" | "R" | "S" | "T" |
             "U" | "V" | "W" | "X" | "Y" |
             "Z" |
             "a" | "b" | "c" | "d" | "e" |
             "f" | "g" | "h" | "i" | "j" |
             "k" | "l" | "m" | "n" | "o" |
             "p" | "q" | "r" | "s" | "t" |
             "u" | "v" | "w" | "x" | "y" |
             "z"
<symbol> ::= "|" | " " | "!" | "#" | "$" |
             "%" | "&" | "(" | ")" | "*" |
             "*" | "+" | "," | "-" | "." |
             "/" | ":" | ";" | ">" | "=" |
             "<" | "?" | "@" | "[" | "\" |
             "]" | "^" | "_" | "`" | "{" |
             "}" | "~"
```

and ranges like so:

```BNF
<range>    ::= <binrange> | <decrange> | <hexrange>
<binrange> ::= <binary> "-" <binp>
<decrange> ::= <decimal> "-" <decimal>
<hexrange> ::= <hex> "-" <hexp>
```

it should be noted that ranges are inclusive of both
the minimum and maximum.

#### Branches

In CBNF Branches are exactly equivalent to
BNF "|", it is formatted as follows:

```
<branch> ::= <operations> "|" <operations>
```

#### Macros

Macros in CBNF are defined as any symbol series
which is equivalent to some number of recursive
branch/rule lists. There are the following macros:

- Optional operations
- Repetition of operations
- Grouped operations

#### Optional operations

An optional operation maps to two operations which
are branched between.

Optional operations are written like so:

```BNF
<optional> ::= "[" <operations> "]"
```

We can see the mapping behaviour as so:

```CBNF
<rule> ::= <a> [<b>]
```

maps to:

```BNF
<rule> ::= <a> | <a> <b>
```

#### Repeated operations

A repeated operation maps to a recursive branch sequence
like so `<rule> ::= *<a>` -> `<rule> ::= <a> | <a> <rule>`

The syntax is like so:

```BNF
<recursive repetition> ::= "*" <operations>
```

We may also specify a minimum and maximum repetition
like so, these can be mapped as
```CBNF
<rule> ::= 3*<a>
```

->

```BNF
<rule> ::= <a> <a> <a> | <a> <a> | <a>
```

```CBNF
<rule> ::= *3<a>
```

->

```BNF
<rule> ::= <a> <a> <subrule>
<subrule> ::= <a> | <a> <subrule>
```

and

```CBNF
<rule> ::= 4*2<a>
```

->

```BNF
<rule> ::= <a> <a> | <a> <a> <a> | <a> <a> <a> <a>
```

with the BNF as follows:

```BNF
<repetition> ::= <recursive repetition> | <minrep> | <maxrep> |
                 <minmaxrep>
<minrep>     ::= "*" <mdig> <operations>
<maxrep>     ::= <mdig> "*" <operations>
<minmaxrep>  ::= <mdig> "*" <mdig> <operations>
<mdig>       ::= <digit> | <digit> <mdig>
```

#### Grouped operations

Grouped operations simply allow for operations that would
regularly be there own rules can be placed within a larger
rule. They map like so:

```CBNF
<rule> ::= <a> | *(<b> | <c>) | <d>
```

->

```BNF
<rule> ::= <a> | <subrule> | <d>
<subrule> ::= <ssubrule> | <ssubrule> <subrule>
<ssubrule> ::= <b> | <c>
```

the syntax is like so:

```CBNF
<group> ::= "(" <operations> ")"
```

### Comments

As a simple footnote ";" is used for comments and may
be placed at the end of a rule, or instead of a rule

### The final test

We shall now define CBNF in terms of CBNF, in order to
give not only a lengthy example but also to prove itself
as usable enough to do so.

```CBNF
<comment>     ::= ";" [*$20-7E]
<spec>        ::= [*(<rule> | <comment>)]
<rule>        ::= <rule name> <ws> "::=" <ws> <operation> <comment> <nl>
<rule name>   ::= "<" <idenchar> [*(<idenchar> | " ")] ">"
<operation>   ::= <branch> | <terminal> | <macro> | <concat>
<concat>      ::= <operation> <ws> <operation>
<branch>      ::= <operation> <ws> "|" <ws> <operation>
<macro>       ::= (<group> | <repetition> | <optional>)
<group>       ::= "(" <ws> <operation> <ws> ")"
<repetition>  ::= [*<digit>] "*" [*<digit>] (<group> | <optional>
                | <terminal>)
<optional>    ::= "[" <ws> <operation> <ws> "]"
<terminal>    ::= <rule name> | <integer> | <string> | <range>
<integer>     ::= <decimal> | <hexadecimal> | <binary>
<decimal>     ::= *<digit>
<hexadecimal> ::= "$" *<hexdigit>
<binary>      ::= "b$" *<bit>
<digit>       ::= $30-39
<hexdigit>    ::= <digit> | $41-47 | $61-67
<bit>         ::= $30-31
<string>      ::= $22 *($20-$7E) $22
<range>       ::= <decrange> | <hexrange> | <binrange>
<decrange>    ::= *<digit> "-" *<digit>
<hexrange>    ::= "$" *<hexdigit> "-" *<hexdigit>
<binrange>    ::= "b$" *<bit> "-" <bit>
<idenchar>    ::= $30-39 | $41-5A | $61-7A
<ws>          ::= *(" " | $9)
<nl>          ::= $A
```
















