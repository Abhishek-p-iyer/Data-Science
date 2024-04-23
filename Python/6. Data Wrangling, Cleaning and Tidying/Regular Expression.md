___

## Introduction 

Regular Expressions have several use-cases, some of them include:
- Validating user input in html forms
- Aiding in searching for keywords in emails and webpages 
- examining test results
- Verifying and parsing text in files, code and applications 

## Literals
The simplest text we can match with regular expressions are **_literals_**. This is where our regular expression contains the exact text that we want to match. The regex `a`, for example, will match the text `a`, and the regex `bananas` will match the text `bananas`.

## Alteration 
If we were to match two words, say banana or apple, we could use the pipe symbol `|`. The `|` allows us to match either the characters preceding the `|` OR the characters after the `|`. The regex `baboons|gorillas` will match `baboons` in the text `I love baboons`, but will also match `gorillas` in the text `I love gorillas`.

## Character Set
 It’s easy to make mistakes on commonly misspelled words like `consensus`, and on top of that, there are sometimes alternate spellings for the same word.
**_Character sets_**, denoted by a pair of brackets `[]`, let us match one character from a series of characters, allowing for matches with incorrect or different spellings.
The regex `con[sc]en[sc]us` will match `consensus`, the correct spelling of the word, but also match the following three incorrect spellings: `concensus`, `consencus`, and `concencus`. The letters inside the first brackets, `s` and `c`, are the different possibilities for the character that comes after `con` and before `en`. Similarly for the second brackets, `s` and `c` are the different character possibilities to come after `en` and before `us`.

We can make our character sets even more powerful with the help of the caret `^` symbol. Placed at the front of a character set, the `^` negates the set, matching any character that is _not_ stated. These are called _negated character sets_. Thus the regex `[^cat]` will match any character that is not `c`, `a`, _or_ `t`

