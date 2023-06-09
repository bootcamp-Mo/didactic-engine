# Regular Expressions: IPv4 address

This tutorial is looking to explain a regex expression that checks for a valid 
IPv4 address in a string: 
```regex
(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}
```
I found this regex expression, [regex for ip address(ipv4)](https://ihateregex.io/expr/ip/), 
at i<span style='color: red;'>HATE</span>Regex.

Regular expressions are ways to check to see if a string contains a specific 
pattern. The one that we will be looking at is a pattern to check to see if 
the string contains a valid IPv4 address. 

>An IPv4 address has the following format: x . x . x . x, where x is called 
an octet and must be a decimal value between 0 and 255. Octets are separated 
by periods. An IPv4 address must contain three periods and four octets. - [IPv4 and IPv6 address formats](https://www.ibm.com/docs/en/ts3500-tape-library?topic=functionality-ipv4-ipv6-address-formats)

This means an IPv4 has can look like any of the following:
- 0.0.0.0
- 123.123.123.123
- 255.255.255.255

But not these:
- 723.823.239.234
- 255.255..0.33
- 232.12.1233

What were are working with:
1. There are 4 octets
2. Each octet is a decimal value between 0 and 255
3. The octets are separated by periods
4. The IPv4 address must contain three periods and four octets

## Summary
- A regular expression is a pattern that can be used to check to see if a 
string contains
a specific pattern.
- A regular expression is a sequence of characters that define a search pattern.
- A regular expression is used with the `match()` method to find a match in a string.
- A regular expression is used with the `test()` method to test if a string 
contains a match
or not.
- A regular expression is used with the `replace()` method to replace a match 
in a string.
- A regular expression is used with the `split()` method to split a string 
into an array of
substrings.

This expression has two parts, 

`(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)` 


`(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3} `


The first part of this expression is only looking at the first octet, 
<span title="first octet" style='color: #bf44d0 ;'>x</span> . x . x . x , 
the largest the octet <span title="first octet" style='color:  #bf44d0;'>x</span> 
value can be is 255, 
<span title="first octet" style='color: #bf44d0 ;' >xxx</span> . xxx . xxx . xxx ,
this is another IPv4 address pattern showing an address can have up to three 
digit for each octet. 

This expressions uses three alternatives patterns catch any matches:

1. `25[0-5]`: The largest the decimal value can be is 255, this is checking 
that if the first two digits are 25 then the third can only be 5 or less, 
so the the value it can range between 250-255.

2. `2[0-4][0-9]`: This ranges is between 200-249. This checks if the first 
digit is a 2 then it will check to see if the second digit is a value between 
`[0-4]`, if the second digit passes it checks to see if there is a third digit 
that is`[0-9]`. It will check to see if any decimal value after the 2 match 
any decimal value between 00-49.

3. `[01]?[0-9][0-9]`: This pattern is different than that last two and it has 
a `?` we are not going to address right now, we want to look at the range. 
The first two patterns check if there was a match decimal values between 200 
and 255, since each octet can be between 0 and 255 we now need to check for 
decimal values between 0 and 199. `[01]` it can be a 0 or a 1, 
ignore the `?` for now, this `[0-9]` is are range 0-9 for the second digit, 
and the same range `[0-9]` but for the first digit. 

Any decimal value between 0 and 255 will pass the check, 256 and above will fail. 

This first part was just to look and see if this might be a valid IPv4 address 
by checking is there is a octet that matches the pattern, if it did not pass at 
this fist check, then there is no need to look to see if anything else passes. 

If it does pass, then we want to spend the time checking the rest of the Regex 
expression and that is what the second part of the expression does, it has the 
same repeating pattern as the first part `(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)` 
but it is surrounded by `(\.(...)){3}` this ensures that the pattern is repeated 
three times and is separated by dots. If that string doesn't match the pattern 
then it doesn't pass.

Ok there was a lot that we skipped, like this `|`  what is that, after dissecting 
this expression we are left with these meta characters:

 `()`, `[]`, `|`, `?`, `\b`, `\.`, `{}`


## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components
	(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}
### Anchors
Anchors in regex are used to represent a position in a string. The two most 
common anchors are the `^`  and the `$`,  theses are not it this expression but 
its still important to cover them.

The `^` represents the start of a line/new string, when this is used in an 
expression it indicates to the pattern that the pattern should only match if 
the pattern appearers at the start of the line/new string. 

		```
		If the pattern was ^look a passing string would be 'look how fast they were', a non-passing string would be 'I asked Marty to look for the spoon.' because of the `^` the pattern has to be at the start of a line/ new string. 
		```

The `$` on the other hand matches everything up until the end of the current 
line or new string.


		```
		If the pattern was duck$ then a passing string would be 'Wow, look at that duck' because the pattern we are looking for is for the duck to be at the end, 'If Nilly got a duck, I want one too' this would not pass because that pattern says that duck had to be at the end of the line/ new string. 
		```

The anchor that is in this regex expression is `\b`, this is a boundary word,  
is its not looking at the start of a string like a `^` or the end of the string 
like the `$`,  for the pattern but its looking for a position where a word 
character is not followed by another word character. A word character is 
alphanumeric or an underscore, `\b` is the boundary between word characters 
and non word characters.

Word characters:

    Alphabetic letters: a, B, z
    Digits: 0, 1, 9
    Underscore: _

Non-word characters:

    Space:   (space character)
    Punctuation marks: . , ! ?
    Special characters: @, #, %
    Mathematical symbols: + - * /
    Brackets and parentheses: () [] {}
    Quotes: " '

In our expression `\b` is uses three times to check to make sure that we are 
only matching with standalone words.

For example:

	'MyIPv4Addressis255.255.255.255' 
  That would not pass the match because this is a lump of character words and 
  the pattern says that for the pattern to pass there can not be character words 
  in front of the `25`, and in this case there is the word character `s` is 
  before the `25` so it does not pass.

	'My IPv4 address is 255.255.255.255'
  This will pass because when the pattern `\b25[0-5]` checks it looks to see if 
  there is 25 and that the character of it in a non character word, in this case 
  it is a space. 

### Grouping Constructs
Grouping construct `()` allow you to group parts of the expression together and 
apply quantifiers or other operators to the group as a whole. The parentheses 
group alternatives together, allowing logical OR operations,

In this expression 
we use grouping constructs twice that is how we are able to have a two part 
pattern, the first part `(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)` which 
checks the string for a pattern that starts with a 2 or 1 or 0 and has a 
non-word character proceeding it. 

The second part of the expression is what checks the string for the dot in 
the IPv4 address pattern. 
x <span title="first octet" style='color: #bf44d0 ;'>.</span> x . x . x 
The second part has two grouping construct `(\. ...){3}` and inside that group 
is the last use of `()` is `(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)` this is a 
group of decimal value patterns that check each digit for its value and position 
in the pattern, the whole second part of the expression checks for a pattern 
match the same way for all three remaining octet, it first checks for a `.` and 
if so it checks the string input for a decimal range of 0-255.  

### Quantifiers
Quantifiers define how many times the preceding character or group should match. 
Above we mention that the second part of the regex expression is repeated three 
times, this `(\. ...){3}` is how the expression knows to repeat. The `()` as we 
now know are grouping construct are first matching with the `.` and the it 
checks for the decimal range of 0-225. The `{3}` is multiplying the pattern by 
three because after the first part there are only three reaming octet each one 
is preceded by a dot.

`{n}` is a quantifier that specifies the exact number of repetitions for the 
preceding element or group. This expression had 3 repetitions

### Bracket Expressions
Bracket expressions are a way to define a set of characters that can match a 
character at a specific position in the input string. Brackets can hold ranges 
to specify a contiguous set of characters, or list individual characters. 

Looking at the IPv4 address that has three digit value for each octet,

<span title="first octet" style='color:  #bf44d0 ;'>xxx</span>. xxx . xxx . xxx , 
we can see that there can be three positions in a octet, 123.123.123.123, 
bracket expressions are used to make sure that the pattern is look at each 
position of the string and comparing it the digit value that is defined by the
bracket contents.

Each octet can have a decimal value between 0-255, a valid IPv4 address octet can 
have up to three positions for each digit, _ _ _ . xxx . xxx . xxx . xxx

In our expression the brackets are used to determine three range groups. 
IPv4 addresses can not be a number greater than 255, the first range check 
is `25[0-5]` this is saying that the pattern it is checking for is 2, 5, and 
something that can be 0 or 5 or the numbers that range between it, 255-250. The 
bracket is in the third position this means the digit can only be in the position 
after the `25_`, not `2_5`, or `_25`, because it has to matches at a specific 
position. 

The second range group is `2[0-4][0-9]`, this says that fist there is a 2 and in 
the next position is something between `0-4` and in the third position there is 
the range of `0-9`, this is checking to see if the pattern will check for a 
range of 249-200. 

The third use of brackets is `[01]?[0-9][0-9]?` this one is a little complicated 
because of the `?`, [what is that](#the-or-operator), but the brackets are the 
same so we are just cover the brackets for this part. In the first position, `[01]`, 
the brackets is looking for a 0 or a 1 for the first position, if that passes then 
the next check is at the second position, and if there is a digits who's value 
is in the `0-9` range and it check the last position for a digit value of `[0-9]`, 
is the same as the second position last, this will find the decimal value 
between 199-0.

### The OR Operator
The OR operator `|` is used in regular expressions to match either the expression 
preceding it or the expression following it. It allows you to define alternative 
patterns within a regular expression. In our expression 
`(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)`, the OR operator is used to 
specify alternative patterns for matching the octets of an IPv4 address.

In the first part of the expression `(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)` 
has three alternative patterns separated by the OR operator `|`. It will match 
either `25[0-5]` with the values between 250 and 255, `2[0-4][0-9]` values between 200 and 249, 
or `[01]?[0-9][0-9]?` values between 0 and 199 for the first octet of the IPv4 address.

The OR operator allows for flexibility in matching different possibilities 
within a regular expression.

### Character Escapes
Character escapes in regular expressions are used to match specific characters 
that have special meaning within the regex syntax. These characters are preceded 
by a backslash `\` to indicate that they should be treated as literal characters.

In the seconde part of our expression we use the backslash `\` character to 
match the `.` of an IPv4 address pattern
### Code Example 

```Javascript
const inputString = "The IP address is 192.168.0.1, and the port is 8080";
const regex = /(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}/;

// Validating and extracting the IP address
const ipAddressMatch = inputString.match(regex);
if (ipAddressMatch) {
  const ipAddress = ipAddressMatch[0];
  console.log("Valid IP address:", ipAddress);
} else {
  console.log("No valid IP address found.");
}
```

## Author
Hello I'm Mo, I'm a person who is learning to code, you can check out what 
projects I have been working on and how my sentence smashing is coming along by 
going to my gitHub at [Bootcamp-Mo](https://github.com/bootcamp-Mo). 

I found this expression on the website [iHateRegex](https://ihateregex.io/expr/ip/) 
it was created by [geongeorge](https://github.com/geongeorge) who is the owner 
of i<span style='color: red;'>HATE</span>Regex 
