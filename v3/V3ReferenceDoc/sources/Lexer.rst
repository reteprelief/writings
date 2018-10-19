Lexical Elements
================

 The text of an AADL description consists of a sequence of lexical
elements, each composed of characters. The rules of composition are
given in this section.

1. .. rubric:: Character Set
  :name: character-set

 The only characters allowed outside of comments are the
graphic\_characters and format\_effectors.

Syntax

character ::= graphic\_character \| format\_effector

\| other\_control\_character

graphic\_character ::= identifier\_letter \| digit \|
space\_character

\| special\_character

Semantics

 The character repertoire for the text of an AADL specification
consists of the collection of characters called the Basic
Multilingual Plane (BMP) of the ISO 10646 Universal Multiple-Octet
Coded Character Set, plus a set of format\_effectors and, in
comments only, a set of other\_control\_functions; the coded
representation for these characters is implementation defined (it
need not be a representation defined within ISO-10646-1).

The description of the language definition in this standard uses the
graphic symbols defined for Row 00: Basic Latin and Row 00: Latin-1
Supplement of the ISO 10646 BMP; these correspond to the graphic
symbols of ISO 8859-1 (Latin-1); no graphic symbols are used in this
standard for characters outside of Row 00 of the BMP. The actual set
of graphic symbols used by an implementation for the visual
representation of the text of an AADL specification is not
specified.

 The categories of characters are defined as follows:

*identifier\_letter *

upper\_case\_identifier\_letter \| lower\_case\_identifier\_letter

*upper\_case\_identifier\_letter *

Any character of Row 00 of ISO 10646 BMP whose name begins Latin
Capital Letter.

*lower\_case\_identifier\_letter *

Any character of Row 00 of ISO 10646 BMP whose name begins Latin
Small Letter.

*digit *

One of the characters 0, 1, 2, 3, 4, 5, 6, 7, 8, or 9.

*space\_character *

The character of ISO 10646 BMP named Space''.

*special\_character *

Any character of the ISO 10646 BMP that is not reserved for a
control function, and is not the space\_character, an
identifier\_letter, or a digit.

*format\_effector *

The control functions of ISO 6429 called character tabulation (HT),
line tabulation (VT), carriage return (CR), line feed (LF), and form
feed (FF).

*other\_control\_character *

Any control character, other than a format\_effector, that is
allowed in a comment; the set of other\_control\_functions allowed
in comments is implementation defined.

 The following names are used when referring to certain
special\_characters:

+--------------+---------------------+--------------+------------------------+
| **Symbol**   | **Name**| **Symbol**   | **Name**   |
+--------------+---------------------+--------------+------------------------+
| "| quotation mark  | :| colon  |
+--------------+---------------------+--------------+------------------------+
| #| number sign | ;| semicolon  |
+--------------+---------------------+--------------+------------------------+
| =| equals sign | (| left parenthesis   |
+--------------+---------------------+--------------+------------------------+
| )| Right parenthesis   | \_   | underline  |
+--------------+---------------------+--------------+------------------------+
| +| plus sign   | [| left square bracket|
+--------------+---------------------+--------------+------------------------+
| ,| Comma   | ]| right square bracket   |
+--------------+---------------------+--------------+------------------------+
| -| Minus   | {| left curly bracket |
+--------------+---------------------+--------------+------------------------+
| .| Dot | }| right curly bracket|
+--------------+---------------------+--------------+------------------------+

Implementation Permissions

 In a nonstandard mode, the implementation may support a different
character repertoire; in particular, the set of symbols that are
considered identifier\_letters can be extended or changed to conform
to local conventions.

NOTE: Every code position of ISO 10646 BMP that is not reserved for a
control function is defined to be a graphic\_character by this standard.
This includes all code positions other than 0000 - 001F, 007F - 009F,
and FFFE - FFFF.

Lexical Elements, Separators, and Delimiters
--------------------------------------------

Semantics

 The text of an AADL specification consists of a sequence of separate
*lexical elements*. Each lexical element is formed from a sequence
of characters, and is either a delimiter, an identifier, a reserved
word, a numeric\_literal, a character\_literal, a string\_literal,
or a comment. The meaning of an AADL specification depends only on
the particular sequences of lexical elements that form its
compilations, excluding comments.

 The text of an AADL specification is divided into lines. In general,
the representation for an end of line is implementation defined.
However, a sequence of one or more format\_effectors other than
character tabulation (HT) signifies at least one end of line.

In some cases an explicit *separator* is required to separate
adjacent lexical elements. A separator is any of a space character,
a format\_effector, or the end of a line, as follows:

-  A space character is a separator except within a comment, a
   string\_literal, or a character\_literal.