## Wildcards 
Sometimes we don’t care exactly WHAT characters are in a text, just that there are SOME characters. Enter the wildcard `.`! **_Wildcards_** will match any single character (letter, number, symbol or whitespace) in a piece of text. They are useful when we do not care about the specific value of a character, but only that a character exists!
Let’s say we want to match any 9-character piece of text. The regex `.........` will completely match `orangutan` and `marsupial`! Similarly, the regex `I ate . bananas` will completely match both `I ate 3 bananas` and `I ate 8 bananas`!
What happens if we want to match an actual period, `.`? We can use the escape character, `\`, to escape the wildcard functionality of the `.` and match an actual period. The regex `Howler monkeys are really lazy\.` will completely match the text `Howler monkeys are really lazy.`

## Ranges 
Character sets are great, but their true power isn’t realized without ranges. **_Ranges_** allow us to specify a range of characters in which we can make a match without having to type out each individual character. The regex `[abc]`, which would match any character `a`, `b`, _or_ `c`, is equivalent to regex range `[a-c]`. The `-` character allows us to specify that we are interested in matching a range of characters.
The regex `I adopted [2-9] [b-h]ats` will match the text `I adopted 4 bats` as well as `I adopted 8 cats` and even `I adopted 5 hats`.
With ranges we can match any single capital letter with the regex `[A-Z]`, lowercase letter with the regex `[a-z]`, any digit with the regex `[0-9]`. We can even have multiple ranges in the same character set! To match any single capital _or_ lowercase alphabetical character, we can use the regex `[A-Za-z]`.
Remember, within any character set `[]` we only match _one_ character.

## Shorthand character classes 
While character ranges are extremely useful, they can be cumbersome to write out every single time you want to match common ranges such as those that designate alphabetical characters or digits. To alleviate this pain, there are **_shorthand character classes_** that represent common ranges, and they make writing regular expressions much simpler. These shorthand classes include:

- `\w`: the “word character” class represents the regex range `[A-Za-z0-9_]`, and it matches a single uppercase character, lowercase character, digit or underscore
- `\d`: the “digit character” class represents the regex range `[0-9]`, and it matches a single digit character
- `\s`: the “whitespace character” class represents the regex range `[ \t\r\n\f\v]`, matching a single space, tab, carriage return, line break, form feed, or vertical tab

For example, the regex `\d\s\w\w\w\w\w\w\w` matches a digit character, followed by a whitespace character, followed by 7 word characters. Thus the regex completely matches the text `3 monkeys`.

In addition to the shorthand character classes `\w`, `\d`, and `\s`, we also have access to the _negated shorthand character classes_! These shorthands will match any character that is NOT in the regular shorthand classes. These negated shorthand classes include:

- `\W`: the “non-word character” class represents the regex range `[^A-Za-z0-9_]`, matching any character that is not included in the range represented by `\w`
- `\D`: the “non-digit character” class represents the regex range `[^0-9]`, matching any character that is not included in the range represented by `\d`
- `\S`: the “non-whitespace character” class represents the regex range `[^ \t\r\n\f\v]`, matching any character that is not included in the range represented by `\s`


## Grouping 
When we use the `|` we are able to match only the expression before and after the operator. What if we wanted to match "I love monkeys" and "I love dogs", the regex of `I love monkeys|dogs` would make the most sense, but THAT IS WRONG, as it would match, "I love monkeys" and "dogs". A way around this issue is to use grouping, `(` and `)`, this would help us group both the animals together. The regular expression would be, 
`I love (monkeys|dogs)`. 
The regex `I love (monkeys|dogs)` will match the text `I love` and _then_ match either `monkeys` or `dogs`, as the grouping limits the reach of the `|` to the text within the parentheses. These groups are also called _capture groups_, as they have the power to select, or capture, a substring from our matched text.

## Quantifiers - Fixed 
 So far we have only matched text on a character by character basis. But instead of writing the regex `\w\w\w\w\w\w\s\w\w\w\w\w\w`, which would match 6 word characters, followed by a whitespace character, and then followed by more 6 word characters, such as in the text `rhesus monkey`, is there a better way to denote the quantity of characters we want to match?

The answer is yes, with the help of quantifiers! **_Fixed quantifiers_**, denoted with curly braces `{}`, let us indicate the exact quantity of a character we wish to match, or allow us to provide a quantity range to match on.

- `\w{3}` will match _exactly_ 3 word characters
- `\w{4,7}` will match _at minimum_ 4 word characters and _at maximum_ 7 word characters

The regex `roa{3}r` will match the characters `ro` followed by `3` `a`s, and then the character `r`, such as in the text `roaaar`. The regex `roa{3,7}r` will match the characters `ro` followed by _at least_ `3` `a`s and _at most_ `7` `a`s, followed by an `r`, matching the strings `roaaar`, `roaaaaar` and `roaaaaaaar`.

> An important note is that quantifiers are considered to be _greedy_. This means that they will match the greatest quantity of characters they possibly can. For example, the regex `mo{2,4}` will match the text `moooo` in the string `moooo`, and not return a match of `moo`, or `mooo`. This is because the fixed quantifier wants to match the largest number of `o`s as possible, which is `4` in the string `moooo`.

## Quantifiers - Optional 
Sometimes words are spelled differently depending on where a particular person is from, for example: color and colour are both correct spellings of the word, humor and humour are both acceptable spellings of the same word. While using regex on these words, we use the `?` to indicate that a character is optional. It can either appear 0 or 1 time. 
The regex `humou?r` matches the characters `humo`, then either `0` occurrences or `1` occurrence of the letter `u`, and finally the letter `r`. Note the `?` _only_ applies to the character directly before it.
With all quantifiers, we can take advantage of grouping to make even more advanced regexes. The regex `The monkey ate a (rotten )?banana` will completely match both `The monkey ate a rotten banana` and `The monkey ate a banana`.

## Quantifiers - 0 or More, 1 or More
 The **_Kleene star_**, denoted with the asterisk `*`, is also a quantifier, and matches the preceding character `0` or more times. This means that the character doesn’t need to appear, can appear once, or can appear many many times.
The regex `meo*w` will match the characters `me`, followed by `0` or more `o`s, followed by a `w`. Thus the regex will match `mew`, `meow`, `meooow`, and `meoooooooooooow`.
Another useful quantifier is the **_Kleene plus_**, denoted by the plus `+`, which matches the preceding character `1` or more times.
The regex `meo+w` will match the characters `me`, followed by `1` or more `o`s, followed by a `w`. Thus the regex will match `meow`, `meooow`, and `meoooooooooooow`, but not match `mew`.
Like all the other metacharacters, in order to match the symbols `*` and `+`, you need to use the escape character in your regex. The regex `My cat is a \*` will completely match the text `My cat is a *`.

## Anchors 
When writing regular expressions, it’s useful to make the expression as specific as possible in order to ensure that we do not match unintended text. To aid in this mission of specificity, we can use the anchor metacharacters. The **_anchors_** hat `^` and dollar sign `$` are used to match text at the start and the end of a string, respectively.
The regex `^Monkeys: my mortal enemy$` will completely match the text `Monkeys: my mortal enemy` but not match `Spider Monkeys: my mortal enemy in the wild` or `Squirrel Monkeys: my mortal enemy in the wild`. The `^` ensures that the matched text begins with `Monkeys`, and the `$` ensures the matched text ends with `enemy`.
Without the anchor tags, the regex `Monkeys: my mortal enemy` will match the text `Monkeys: my mortal enemy` in both `Spider Monkeys: my mortal enemy in the wild` and `Squirrel Monkeys: my mortal enemy in the wild`.
Once again, as with all other metacharacters, in order to match the symbols `^` and `$`, you need to use the escape character in your regex. The regex `My spider monkey has \$10\^6 in the bank` will completely match the text `My spider monkey has $10^6 in the bank`.





