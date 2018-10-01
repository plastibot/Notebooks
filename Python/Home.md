## House Password
Stephan and Sophia forget about security and use simple passwords for everything. Help Nikola develop a password security check module. The password will be considered strong enough if its length is greater than or equal to 10 symbols, it has at least one digit, as well as containing one uppercase letter and one lowercase letter in it. The password contains only ASCII latin letters or digits.

**Input:** A password as a string.
**Output:** Is the password safe or not as a boolean or any data type that can be converted and processed as a boolean. In the results you will see the converted results.

checkio('A1213pokl') == False
checkio('bAse730onE') == True
checkio('asasasasasasasaas') == False
checkio('QWERTYqwerty') == False
checkio('123456123456') == False
checkio('QwErTy911poqqqq') == True

**Precondition:**
re.match("[a-zA-Z0-9]+", password)
0 < len(password) ≤ 64


### Best Solutioms
```` python
# using RegExp
import re
def checkio(data):
    return True if re.search("^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).*$", data) and len(data) >= 10 else False

import re
def checkio(data):
    # (?=) is positive lookahead. Use this construction to make sure the enclosed pattern exists
    return bool(re.search(r'^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{10,}$', data))

import re
def checkio(d):
    return len(d) > 9 and all(re.search(p, d) for p in ('[A-Z]', '\d', '[a-z]'))
    
import re
DIGIT_RE = re.compile('\d')
UPPER_CASE_RE = re.compile('[A-Z]')
LOWER_CASE_RE = re.compile('[a-z]')
def checkio(data):
    if len(data) < 10:
        return False 
    if not DIGIT_RE.search(data):
        return False
    if not UPPER_CASE_RE.search(data):
        return False
    if not LOWER_CASE_RE.search(data):
        return False
    return True
    
#or with simple if statements
def checkio(data):
    if len(data) < 10:
        return False
    if data.upper() == data:
        return False
    if data.lower() == data:
        return False
    return any(c.isdigit() for c in data)
    
````

### My Solution
```` python
def checkio(data: str) -> bool:
    upper = False
    lower = False
    number = False
    if len(data) < 10:
        return False
    for c in data:
        if c.isupper():
            upper = True
        if c.islower():
            lower = True
        if c.isnumeric():
            number = True
    return upper & lower & number
````


## The Most Wanted Letter
You are given a text, which contains different english letters and punctuation symbols. You should find the most frequent letter in the text. The letter returned must be in lower case.
While checking for the most wanted letter, casing does not matter, so for the purpose of your search, "A" == "a". Make sure you do not count punctuation symbols, digits and whitespaces, only letters.

If you have two or more letters with the same frequency, then return the letter which comes first in the latin alphabet. For example -- "one" contains "o", "n", "e" only once for each, thus we choose "e".

**Input:** A text for analysis as a string.

**Output:** The most frequent letter in lower case as a string.

**Precondition:**
A text contains only ASCII symbols.
0 < len(text) ≤ 105

### Best Solutioms
```` python
import string
def checkio(text):
    text = text.lower()
    return max(string.ascii_lowercase, key=text.count)
    
# Or with lambda function
from string import ascii_lowercase as letters
checkio = lambda text: max(letters, key=text.lower().count)

# which s equivalent to:
from string import ascii_lowercase as letters
def checkio(text):
    return max(letters, key=text.lower().count)

# using Counter from collections library
from collections import Counter
def checkio(text):
    count = Counter([x for x in text.lower() if x.isalpha()])
    m = max(count.values())
    return sorted([x for (x, y) in count.items() if y == m])[0]

# using sorted
def checkio(text: str) -> str:
    st = sorted(set([i for i in text.lower() if i.isalpha()]))
    return sorted(st, key=text.lower().count, reverse=True)[0]
    

````

### My Solution
```` python
def checkio(text: str) -> str:
    top_count_letter = ""
    top_count = 0
    alphabet = "abcdefghijklmnopqrstuvwxyz"
    text = text.lower()
    for letter in alphabet:
        count = 0
        for character in text:
            if character == letter:
                count += 1
        if count > top_count:
            top_count = count
            top_count_letter = letter
    return top_count_letter
