---
title: Cooking parsers with Winnow
theme: themes/rusty.css
highlightTheme: vs
duration: 40 min
---
<base target="_blank">

# Cooking parsers with Winnow

---


### Santiago


locations 🇦🇷 🇳🇱  <!-- .element: class="fragment" data-fragment-index="0" -->

experience 🐍 🦀  <!-- .element: class="fragment" data-fragment-index="0" -->
![typescript logo](./assets/ts.png) <!-- .element: class="fragment" data-fragment-index="0" height="35px" width="35px" style="margin: 0"-->

![kpn logo](./assets/kpn-logo.svg) <!-- .element: class="fragment" data-fragment-index="1" width="200px" style="margin-right: 50px" -->
![rustlab logo](./assets/rustlab-logo.png) <!-- .element: class="fragment" data-fragment-index="1" width="200px" style="margin-left: 50px" -->

Notes:
- First time speaking at a rust conference

---

### Content

1. Beginnings
2. Parsers
3. Winnow
4. Demo

---

### An Idea 💡

![alt text](./assets/garage.jpg)
<small>
Photo by <a href="https://unsplash.com/@johnpaulsen?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">John Paulsen</a> on <a href="https://unsplash.com/photos/white-vehicle-near-tree-MJeyFglfq9E?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a> and edited by me
</small>

Note:
- Everything began
- idea in a garage
- would take me into an endless rabbit hole
- makes no money
- I love it

---

### What idea?

Exchanging and tune recipes

Notes:
- I'm picky, if you like pineapple doesn't mean you like pineapple pizza

----

![image of santi cooking 1](./assets/santi-cooking-1.jpg) <!-- .element:  width="200px" style="margin: 0" -->
![image of santi cooking 2](./assets/santi-cooking-2.jpg) <!-- .element: class="fragment" data-fragment-index="0" width="200px" style="margin: 0" -->
![image of santi cooking 3](./assets/santi-cooking-3.jpeg) <!-- .element: class="fragment" data-fragment-index="1" width="200px" style="margin: 0" -->
![image of santi cooking 4](./assets/santi-cooking-4.jpeg) <!-- .element: class="fragment" data-fragment-index="2" width="200px" style="margin: 0" -->
![image of santi cooking 5](./assets/santi-cooking-5.jpeg) <!-- .element: class="fragment" data-fragment-index="3" width="200px" style="margin: 0" -->
![image of santi cooking 5](./assets/santi-cooking-6.jpeg) <!-- .element: class="fragment" data-fragment-index="4" width="200px" style="margin: 0" -->
![image of santi cooking 5](./assets/santi-cooking-7.jpeg) <!-- .element: class="fragment" data-fragment-index="5" width="200px" style="margin: 0" -->
![image of santi cooking 5](./assets/santi-cooking-8.jpeg) <!-- .element: class="fragment" data-fragment-index="6" width="200px" style="margin: 0" -->

----
<!-- .slide: data-auto-animate -->

#### Short history

![animation 1](./assets/a1.svg)

----

<!-- .slide: data-auto-animate -->

#### Short history

![animation 2](./assets/a2.svg)

----

<!-- .slide: data-auto-animate -->

#### Short history

![animation 3](./assets/a3.svg)

----

<!-- .slide: data-auto-animate -->

#### Short history

![animation 4](./assets/a4.svg)

----

<!-- .slide: data-auto-animate -->

#### Short history

![animation 5](./assets/a5.svg)

----

## recipe-lang

```recp
Take {potatoes}(3) and wrap them in &{aluminium foil}.
Place them directly into the coals of the grill.
Let cook for t{1 hour} until the potatoes are fork-tender.
```

