# CBNF
## Compact extended Backus Naur Form.
### Abridged version
#### Lilly H. St Claire

CBNF aims to be a simple standin for EBNF
that is subjectively less fugly. Sorry EBNF
I dont like you :< (which is a skill issue
but whatever). CBNF while conversed a little
jokingly here is defined more formally in
the unabridged version.

## Syntax

- rule: `<name> ::= <rule definition>`
- branch: `<a> | <b>`
- group: `(<a>)`
- option: `[<a>]`
- repetition: `<max>*<min><a>`

## Description

Simply put it consists of named rules, which
may contain branches, macros, or terminals.

Branches consist of a pair of either branches
macros or terminals which one of which may be
chosen for a rule.

Terminals are either rule names, strings, integers
or ranges (which themselves are a set of all integers
between two numbers as well as those two numbers).

Macros are either groups, options, or repetitions.

A group is simply a subrule, meaning that the
contents of a group are treated the same way a
rule name should be.

An option is a group which may or may not be included
in the collapse of a rule.

And a repetition is by default 1 or more of a rule
but a minimum and maximum may be defined, to have
a rule which has zero or more appearances an optional
clause encloses a non indexed repetition.

This culminates in the CBNF of CBNF, like so:

```CBNF
<comment>     ::= ";" [*$20-$7E]
<spec>        ::= [*(<rule> | <comment>)]
<rule>        ::= <rule name> <ws> "::=" <ws> <operation> <comment> <nl>
<rule name>   ::= "<" <idenchar> [*(<idenchar> | " ")] ">"
<operation>   ::= <branch> | <terminal> | <macro>
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
<ws>          ::= " " | $9
<nl>          ::= $A
```

