````

## Non Unique
You are given a non-empty list of integers (X). For this task, you should return a list consisting of only the
non-unique elements in this list. To do so you will need to remove all unique elements (elements which are 
contained in a given list only once). When solving this task, do not change the order of the list. 

Example: [1, 2, 3, 1, 3] 1 and 3 non-unique elements and result will be [1, 3, 1, 3].

**Input:** A list of integers.

**Output:** The list of integers.


### Best Solutioms
```` python
def checkio(data: list) -> list:
    return [i for i in data if data.count(i) > 1]
````

### My Solution
```` python
def checkio(data: list) -> list:
    keep = False
    response = []
    for i, x in enumerate(data):
        for j, y in enumerate(data):
            if x == y and i != j:
                keep = True
        if keep:
            response.append(data[i])
        keep = False
    return response
````

## monkey typing
The infinite monkey theorem states that a monkey hitting keys at random on a typewriter keyboard for an infinite length
of time will almost surely type out a given text, such as the complete works of John Wallis, or more likely, a Dan Brown
novel.

Let's suppose our monkeys are typing, typing and typing, and have produced a wide variety of short text segments. Let's
try to check them for sensible word inclusions.

You are given some text potentially including sensible words. You should count how many words are included in the given
text. A word should be whole and may be a part of other word. Text letter case does not matter. Words are given in lowercase and don't repeat. If a word appears several times in the text, it should be counted only once.

For example, text - "How aresjfhdskfhskd you?", words - ("how", "are", "you", "hello"). The result will be 3.

**Input:** Two arguments. A text as a string (unicode for py2) and words as a set of strings (unicode for py2).

**Output:** The number of words in the text as an integer.

count_words("How aresjfhdskfhskd you?", {"how", "are", "you", "hello"}) == 3
count_words("Bananas, give me bananas!!!", {"banana", "bananas"}) == 2
count_words("Lorem ipsum dolor sit amet, consectetuer adipiscing elit.",
            {"sum", "hamlet", "infinity", "anything"}) == 1

**Precondition:**
0 < len(text) ≤ 256
all(3 ≤ len(w) and w.islower() and w.isalpha for w in words)


### Best Solutioms
```` python
def count_words(text: str, words: set) -> int:
    d = [i for i in words if text.lower().find(i) != -1]
    return len(d)
````


### My Solution
```` python
def count_words(text: str, words: set) -> int:
    count = 0
    for word in words:
        if text.lower().find(word) >= 0:
            count += 1
            print(word)
    return count
````


## Long-Repeat
This mission is the first one of the series. Here you should find the length of the longest substring that consists of the
same letter. For example, line "aaabbcaaaa" contains four substrings with the same letters "aaa", "bb","c" and "aaaa". The 
last substring is the longest one which makes it an answer.

**Input:** String.

**Output:** Int.


### Best Solutioms
```` python
def long_repeat(line):
    m=0
    p=0
    for x in range(len(line)):
        if x<len(line)-1:
            if line[x]==line[x+1]:
                p+=1
            else:
                p=0
        if p+1>m:
            m=p+1
    return m
    
    
def long_repeat(line):
    count = 1
    maxi = 1
    if line != "":
        for i in range(1,len(line)):
            if line[i] == line[i-1]:
                count+=1
                if count > maxi:
                    maxi = count
            else:
                count = 1
        return maxi
    else:
        return 0


from itertools import groupby
def long_repeat(line):
    return max(len(list(g)) for _, g in groupby(line)) if line else 0


# Or slower than above.
from itertools import groupby
def long_repeat(line):
    return max((sum(1 for _ in g) for k, g in groupby(line)), default=0)
````



### My Solution
```` python
def long_repeat(line):
    if len(line) < 1:
        return 0
    count = 1
    top_count = 1
    line = line+" "   
    print("index, char, count, top-count")
    for index in range(len(line)-1):
        print("{},    {},   {},    {}".format(index, line[index], count, top_count))
        if line[index] == line[index+1]:
            count += 1
        else:
            if count > top_count:
                top_count = count
            count = 1
    return top_count
````

