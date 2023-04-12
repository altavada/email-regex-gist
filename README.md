# TUTORIAL: Matching an Email with a Regular Expression

In this tutorial, you will be learning how to use a regular expression (regex) to create a search pattern for email addresses. This can be used in code and many search applications to find email addresses in a body of text or determine whether or not a string is an email address.

## Summary

This is the full regex used to identify email addresses: `/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`.

As a literal expression, a regex is always wrapped with `/` characters. Within this wrapping, the expression looks for a concurrent string of characters. It identifies email addresses by finding a username, followed by an `@` sign, followed by a domain name, followed by a `.`, followed by a domain extension. While it cannot validate whether these names are real and in-use, this regex does validate whether the string follows the same semantic rules as a valid email address.

## Table of Contents

- [Anchors](#anchors)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [Character Escapes](#character-escapes)
- [Quantifiers](#quantifiers)

## Anchors
Immediately within the `/` characters bookending our expression are anchors which indicate the beginning (`^`) and end (`$`) of a concurrent string, which will have to match the search pattern we specify within these anchors to return a match.

## Grouping Constructs
Depending on the complexity of our desired overall search pattern, we may need to divide our regex into distinct sections, or "groups," if one part of the search pattern requires different rules than another. Groups are wrapped with the `()` characters. Such is the case in this expression, as we can see three parenthesized groups that separately handle differing validation requirements:

- Group 1, `([a-z0-9_\.-]+)`, matching a username.
- Group 2, `([\da-z\.-]+)`, matching a domain name.
- Group 3, `([a-z\.]{2,6})`, matching a domain extension. 

Groups 1 and 2 are separated by an `@` character, which in this context is a *literal character*, meaning the regex will require an `@` sign between the username and the domain name. Likewise, Groups 2 and 3 are separated by `\.`, which will require there be a period between the domain name and extension. We will examine why the backslash is needed here in the [Character Escapes](#character-escapes) section.

## Bracket Expressions
Bracket expressions allow us to define a custom set of characters to search for. Let's take the bracket expression in Group 1 for example:

- `[a-z0-9_\.-]`

Within the bracket expression, we want to enumerate the allowed characters in the string or substring to which the expression applies. We can write this as a range, `x-z`, or by individually listing characters, `xyz`. So in our example, we're looking for a character that could be any lowercase Roman alphabet letter from a to z (`a-z`), any Arabic numeral from 0 to 9 (`0-9`), an underscore (`_`), a period (`\.`), or a hyphen (`-`).

## Character Classes
Bracket expressions are one of many *character classes*. Character classes define a set of characters to look for with our regex. While bracket expressions allow us to manually define a set, many classes are predefined with a convenient regex shorthand that can be easily nested within a bracket expression or kept on its own. In our email-matching regex, Group 2 contains one such character class, `\d`, which matches any Arabic numeral from 0 to 9, just as `0-9` did in the Group 1 bracket expression. Taking this with what we've already learned, we thus find that the bracket expression in Group 2 looks for a character which could be a number from 0-9, a lowercase letter from a to z, a period, or a hyphen.

## Character Escapes
The `\d` character class is also an example of *character escaping* in regex. This allows us to override the default regex usage of a character, with the key operator being the backslash. If we just wrote the character `d`, we'd be telling our search pattern to literally look for a lowercase `d`. Adding the `\` before it "escapes" the letter out of its literal definition, and redefines it as the character class for numerals. Conversely, the `.` character on its own is *not* a literal character in regex. Instead, it defines the "all characters" class. This is why, if we want to literally match a period, we need to escape out of the character's default definition, its character-class definition, and redefine it as literal. Therefore, we write `\.`.

## Quantifiers
Now that we have learned how to determine which characters to look for and how to divide our search pattern according to differing validation requirements, we need to tell our regex *how many* (or how few) matches we're looking for. Without any quantifiers, here's how our regex would look:

- `/^([a-z0-9_\.-])@([\da-z\.-])\.([a-z\.])$/`

Without quantifiers, the default for an expression is to look for a match *one* time and one time only. Our regex is fundamentally comprised of five expressions -- three bracket expressions and two literal expressions -- so if written as above, the regex will only match strings that are literally five characters long. That means `s@e.c` would return as a match, but a complete and properly written email address like `steve@email.com` would not. Our literal expressions are fine as-is -- we only *want* one of each of those characters at those positions. However, for our bracket expressions, we want to allow for *multiple* matching characters. As we can see, in the properly-written regex, the bracket expressions in Groups 1 and 2 are followed with a `+`. This quantifier specifies that the preceding expression should match *one or more* characters, with no upper limit. In Group 3, since we're looking for a domain extension, we're trying to match anywhere from 2 to 6 characters, so we write `{2,6}` after the bracket expression. Naturally, the syntax here is `{:minimum,:maximum}`.

## Author

My name is Sam Tomaka. I am a UNC Chapel Hill web development student. Check out my other work [here](https://github.com/altavada).
