---
title: Becoming a Regex Ninja
date: 2018-07-02 18:40:34
tags: #regex #validation
---
In this post I cover eveything I learned to become a Regex Ninja :)
By the end of reading you should become one too!

# REGULAR EXPRESSIONS
- Allows us to check a series of characters for 'matches'
- Allows us to check a form field to try and match a valid email address (form validation)
- Used for string transformations (eg. removing http:// from links)

Test your Regex online on [regex101](https://regex101.com/)

## BASIC STRUCTURE
- Syntax: <code>/.../</code>
- RegEx are <u>case sensitive</u>: <code>/ninja/ !== /Ninja/</code>
- Flags: <code>/.../gi</code> 'g' stands for global, 'i' for case insensitive

## CHARACTER SETS
1. <code>/[ng]inja/</code>
- [ng] means n OR g
- 'ninja' OR 'ginja' => **matches**
2. <code>/[abc]/</code>
- Find any 'a' OR 'b' OR 'c'
3. <code>/[^pe]/</code>
- **^** means EXCLUDE so this regex matches everything except 'p' OR 'e'

## RANGES
<code>/[abcdefghijklmnopqrstuvwxyz]/ === /[a-z]/</code>
<code>/[a-zA-z]/</code> - all letters case insensitive
<code>/[0-9]/</code> - all digits
<code>/[5-9]inja/</code> - '6inja' => **matches**

## REPEATING CHARACTERS
<code>/[0-9][0-9][0-9]/ === /[0-9]{3}/</code>
- {} is called a quantifier
- '3' - how many times match?
- '017' => **matches**

<code>/[a-z]{5,8}/</code>
- words between 5-8 characters long
- 'hello' => **matches**

<code>/[0-9]{5,}/</code>
- numbers AT LEAST 5 digits long (second parameter in quantifier deliberately omited)
- '02353251' => **matches**

<code>/[0-9]+/</code>
- '**+**' - one or more quantifier
- '5215623' => **matches**

## METACHARACTERS
-<code>\d</code> - match any digit character (same as [0-9])
-<code>\w</code> - match any word character (a-z, A-Z, 0-9, and _'s)
-<code>\s</code> - match any whitespace character (spaces, tabs etc.)
-<code>\t</code> - match a tab character only

Examples:

<code>/\d\s\w/</code>
- '1   n' => **matches**
- '5    p' => **matches**

<code>/\d{3}\s\w{5}/</code>
- '123 ninja' => **matches**
- '997 hello' => **matches**

## SPECIAL CHARACTERS
- '**+**' - the one or more quantifier
- '**\**' - the escape character
- '**[]**' - the character set
- '**[^]**' - the negate symbol in character set
- '**?**' - the zero-or-one quantifier (makes a preceding character optional)
- '**.**' - any character whatsoever (except the newline character)
- '*****' - the zero-or-more quantifier (a bit like '**+**')

Examples

<code>/hello?/</code>
- 'o' is an optional character
- 'hello' => **matches**
- 'hell' => **matches**

<code>/car./</code>
- 'cars' => **matches**
- 'card' => **matches**

<code>/abc\*/</code>
- 'abc*' => **matches**

<code>/a[a-z]?/</code>
- 'ag' => **matches**
- 'a' => **matches**

<code>/.+/</code>
- Trick for matching any string
- 'mystring_[]o22' => **matches**

## STARTING AND ENDING PATTERNS
'**^**' - start of a string
'**$**' - end of a string

<code>/^[a-z]{5}$/</code>
- Matches exact 5 characters long word
- 'hello' => **matches**
- 'ninja' => **matches**

## ALTERNATE CHARACTERS
'**|**' - logical OR

<code>/(p|t)yre/</code>
- 'pyre' => **matches**
- 'tyre' => **matches**

<code>/(pet|toy|crazy)?rabbit/</code>
- '**?**' - stands for optional
- 'pet rabbit' => **matches**
- 'toy rabbit' => **matches**
- 'rabbit' => **matches**

## USAGE IN JAVASCRIPT
Two ways to declare:
1. <code>const reg = /[a-z]/i;</code>
2. <code>const reg2 = new RegExp(/[a-z]/, 'i');</code>

Testing:

<code>reg.test('mystring'); // returns TRUE because it matches</code>

## EXERCISES
You can use [regex101](https://regex101.com/) tool for solving.
Write regex <code>/.../</code> for matching:

1. Telephone that must be a valid UK telephone number (11 digits)
2. Username that must be alphanumeric and contain 5-12 characters
3. Password that must be alphanumeric (@._ and - are also allowed) and be 8-20 characters
4. (tricky) Email that must be a valid email address e.g. boss@mydomain.co.uk

Good luck and I encourage you to share your solutions in the comments.

This is how you successfully became a **Regex Ninja**!

## CREDITS
[The Net Ninja](https://www.youtube.com/channel/UCW5YeuERMmlnqo4oq8vwUpg)
