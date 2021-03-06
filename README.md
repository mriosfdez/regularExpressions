# regularExpressions

## 1. Basic Set
## 2. Extende Set
## 3. Find and Replace with Capture Groups 

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

### 1.12 [#:.] AND x[#:\^]y

#### 1.12.1 [#:.]

If a period (.) is inside square brackets there is no need to scape the characters. 

x#y
x:y
x.y
x&y XXX
x%y XXX

regex: x[#:.]y

#### 1.12.2 x[#:\^]y

If any of the characters ^, - appear inside square brackets, it needs to be escaped with a backslash as these two characters have specials meaning insider square brackets. 

x#y
x:y
x^y
x&y XXX
x%y XXX

regex: x[#:\^]y

#### 1.12.3 \\

The backlash itself should always be escaped with a backslash, irrespective of its position within the regex: \\

x#y
x\y
x^y
x&y   XXX
x%y   XXX

regex: x[#\\\^]y

### 1.13 ^

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

### 1.14 $

$ is a placeholder that signifies end of a line. 

foo bar baz   XXX
bar foo baz   XXX
baz foo bar
bar baz foo   XXX
foo baz bar
baz bar foo   XXX

regex: .*bar$

### 1.15 ^ and $ 

^ is a placeholder that signifies beginning of a line
$ is a placeholder that signifies end of a line

foo 
foo bar   XXX
baz foo   XXX
foo bar baz   XXX
baz bar foo   XXX

regex: ^foo$


## 2. Extended Set 

### 2.1 Curly Braces Repetear {}

#### 2.1.1 a{m}

a{number} represents exaclty the number of repetitions of whatever immediately precedes this, in this example the 'a'

834   >>> ^[0-9][0-9][0-9]$
519
645

4874
5
89
45687
25

regex: ^[0-9]{3}&

l$ grep -E '^[0-9]{3}&' filename.txt

The "-E" option is required, if you are using any symbol from POSIX extended set in the regex. 

#### 2.1.2 a{m,n}

a{m,n} represents at least 'm' and at most 'n' repetitions of whatever immediately precedes this, in this case: 'a'

lion  >> ^[a-z]{4}$
tiger >> ^[a-z]{5}$
mouse >> ^[a-z]{5}$
cuckoo >> ^[a-z]{6}$
deer >> ^[a-z]{4}$

leopard
fox
kangaroo
bat

regex: ^[a-z]{4,6}$

### 2.2 Single Endend Curly Braces Repeater 

##### 2.2.1 (){m,}

() parenthesis is used to group and treat as a single entity. 
{m,} represents at least 'm' repetitions of whatever immediatily precedes this

hahahahaha  >> ha{5}
hahahaha >>   ha{4}
hahahahahaha >> ha{6}
hahahahahahahaha  >> ha{8}
hahahahahahahahaha  >> ha{9}

ha
hahaha
haha
-

regex: (ha){4,}

##### 2.2.2 (){,n}

() is used to group and treat as a single entity
{,n} represents at most 'n' repetitions of whatever immediately precedes this. 

ha
haha
hahahahaha  >> ^ha{5}$
hahahaha    >> ^ha{4}$
hahaha      >> ^ha{3}$
hahahahahahaha >>   ^ha{7}$
hahahahahaha >>     ^ha{6}$

regex: ^(ha){,2}$

### 2.3 The plus repeater

a+: one or more ocurrences of 'a', the character just preceding the plus symbol. 

fooaaaabar
fooabar
foobar  XXX
fooaabar
fooxxxbar XXX
fooxbar   XXX

regex: fooa+bar

### 2.4 The Question Mark Binary a?

a?: represents only two possibilities, ZERO or ONE occurrences of what precedes the question mark, here the 'a'

https://website
http://website
httpss://website  XXX
httpx://website   XXX
httpxx://website  XXX

regex: httpes?://website

### 2.5 Making Choices With Pipe (a|b)

(a|b) represents either 'a' or 'b', where 'a' and 'b' can be multicharacter strings. 

sapwood   XXX
rosewood  XXX
logwood 
teakwood  XXX
plywood
redwood   XXX

regex: (log|ply)wood


## 3. Find and Replace with Capture Groups 

#### 3.1. Example 1 

1280x720 > 1280 pix by 720 pix
1920x1080 > 1920 pix by 1080 pix
1600x900 > 1600 pix by 900 pix
1280x1024 > 1280 pix by 1024 pix
800x600 > 800 pix by 600 pix
1024x768 > 1024 pix by 768 pix

([0-9]+)x([0-9]+) > \1 pix by \2 pix

linux command: sed -r's/pattern/replacement/g'inputfile>
sed /r'/([0-9]+)x([0-9]+)/\1 pix by \2 pix/g'

#### 3.2 Firs name, last name Problem

John Wallace > Wallace,John
Steve King > King,Steve
Martin Cook > Cook, Martin
Adam Smith > Smith, Adam
Irene Peter > Peter, Irene
Alice Johnson >. Johnson, Alice 

([a-zA-Z]+)\s([a-zA-Z]+) > \2,\1

linux regex: sed -r 's/([a-zA-Z]+)\s([a-zA-Z]+/\2,\1/g' filename

#### 3.3 The Clock Time Problem

7:32 > 32 mins past 7
6:12 > 12 mins past 6
12:23 > 23 mins past 12
1:23 > 23 mins past 1
11:33 > 33 mins past 11
4:21 > 21 mins past 4

([0-9]{1,2}):([0-9]{2}) > \2 mins past \1

linux regex: regex: sed -r 's/([0-9]{1,2}):([0-9]{2})/\2 mins past \1/g' filename

#### 3.4 The Phone Number Problem 

914.582.3013 > xxx.xxx.3013
873.334.2589 > xxx.xxx.2589
521.589.3147 > xxx.xxx.3147
625.235.3698 > xxx.xxx.3698
895.568.2145 > xxx.xxx.2145
745.256.3369 > xxx.xxx.3369

[0-9]{3}.\[0-9]{3}\.([0-9]{4}) > xxx.xxx.\1

lixux regex: regex: sed -r 's/[0-9]{3}\.[0-9]{3}\.([0-9]{4})\xxx.xxx.\1

#### 3.5 The Date Problem

Jan 5th 1987 > 5-Jan-87
Dec 26th 2010 > 26-Dec-10
Mar 2nd 1923 > 2-Mar-23
Oct 1st 2008 > 1-Oct-08
Aug 3rd 2009 > 3-Aug-09
Jun 10th 2001 > 10-Jun-01

([a-zA-Z{3}])\s([0-9]{1,2})[a-z]{2}\s[0-9]{2}([0-9{2}]) > \2-\1-\3

Linux regex: regex: sed -r 's/([a-zA-Z{3}])\s([0-9]{1,2})[a-z]{2}\s[0-9]{2}([0-9{2}])/\2-\1-\3/g' filename

### 3.6 Another Phone Number Problem 

(914).582.3013 > 914.582.3013
(873).334.2589 > 873.334.2589
(521).589.3147 > 521.589.3147
(625).235.3698 > 625.235.3698
(895).568.2145 > 895.568.2145

\(([0-9]{3})\)(\.[0-9]{3}.\[0-9]{4}) > \1\2

linux regex: sed -r 's/\(([0-9]{3})\)(\.[0-9]{3}.\[0-9]{4})/\1\2/g' filename