![github logo](./assets/github.png) <!-- .element:  width="40px" style="margin: 0" -->
[reciperium/recipe-lang](https://github.com/reciperium/recipe-lang)

---

### Why Rust as a solo dev?

#### INSANE CONFIDENCE <!-- .element: class="fragment" data-fragment-index="0" -->

Note:
- performance by default
- cheap hosting
- scales well
- things just work
- fast iteration (next slide)

---

![alt text](./assets/rust-vs-js.png)

Note:
- easy to maintain
- easy to reproduce
- easy to refactor

---

## What was I needing?

- A way to write recipes that was easy for humans and machines <!-- .element: class="fragment" data-fragment-index="0" -->

---

## What's the problem with recipes?

Many sections

- ingredients
- materials
- instructions
- and more

---

## recipe-lang

```recp
Take {potatoes}(3) and wrap them in &{aluminium foil}.
Place them directly into the coals of the grill.
Let cook for t{1 hour} until the potatoes are fork-tender.
```

[playground](https://play.reciperium.com)

---

### Why building a parser in rust?

- for fun <!-- .element: class="fragment" data-fragment-index="0" -->
- for performance <!-- .element: class="fragment" data-fragment-index="1" -->
- for portability <!-- .element: class="fragment" data-fragment-index="2" -->

---

## What is a parser?

----

### First: What is a grammar?

> a finite set of rules to generate strings
<!-- .element: class="fragment" data-fragment-index="0" -->
> that are in the grammar
<!-- .element: class="fragment" data-fragment-index="1" -->

```sql
SELECT ( ALL | DISTINCT )?
    ( <star> | (
        <select sublist> ( <comma> <select sublist> )* )
    )
```
<!-- .element: class="fragment" data-fragment-index="2" -->

- SELECT is a terminal <!-- .element: class="fragment" data-fragment-index="2" -->
- \<select sublist\> is a non-terminal <!-- .element: class="fragment" data-fragment-index="2" -->

Notes:
- it's actually a context-free grammar
- it's context free because it doesn't have to consider the context
- popular: Backus Normal Form (BNF)
- terminal: like a literal value
- non-terminal: reference to another rule (play this rule)
- Precedence: order of operations
- Associativity: left to right, right to left, etc

----

### Then: What is a parser?

> A tool that maps a series of tokens to grammar rules to create a syntax tree, identifying errors if the token sequence is invalid
<!-- .element: class="fragment" data-fragment-index="0" -->

----

<!-- .slide: class="grid grid-cols-2 items-center content-center" -->

```json
{
  "name": "H J Simpson",
  "age": 40,
  "address": {
    "street": "Evrgrn Terace",
    "number": 742
  }
}
```

![syntax tree image](./assets/graph.png) <!-- .element: class="fragment" data-fragment-index="0" width="300px" -->

----
<!-- .slide: data-auto-animate -->
### Disclaimer

![programming language parts](./assets/pl-parts-1.svg)

----
<!-- .slide: data-auto-animate -->
### Disclaimer

![programming language parts 2](./assets/pl-parts-2.svg)

---

### Choosing a parser

| Grammars | Combinators | Regex |
| --- | --- | --- |
| New syntax | Familiar lang | New syntax |
| Maintanable | Maintanable | Depends |
| Reusable* |  | Reusable* |
| Rely on macros | | |

\* In theory <!-- .element: class="fragment" data-fragment-index="0" -->


Notes:
- This is almost never the case
- antlr works in many languages, but not rust
- all code is portable to other languages
- is there a correct one? No
- combinators: a mix of parser functions combined into a parser
- combinators are more intuitive to me, easier to test
- grammars: context-free grammar

---

### Combinator example

`winnow`

```rs
fn hex_primary(input: &mut &str) -> PResult<u8> {
    take_while(2, |c: char| c.is_ascii_hexdigit())
        .try_map(|input| u8::from_str_radix(input, 16))
        .parse_next(input)
}
```

<small>
<a href="https://docs.rs/winnow/latest/winnow/#example">docs.rs/winnow/latest/winnow/#example</a>

</small>

----

### Grammar example

`pest`

```
alpha = { 'a'..'z' | 'A'..'Z' }
digit = { '0'..'9' }

ident = { (alpha | digit)+ }

ident_list = _{ !digit ~ ident ~ (" " ~ ident)+ }
```

<small>
<a href="https://pest.rs/">pest.rs/</a>

</small>

----

### Regex

```re text-wrap
(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])
```

---

## What's in the rust market?

- grammars: [pest](https://github.com/pest-parser/pest), [peg](https://github.com/kevinmehall/rust-peg)
- combinators: [winnow](https://github.com/winnow-rs/winnow), [nom](https://github.com/rust-bakery/nom), [chumsky](https://github.com/zesterer/chumsky)
- regex: [regex](https://github.com/rust-lang/regex)

[and more](https://github.com/rosetta-rs/parse-rosetta-rs "target='_blank'")

---

## Winnow

- Excellent documentation with tutorials
- Very extendable
- Performant
- A fork of `nom` by [epage](https://github.com/epage)

----

## Setup

```sh
cargo add winnow
```

----

### Main usage

```rs [1-14|2|4,8|8,12]
use winnow::combinator::alt;
use winnow::{PResult, Parser};

pub fn parse_foobar_bool(i: &mut &str) -> PResult<bool> {
    alt((
        "foo".value(true),
        "bar".value(false)
    )).parse_next(i)
}
fn main() {
    let input = "foo";
    let out = parse_foobar_bool.parse(input).unwrap();
    println!("{}", out);
}
```

<small>
<a
    href="https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=6b5293f427f6b6933f013dd99abdd6d8" target="_blank">
    <i data-lucide="play"></i>
</a>
</small>


notes:
- PResult handles winnow errors with `ErrMode`
- Parser implements combinators for common types: `&str`, `&[u8]`, `char`, etc
- 4-9 is a combinator
- `parse_next` used inside comb
- `parse` for our main parser

---

## Demo

---

### JSONOO

(JSON Object Only)

_/Jay • SO • NOOOO/_ <!-- .element: class="fragment" data-fragment-index="0" -->

----

```json
{"event": "subscribed", "name": "bruce", "isadmin": false}
{"event": "subscribed", "name": "marta", "isadmin": true}
```

---

<!-- .slide: data-auto-animate -->

### alt

----

<!-- .slide: data-auto-animate -->

### alt

![alt func](./assets/alt1.svg)

----

<!-- .slide: data-auto-animate -->

### alt

![alt func 2](./assets/alt2.svg)
----

<!-- .slide: data-auto-animate -->

### alt

![alt func 3](./assets/alt3.svg)
----

<!-- .slide: data-auto-animate -->

### alt

![alt func backtrack 4](./assets/alt4.svg)

----

```rs
use winnow::ascii::{alpha1, digit1};
use winnow::combinator::alt;

fn parser(input: &str) -> IResult<&str, &str> {
  alt((alpha1, digit1)).parse_peek(input)
}
```

---

<!-- .slide: data-auto-animate -->

### delimited

----

<!-- .slide: data-auto-animate -->

### delimited

![delimited 1](./assets/del1.svg)

----

<!-- .slide: data-auto-animate -->

### delimited

![delimited 2](./assets/del2.svg)

----

<!-- .slide: data-auto-animate -->

### delimited

![delimited 3](./assets/del3.svg)

----

```rs
use winnow::combinator::delimited;

let mut parser = delimited("(", "abc", ")");

assert_eq!(parser.parse_peek("(abc)"), Ok(("", "abc")));
```

---


<!-- .slide: data-auto-animate -->

### separated

----

<!-- .slide: data-auto-animate -->

### separated

![separated](./assets/sep.svg)

----

<!-- .slide: data-auto-animate -->

### separated

![separated 2](./assets/sep2.svg)

----

<!-- .slide: data-auto-animate -->

### separated

![separated 3](./assets/sep3.svg)

----

```rs
// { "key1": "value1", "key2": "value2" }
delimited(
    ("{", space0),
    separated(
        0..,
        separated_pair(parse_string, ":", parse_token),
        (",", space0),
    ),
    (space0, "}"),
)
```
---

## Error handling

----

### ErrMode

Backtrack: recoverable (default) <!-- .element: class="fragment" data-fragment-index="0" -->

Cut: unrecoverable <!-- .element: class="fragment" data-fragment-index="1" -->

~`Incomplete`~ <!-- .element: class="fragment" data-fragment-index="2" -->

Notes:
- Incomplete only relevant for streaming

----

<!-- .slide: data-auto-animate -->

### delimited

![delimited 3](./assets/del3.svg)

----

<!-- .slide: data-auto-animate -->

### delimited

![delimited with cut error](./assets/del_err.svg)

----

```rs [1-14|7|8-10]
use winnow::combinator::cut_err;
use winnow::combinator::delimited;

let mut parser = delimited(
    "(",
    "abc",
    cut_err(")").context(
        StrContext::Expected(
            StrContextValue::CharLiteral('}')
        )
    )
);

assert_eq!(parser.parse_peek("(abc)"), Ok(("", "abc")));
```

----

### Context

```rs
StrContext::Label  // what is currently being parsed
StrContext::Expected  // expected grammar item
StrContextValue::CharLiteral
StrContextValue::StringLiteral
StrContextValue::Description
```

---


## Wrap up

- [winnow][winnow] is a well-documented powerful library
- check out [crafting interpreters] book
- let me know if you write a parser!

[winnow]: https://github.com/winnow-rs/winnow
[crafting interpreters]: https://craftinginterpreters.com

---

## Thanks

![qr code](./assets/qr-code.png)

![mastodon logo](./assets/mastodon.png) <!-- .element:  width="40px" style="margin: 0" --> [@woile@hachyderm.io](https://hachyderm.io/@woile)
&nbsp;&nbsp;&nbsp;
![github logo](./assets/github.png) <!-- .element:  width="50px" style="margin: 0" --> [woile](https://github.com/woile)

santiwilly@gmail.com
