# Regular Expressions: Matching Hexadecimal Colors

This tutorial is looking to explain a regex expression that checks for a valid IPv4 address in a string: 
```regex
(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3}
```
I found this regex expression, [regex for ip address(ipv4)](https://ihateregex.io/expr/ip/), at i<span style='color: red;'>HATE</span>Regex.

Regular expressions are ways to check to see if a string contains a specific pattern. The one that we will be looking at is a pattern to check to see if the string contains a valid IPv4 address. 

>An IPv4 address has the following format: x . x . x . x, where x is called an octet and must be a decimal value between 0 and 255. Octets are separated by periods. An IPv4 address must contain three periods and four octets.  - [IPv4 and IPv6 address formats](https://www.ibm.com/docs/en/ts3500-tape-library?topic=functionality-ipv4-ipv6-address-formats)

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

```regex
 (\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3} 
```
This expression has two parts, 

`(\b25[0-5]|\b2[0-4][0-9]|\b[01]?[0-9][0-9]?)` 


`(\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)){3} `



```javascript
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

The first part of this expression is only looking at the first octet, <span title="first octet" style='color: hotpink;'>x</span> . x . x . x , the larges the x value can be is 255, this is an thing IPv4 address, if it matches the one of the three patterns:

1. `25[0-5]`: The largest the decimal value can be is 255, this is checking that if the first two digits are 25 then the third can only be 5 or less, so the the value it can range between 250-255.

2. `2[0-4][0-9]`: This ranges is between 200-249. This checks if the first digit is a 2 then it will check to see if the second digit is a value between `[0-4]`, if the second digit passes it checks to see if there is a third digit that is`[0-9]`. It will check to see if any decimal value after the 2 match any decimal value between 00-49.

3. `[01]?[0-9][0-9]`: This pattern is different than that last two and it has a `?` we are not going to address right now, we want to look at the range. The first two patterns check if there was a match decimal values between 200 and 255, since each octet can be between 0 and 255 we now need to check for decimal values between 0 and 199. `[01]` it can be a 0 or a 1, ignore th `?` for now, this `[0-9]` is are range 0-9 for the second digit, and the same range `[0-9]` but for the first digit. 

Any decimal value between 0 and 255 will pass the check, 256 and above will fail.

This first part was just to look and see if this might be a valid IPv4 address by checking is there is a octet that matches the pattern, if it did not pass at this fist check, then there is no need to look to see if anything else passes. 

If it does pass, then we want to spend the time checking the rest of the Regex expression and that is what the second part of the expression does, it has the same repeating pattern as the first part `(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)` but it is surrounded by `(\. ){3}`, this is repeating the same pattern as above expect it does it three times `{3}` if it see a `.` in the correct spot all three and each time it checks the decimal value to see if it is between 0-255, then it will be a valid IPv4. 

Ok there was a lot that we skipped, like this `|` what is that, after dissecting this expression we are left with these  we will go over the things I did not cover like:

 `(), \b, [], |, ?, \, {}`

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
	()

	[]
	| 
	\.
	?
### Anchors
	Anchors are used to match a position before or after a character.
		\b

		^ $ 
### Quantifiers
	?
### Grouping Constructs

### Bracket Expressions

### Character Classes

### The OR Operator

### Flags

### Character Escapes

## Author
[iHateRegex](https://ihateregex.io/expr/ip/)
A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)


[geongeorge](https://github.com/geongeorge) owner of i<span style='color: red;'>HATE</span>Regex 
