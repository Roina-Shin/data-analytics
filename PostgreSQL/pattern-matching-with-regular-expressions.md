## Pattern matching with regular expressions

- If you need to locate text strings within the values stored in a database, then you could use a **like** operator.


- But a more powerful approach is to leverage regular expressions. Regular expressions provide a pattern-matching syntax that's used in a wide variety of programming languages.


- So, like learning structured query language, taking the time to learn how regular expressions work will benefit you throughout your entire career in data science.


```
-- Regular Expressions Usage

'input string' [match operator] [character sequence]

-- Regular Expression Match Operators

~  matchies string case sensitive
~* matchies string case insenstive
!~ excludes string case sensitive
!~* excludes string case insensitive

-- Regular Expression Character Sequences

~ 'abc'  matches to the string 'abc' only: 'abcd', 'xabcy'
~ 'c.t'  . matchies any single character: 'cat', 'cot', but not 'coat'
~ '(at|an)'  | inside () indicates OR: will match 'boat', 'plane', but not 'train'
~ '[abc]'  [ ] defines a character set: 'cat', 'boy', but not 'dog'
~ '^a'  ^ anchors the pattern to the start: 'apple', but not 'banana'
~ 'a$'  $ anchors the pattern to the end 'banana', but not 'apple'
```


- Execuate the following queries line by line:


```
SELECT 'abracadabra' ~ 'ca';
SELECT 'abracadabra' ~ 'abc';
SELECT 'abracadabra' ~ 'ab..c';
SELECT 'abracadabra' ~ 'ax|ay|ac';
SELECT 'abracadabra' ~ '[xyz]';
SELECT 'abracadabra' ~ '^c';
SELECT 'abracadabra' ~ 'a$';
```


![regular-expressions](/pictures/PostgreSQL/additional-querying-techniques/regular-expressions.PNG "regular expressions")


- We will use the match character '~' (tilde) to find the product_name that includes 'f' in it.


```
select product_name
from inventory.products
where product_name ~ 'f';
```


![tilde-match-character](/pictures/PostgreSQL/additional-querying-techniques/tilde-match-character.PNG "tilde match character")


- And to find the product names that start with character 'b' case insensitive, run the following:


```
select product_name
from inventory.products
where product_name ~* '^b';
```


![start-with-b-case-insensitive](/pictures/PostgreSQL/additional-querying-techniques/start-with-b-case-insensitive.PNG "start with b case insensitive")


- You can use case-insensitive LIKE operator, **ILIKE** to locate product_name that includes 'infused' in it.


```
select product_name,
	count(*)
from inventory.products
group by product_name
having product_name ilike '%infused%';
```


![ilike-operator-percent-signs](/pictures/PostgreSQL/additional-querying-techniques/ilike-infused-percent-signs.PNG "ilike operator percent signs")


- Regular expressions are an extremely powerful way to interact with text fields not only in PostgreSQL but in all kinds of programming contexts as well.