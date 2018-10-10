## Roman Numerals
Roman numerals come from the ancient Roman numbering system. They are based on specific letters of the alphabet which are combined to signify the sum (or, in some
cases, the difference) of their values. The first ten Roman numerals are:

I, II, III, IV, V, VI, VII, VIII, IX, and X.

The Roman numeral system is decimal based but not directly positional and does not include a zero. Roman numerals are based on
combinations of these seven symbols:

Numeral |	Value
-------- | ------
I |	1 (unus)
V	| 5 (quinque)
X	| 10 (decem)
L	| 50 (quinquaginta)
C	| 100 (centum)
D	| 500 (quingenti)
M	| 1,000 (mille)


For this task, you should return a roman numeral using the specified integer value ranging from 1 to 3999.

**Input:** A number as an integer.

**Output:** The Roman numeral as a string.

**Precondition:** 0 < number < 4000

### Best Solutions
```` python

````

### My Solution
````python
def checkio(data):
    combos = (
        ("MMM", 3000), 
        ("MM", 2000), 
        ("M", 1000), 
        ("CM", 900), 
        ("DCCC", 800), 
        ("DCC", 700),
        ("DC", 600),
        ("D", 500),
        ("CD", 400),
        ("CCC", 300),
        ("CC", 200),
        ("C", 100),
        ("XC", 90),
        ("LXXX", 80),
        ("LXX", 70),
        ("LX", 60),
        ("L", 50),
        ("XL", 40),
        ("XXX", 30),
        ("XX", 20),
        ("X", 10),
        ("IX", 9),
        ("VIII", 8),
        ("VII", 7),
        ("VI", 6),
        ("V", 5),
        ("IV", 4),
        ("III", 3),
        ("II", 2),
        ("I", 1)
        )
    roman = ""
    for comb in combos:
        if data >= comb[1]:
            data -= comb[1]
            roman = roman + comb[0]
    return roman
````



## Reverse Roman Numerals


