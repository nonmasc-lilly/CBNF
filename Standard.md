# Compact extended Backus Naur Form (CBNF 1.0)
## Lilly H. St Claire

CBNF is a simple alternative to ABNF, EBNF with a greater focus on BNF similarity. This is mostly for the purposes of creating a more (subjectively) appealing
form.

A syntactic variable is written as:

    <variable name>

We may define the contents of a syntactic variable as so:

    <variable> ::= <production rule>

Where a production rule is a combination of the following productions:

    <a>

        The syntactic variable <a>.

    <a> <b>

        A concatenation of the production rules <a> and <b> delimeted with some amount of optional whitespace.

    <a> . <b>

        A direct concatenation of the production rules <a> and <b> without whitespace.

    <a> | <b>

        A decision between either the production rules <a> or <b>.

    [<a>]

        An optional production rule <a>.

    {<a>}

        A repeated production rule <a> with implied whitespace similar to implicit concatenation `<a> <b>'.

    *<a>

        {<a>} without implicit spaces.

    (<a>)

        A grouped production rule <a>.

    "a"

        The literal string `a'.

    $0A

        The hexadecimal value for ten.

    10

        The integer literal ten.

    <a>-<b>

        The literal representing all values between the integer literal <a> and the integer iteral <b> (inclusive).

For any production rule containing only explicit production rules (`*', `.' or '|') between only syntactic variables (no literals) any and all angle
parentheticals may be replaced with an enclosing set of the appropriate parentheticals (`[ ]', `{ }', '( )', or `< >') i.e., `<a | b>', `[a . b | c]'.

i.e., the Pascal like program syntax as described at: https://en.wikipedia.org/wiki/Extended\_Backus-Naur\_form

    <program>               ::= "PROGRAM" <identifier> "BEGIN" {<assignment> . ";"} "END."
    <identifier>            ::= <alphabetic character> . *(alphabetic character | digit)
    <number>                ::= ["-"] . <*digit>
    <string>                ::= $22 <*character> $22
    <assinment>             ::= <identifier> . ":=" . (number | identifier | string)
    <alphabetic character>  ::= $41-$5A | $61-$7A
    <digit>                 ::= $30-$39
    <all characters>        ::= $20-$7E

Such that a correct program would be:

    PROGRAM DEMO1
    BEGIN
        A:=3;
        B:=45;
        H:=-100023;
        C:=A;
        D123:=B34A;
        BABOON:=GIRAFFE;
        TEXT:="Hello World!";
    END.
