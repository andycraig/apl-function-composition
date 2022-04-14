# Why is function composition in APL cool?

A short article to highlight a cool feature of APL which doesn't exist in most other programming languages. It assumes absolutely no knowledge of APL.

## Key features of APL

There are three features of APL that stand out to me:

1. It's an array language
2. It's very terse: its built-in functions are all one character each
3. It allows for unusual forms of function composition

If you've worked with R or Python's numpy library then the arrays will feel very familiar (and indeed APL inspired both). I don't get much benefit from the one-character functions yet - I'm still new enough to APL that I often have to look them up. And if you're willing to define one-character aliases for your functions, you can write some [terse APL-like Python](https://medium.com/@jansson.sebastian/apl-in-the-eyes-of-a-python-programmer-7b1b955f7134) too.

Point 3 is to me the most interesting, because it's more unusual than point 1 and takes less experience to appreciate than point 2. In this article I'm going to try and explain point 3 in a way that makes sense even if you haven't seen any APL before.

## Example: Palindromes

Let's use the example of checking if a string is a palindrome to show some interesting APL function composition. In APL we can check if a string is a palindrome with `⌽≡⊢`. A result of 1 means 'true' and 0 means 'false'.

```apl
      (⌽≡⊢) 'aba'
1
      (⌽≡⊢) 'abc'
0
```

Each of `⌽`, `≡` and `⊢` is a function. Most of the characters APL uses for its built-in functions are unusual characters like those. To concentrate on the function composition instead of the function names let's call them `reverse`, `match` and `identity` instead. (The function `identity` just returns its argument unchanged.)

```apl
      reverse ← ⌽
      match ← ≡
      identity ← ⊢
      (reverse match identity) 'abc'
0
```

This is saying, 'Does the REVERSE of the argument MATCH the IDENTITY of the argument?'. The argument `'abc'` has been passed to `identity` (which it's next to) but also to `reverse` (three functions away). This is the feature of APL that I want to highlight as being cool. **When an argument `x` is passed to a sequence of three functions `(f g h)x`, it's evaluated like `g(f(x), h(x))`.**

## Why is this good?

When I saw this for the first time I thought it was pretty bizarre and couldn't see the motivation, especially since it's common to see `identity` used in these cases. What other language makes common use of an `identity` function? But it turns out to allow for some very concise programming. Let's demonstrate this by making the palindrome task a bit more complicated: now we want to check whether a string is a palindrome _ignoring whitespace_.

First we'll define a function `remove_spaces`. (It's a few APL characters but we'll assign it a single name to make things clearer.)

```apl
      remove_spaces ← ~∘' '
      remove_spaces 'a ba'
aba
```

Now we can do this:

```apl
      (reverse match identity) remove_spaces 'a ba'
1
```

If we re-wrote this into Python-esque pseudocode we'd get a lot more repetition:

```
match(reverse(remove_spaces('a ba')), identity(remove_spaces('a ba')))
```

We could remove some of that repetition by defining an intermediate variable `spaces_removed = remove_spaces('a ba')`. But in APL we don't have to: we can pass around _input arguments_ and _intermediate results_ in a concise implicit way.

This three-function composition is called a 'fork' in APL. Because the middle function is called last, we can write something like `(f+g)x` and it will be evaluated as `f(x)+g(x)`, which is what we might expect based on standard mathematical notation.

APL also supports several other function composition patterns using explicit 'operator' characters. As a side note, nothing special happens in APL when there are only two functions: `(f g)x` is evaluated as `f(g(x))`.

## In conclusion

At work I write a lot of Python and no APL. Does knowing about this feature of APL help me when I write Python? Not really. As I write Python code I sometimes think, 'If Python had function composition like APL does, this code could be shorter', and that's about it. But learning about it made me aware of a feature a programming language _could_ have that I hadn't thought of before.


## How can I get started with APL?

If reading this has made you interested in APL and you'd like to give it a try, I recommend https://tryapl.org/. It runs APL in the browser, and you can click on the symbols to enter them - no need for a special keyboard or anything.
