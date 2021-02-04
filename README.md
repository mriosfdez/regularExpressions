# regularExpressions

## 1. Basic Set

### 1.1 *

* is a wilcard that represents ZERO or MORE occurences of the characters that precedes this asterisk. 

### 1.2 .

. is single wildcard that can represent any ONE characters in a single position. 

foo - a - bar
foo - x - bar
foo - c - bar 

regex: foo.bar

pattern in a regex engine: input file name and then grep 'foo.bar' filename

### 1.3 .*

.* represents ZERO or MORE occurences of wildcard, which means zero or more occurences of any character. 

foo - bar
foo- abc - bar
foo - bxc - bar
foo - z - bar

What's inside is unpredictable, either the letter or the number of letters. 

regex: foo.*bar 

grep 'foo.*bar' filename

### 1.4 \s and \s* 

\s represents whitespaces
\s* represents ZERO or MORE occurences of whitespace. 

foo bar
foo   bar
foo     bar
foobar

regex: foo\s*bar

### 1.5 [abc]

[abc] is a character class, one of the characters inside the square brackets.  

f oo
c oo
l oo 

regex: [fcl]oo

f oo
c oo
d oo
p oo
l oo
b oo

regex: [fcdplb]oo

### 1.6 [^abc]

[^abc] any character EXCEPT any of the ones inside the square brackets in a single positino. 

f oo
c oo
d oo
p oo
l oo
b oo
h oo XXX
m oo XXX

regex: [^hm]oo

### 1.7 [a-c]

[a-c]: one of the characters falling in the range given in square brackets.
It represents all the letters that start in the range included in the squares. 

joo
koo
loo
moo
boo XXX
woo XXX
zoo XXX
coo XXX

regex: [j-m]oo

### 1.8 [a-cx]

[a-cx] One of the characters falling in the range OR any of the other choices given in the square brackets. 
Union of the range plus the next character.

joo
koo
loo
moo
zoo
boo XXX
woo XXX
coo XXX

regex: [j-mz]oo 

### 1.9 [a-cA-Cx]

[a-cA-Cx]: One of the characters falling in one of the ranges OR any of other given insquare brackets.

joo
boo XXX
Koo
Loo
woo XXX
moo
zoo
coo XXX

regex: [j-mJ-Mz]oo

### 1.11 Backslask 

^$*.[()\

if your input string contains special symbols used in a literal way, used backlasy patterns 

xxx . yy
xx . yyyy
x . yy
xy  XXX
xxyy  XXX
yyxx  XXX
yx  XXX
yxxx  XXX

regex: x*\.y*

### 1.11 [#:.] AND x[#:\^]y

1.11.1 [#:.]

If a period (.) is inside square brackets there is no need to scape the characters. 

x#y
x:y
x.y
x&y XXX
x%y XXX

regex: x[#:.]y

1.11.2 x[#:\^]y

If any of the characters ^, - appear inside square brackets, it needs to be escaped with a backslash as these two characters have specials meaning insider square brackets. 

x#y
x:y
x^y
x&y XXX
x%y XXX

regex: x[#:\^]y

1.11.3 \\

The backlash itself should always be escaped with a backslash, irrespective of its position within the regex: \\

x#y
x\y
x^y
x&y   XXX
x%y   XXX

regex: x[#\\\^]y

### 1.12 ^

^ is a placeholder that signifies beginning of a line. 
The interpretation of ^ differs within square brackets and outsite of it:
- Inside square brackets, ^ stands for negation
- Outside square brackets, ^ represents for beginning of line

foo bar baz
bar foo baz   XXX
baz foo bar   XXX
bar baz foo   XXX
foo baz bar
baz bar foo   XXX

regex: ^foo.*

### 1.13 $

$ is a placeholder that signifies end of a line. 

foo bar baz   XXX
bar foo baz   XXX
baz foo bar
bar baz foo   XXX
foo baz bar
baz bar foo   XXX

regex: .*bar$

## 2. Extended Set 