-  Character tabulation (HT) is a separator except within a comment.

-  The end of a line is always a separator.

 One or more separators are allowed between any two adjacent lexical
elements, before the first, or after the last. At least one
separator is required between an identifier, a reserved word, or a
numeric\_literal and an adjacent identifier, reserved word, or
numeric\_literal.

A *delimiter* is either one of the following special characters

**( ) [ ] { } , . : ; = \* + - **

 or one of the following *compound delimiters* each composed of two
or three adjacent special characters

**:: => +=> -> <-> .. −[ ]−> {\*\* \*\*}**

 Each of the special characters listed for single character
delimiters is a single delimiter except if that character is used as
a character of a compound delimiter, or as a character of a comment,
string\_literal, character\_literal, or numeric\_literal.

The following names are used when referring to compound delimiters:

+-----------------+----------------------------+
| **Delimiter**   | **Name**   |
+-----------------+----------------------------+
| **::**  | qualified name separator   |
+-----------------+----------------------------+
| =>  | association|
+-----------------+----------------------------+
| +=> | additive association   |
+-----------------+----------------------------+
| ->  | directional connection |
+-----------------+----------------------------+
| <-> | bidirectional connection   |
+-----------------+----------------------------+
| ..  | interval   |
+-----------------+----------------------------+
| -[  | left step bracket  |
+-----------------+----------------------------+
| ]-> | right step bracket |
+-----------------+----------------------------+
| {\*\*   | begin annex|
+-----------------+----------------------------+
| \*\*}   | end annex  |
+-----------------+----------------------------+

Processing Requirements and Permissions

 An implementation shall support lines of at least 200 characters in
length, not counting any characters used to signify the end of a
line. An implementation shall support lexical elements of at least
200 characters in length. The maximum supported line length and
lexical element length are implementation defined.

1. .. rubric:: Identifiers
  :name: identifiers

 Identifiers are used as names. Identifiers are case insensitive.

Syntax

identifier ::= identifier\_letter {[underline] letter\_or\_digit}\*

letter\_or\_digit ::= identifier\_letter \| digit

-  An identifier shall not be a reserved word.

-  For the lexical rules of identifiers, the rule of whitespace as token
   separator does not apply. In other words, identifiers do not contain
   spaces or other whitespace characters.

Legality Rules

1. An identifier must be distinct from the reserved words of the AADL.

Semantics

 All characters of an identifier are significant, including any
underline character. Identifiers differing only in the use of
corresponding upper and lower case letters are considered the same.

Processing Requirements and Permissions

 In a nonstandard mode, an implementation may support other
upper/lower case equivalence rules for identifiers, to accommodate
local conventions.

In non-standard mode, a method of implementation may accept
identifier syntax of any programming language that can be used for
software component source text.

Examples

Count X Get\_Symbol Ethelyn Garçon

Snobol\_4 X1 Page\_Count Store\_Next\_Item Verrűckt

Numerical Literals
------------------

 There are two kinds of numeric literals, real and integer. A
real\_literal is a numeric\_literal that includes a point; an
integer\_literal is a numeric\_literal without a point.

Syntax

numeric\_literal ::= integer\_literal \| real\_literal

integer\_literal ::= decimal\_integer\_literal \|
based\_integer\_literal

real\_literal ::= decimal\_real\_literal

Decimal Literals
~~~~~~~~~~~~~~~~

 A decimal literal is a numeric\_literal in the conventional decimal
notation (that is, the base is ten).

Syntax

decimal\_integer\_literal ::= numeral [ positive\_exponent ]

decimal\_real\_literal ::= numeral **.** numeral [ exponent ]

numeral ::= digit {[underline] digit}\ :sup:`\*`

exponent ::= E [+] numeral \| E – numeral

positive\_exponent ::= E [+] numeral

Semantics

 An underline character in a numeral does not affect its meaning. The
letter E of an exponent can be written either in lower case or in
upper case, with the same meaning.

An exponent indicates the power of ten by which the value of the
decimal literal without the exponent is to be multiplied to obtain
the value of the decimal literal with the exponent.

Examples

12 0 1E6 123\_456 *-- integer literals*

12.0 0.0 0.456 3.14159\_26 *-- real literals*

Based Literals
~~~~~~~~~~~~~~

 A based literal is a numeric\_literal expressed in a form that
specifies the base explicitly.

Syntax

based\_integer\_literal ::= base **#** based\_numeral **#** [
positive\_exponent ]

base ::= digit [ digit ]

based\_numeral ::= extended\_digit {[underline] extended\_digit}

extended\_digit ::= digit \| A \| B \| C \| D \| E \| F \| a \| b \|
c \| d \| e \| f

Legality Rules

