# Regex Tutorial - Matching a Hex Value

Regex, or regular expressions, are a set of special characters that constitute a search pattern. This tutorail will show how to use regex to match a Hex Value.

## Summary
/^#?([a-f0-9]{6}|[a-f0-9]{3})$/

The phrase above can be used to search strings for hexadecimal numbers. For a match to occur, the string provided must begin and end with the hexidecimal value. 

This expression matches either a 6-character or a 3-character hexidecimal string. The string may optionally begin with a # character.

#fff or 000aaaa are allowed, but fff9 or fffa are not.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors
The user can set the position constraints of a regex match using anchors. 

The caret  ^ indicates that the match must occur at the beginning of a string. A regex like /^aee/, for example, would match aee but not:123aee (or 0x123aee). 

Alternatively, the $ character can be used to match the end of a string. This might be done using /fff$/, which would match 000fff but not: fff000.

These anchors can be used in the same expressions as /000fff$/, which matches 000fff but not: 000ffff or 0000fff.

### Quantifiers
Regular expressions can make use of quantifiers to count the number of times a match is made. 

For example, the * character after an item specifies that it will be matched 0 or more times. A similarly positioned + character indicates that there will be one or more matches for the item.

When given the strings 00 00f or 00ffffff, /00f*/ will produce matches.
While /00f+ will match in all of the cases listed above save 00 because the f character does not appear there.

Furthermore,? can be used to denote that a match will be made with either 0 or 1 instances of the character before it. 00 and 00f but not 00ffffff would be matched by the regular expression /00f?/ in real life.

Curly brackets can be used to accomplish this if a specific number of matches are needed. The character x must match exactly n times when the expression xn is used. Alternatively, the expression xn, means that the character x needs to be matched n or more times.
Using xn,m, where x is matched between n (lower bound) and m (higher bound) times, a range of matches can be specified.

Additionally, you can specify that a match must happen either 0 or 1 time(s) by using the? character alone. Lazy matching can also be specified by combining the question mark operator with other quantifiers.

### OR Operator
To implement more complex logical expressions, combine regex with the OR operator. 

For instance, one might want to accept hexadecimal values 80, 90, a0, etc. if the binary representation of the hexidecimal was something like 1xxx0000 (where x indicates bits that can take any value), the following regex expression could be used to achieve this:.
/80|90|a0|b0|c0|d0|e0|f0/.
where the OR operator is denoted by the pipe | within the regex. 

Accordingly, a match can only take place if one of the strings contained in the expression is present.

### Character Classes
The regex user can specify character classes to limit the characters that can appear in a match. 

For instance, if we wanted to match ffx in hexadecimal, where x can be any hexadecimal character, we could define a character class using the bracket expression /ff[abcdef0123456789]/. This specifies that the match must be ff followed by any of the characters in the square brackets. The alternative is to use the notation /ff[a-f0-9]/, where the range of acceptable characters (abcdef and 0123456789) is specified by the notation a-f and 0-9.

Classes of shorthand characters can also be expressed using the letters d, s, or w. Any digit would match the characters d, s, or w, which would match any whitespace character (such as spaces, tabs, or newlines). 

The bracket expression [A-za-z0-9_] has the same meaning as the variable w. Character classes can be inverted D, S, or W, which simply means they match the opposite of their counterparts. Last but not least, all characters (aside from newlines) can be matched using the . character class.

### Flags
This tutorial will cover the three flags (i, g, and m). In regular expressions, flags are used to enable extra specified functionality. 

When you add an i flag to your regex, such as /ffff/i, it will look for the character ffff without regard to case, so both ffff and FFFF or ffFF will match.

The g (global) flag is another helpful flag because it searches the entire provided string for matches rather than stopping after the first match and returning instead.
Eg: If you add "90 + 10 = a0" to the expression "/[a-f0-9]2/g," it will match 90, 10, and a0; if you omit the global flag, only the first hex number will match.

The $ and anchors can be changed to refer to the start and finish of a line rather than the start and end of a string by using the multi line flag m.

### Grouping and Capturing
Using parenthesis or round brackets (and) to indicate a group is known as grouping. 

This enables one to think of a group of characters as a single entity. Every group in an expression is by default numbered from left to right and has a back reference. 

If this behavior is not desired, a non-capturing group with the syntax (?:text) can be used. 

An ordinary capture group might resemble:.
/a(ff)/, where ff is contiguous. As a result, it is possible to apply quantifiers to a group of characters (for example, /a(ff)2/ would match affff) or use alternation within groups (for example, /f(11|00)/ would match either f11 or f00).

### Bracket Expressions
A list of acceptable characters to match can be made using bracket expressions, which were covered in the character classes section above. Comparatively to shorthand character classes, bracket expressions support more complex behavior.

Ranges were used as a shorthand for writing out each of the acceptable characters in a set in the character class section above. One could define a bracket expression as [a-f] instead of [abcdef], where the hyphen - designates a range.

While shorthand character classes do support negation (D is negated d), bracket expressions can use negation in more sophisticated ways. For instance, using the caret symbol () at the start of a bracket expression to negate the entire expression would be an example. Like [g-z], where a match is made to characters outside of the range of g-z, this could be accomplished.

### Greedy and Lazy Match
Greedy matches will match as many characters as they can (/fa+/ will match faaa, matching as many characters as they can),

 whereas lazy matches (done by adding a? to the quantifier) will match as few characters as they can (/fa+?/ will match fa from a string faaa).

### Boundaries
Boundaries and anchors are similar, in that they outline the positional restrictions of the match. 

If a match must be contained in whitespace, such as a word, one can use matching to the front and back of the word. For instance, the regex /af800/ could be used to match af800 from "0xaf800 but not 0xaf800ff.

### Back-references
Backlinks are useful when there is an opportunity to reuse code. 

Groups of previously defined characters (named by numbers, indexed from left to right in regular expressions) \1 \2 etc. You can consult For example, if the regular expression /(123)(fff)/ produces two groups of characters, you can use /(123)(fff)\1\2/ to refer to these groups elsewhere in the expression. where \1 ( 123 ) and \2 (fff) represent bonds.

### Look-ahead and Look-behind
Look-ahead and look-behind are techniques for specifying match requirements based on surrounding characters. 

For example, if you wanted to match a hex value ffff from 0xffff but only when it was preceded by 0x, you could use a look-behind like /(?=0x)ffff/. To be more specific, the look-behind notation is (?=y)x, where item y must come before item x in order for a match to occur. 

The look-ahead notation is x(?=y), where item x must be followed by item y. This may be used to specify the end of a hex word 0xcafed00d, so that 0xcafe is only matched if it is followed by d00d: /0xcafed00d(?=d00d)/

## Author
Abenezer Mengesha is soon to graduate as a Fullstack Developer. 

Github: https://github.com/AbenezerTMengesha