1. The base (the numeric value of the decimal numeral preceding the
   first #) shall be at least two and at most sixteen. The
   extended\_digits A through F represent the digits ten through
   fifteen respectively. The value of each extended\_digit of a
   based\_literal shall be less than the base.

Semantics

 The conventional meaning of based notation is assumed. An exponent
indicates the power of the base by which the value of the based
literal without the exponent is to be multiplied to obtain the value
of the based literal with the exponent. The base and the exponent,
if any, are in decimal notation.

The extended\_digits A through F can be written either in lower case
or in upper case, with the same meaning.

Examples

2#1111\_1111# 16#FF# 016#0ff# *-- integer literals of value 255*

2#1110\_0000# 16#E#E1 8#340# *-- integer literals of value 224*

String Literals
---------------

 A string\_literal is formed by a sequence of graphic characters
(possibly none) enclosed between two quotation marks used as string
brackets.

Syntax

string\_literal ::= "{string\_element}\*"

string\_element ::= "" \| non\_quotation\_mark\_graphic\_character

 A string\_element is either a pair of quotation marks (""), or a
single graphic\_character other than a quotation mark.

Semantics

 The sequence of characters of a string\_literal is formed from the
sequence of string\_elements between the bracketing quotation marks,
in the given order, with a string\_element that is "" becoming a
single quotation mark in the sequence of characters, and any other
string\_element being reproduced in the sequence.

A null string literal is a string\_literal with no string\_elements
between the quotation marks.

NOTE: An end of line cannot appear in a string\_literal.

Examples

"Message of the day:"

"" *-- a null string literal*

" " "A" """" *-- three string literals of length 1*

"Characters such as $, %, and } are allowed in string literals"

Comments
--------

 A comment starts with two adjacent hyphens and extends up to the end
of the line.

Syntax

comment ::= *--{non\_end\_of\_line\_character}\**

-  A comment may appear on any line of a program.

Semantics

 The presence or absence of comments has no influence on whether a
program is legal or illegal. Furthermore, comments do not influence
the meaning of a program; their sole purpose is the enlightenment of
the human reader.

Examples

*-- this is a comment*

**end**; *-- processing of Line is complete*

*-- a long comment may be split onto*

*-- two or more consecutive lines*

*---------------- the first two hyphens start the comment*

Reserved Words
--------------

 The following are the AADL reserved words. Reserved words are case
insensitive.

+-------------------+-------------------+----------------------+---------------------+
| **component**  | **access**| **all**  | **and** |
+-------------------+-------------------+----------------------+---------------------+
| **annex** | **applies**   | **binding**  | **bus** |
+-------------------+-------------------+----------------------+---------------------+
| **calls** | **classifier**| **compute**  | **configuration** |
+-------------------+-------------------+----------------------+---------------------+
| **constant**  | **data**  | **delta** | **device**  |
+-------------------+-------------------+----------------------+---------------------+
| **end**   | **enumeration**   | **event**| **extends** |
+-------------------+-------------------+----------------------+---------------------+
| **false** | **feature**   |  | **flow**|
+-------------------+-------------------+----------------------+---------------------+
|  | **group** | **implementation**   | **in**  |
+-------------------+-------------------+----------------------+---------------------+
| **inherit**   | **initial**   | **inverse**  | **is**  |
+-------------------+-------------------+----------------------+---------------------+
| **list**  | **memory**| **mode** |    |
+-------------------+-------------------+----------------------+---------------------+
|   | **not**   | **of**   | **or**  |
+-------------------+-------------------+----------------------+---------------------+
| **out**   | **package**   | **parameter**| **path**|
+-------------------+-------------------+----------------------+---------------------+
| **port**  |    | **process**  | **processor**   |
+-------------------+-------------------+----------------------+---------------------+
|  | **property**  |     | **provides**|
+-------------------+-------------------+----------------------+---------------------+
|  | **range** | **record**   | **reference**   |
+-------------------+-------------------+----------------------+---------------------+
| **refined**   | **renames**   | **requires **| **self**|
+-------------------+-------------------+----------------------+---------------------+
| **set**   | **sink**  | **source**   |    |
+-------------------+-------------------+----------------------+---------------------+
| **subprogram**| **system**| **thread**   | **to**  |
+-------------------+-------------------+----------------------+---------------------+
| **true**  | **type**  | **units**| **virtual** |
+-------------------+-------------------+----------------------+---------------------+
| **with**  |   |  | |
+-------------------+-------------------+----------------------+---------------------+

NOTE: The reserved words appear in lower case boldface in this standard.
Lower case boldface is also used for a reserved word in a
string\_literal used as an operator\_symbol. This is merely a convention
– AADL specifications may be written in whatever typeface is desired and
available.

