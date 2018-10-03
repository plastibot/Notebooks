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
# using lambda
count_words=lambda t,W:sum(w in t.lower()for w in W)

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


### Best Solutions
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


## All the Same
In this mission you should check if all elements in the given list are equal.

**Input:** List.

**Output:** Bool.

**Precondition:** all elements of the input list are hashable


### Best Solutioms
```` python
def all_the_same(elements):
   return elements[1:] == elements[:-1]

def all_the_same(elements):
   return elements == elements[1:] + elements[:1]
   
# Using Lambda function
all_the_same = lambda elements: not elements or [elements[0]] * len(elements) == elements

# Use the fact that sets cannot hold duplicate values by converting to set and then testing if len() is more than 1
def all_the_same(elements: List[Any]) -> bool:        
    return False if len(set(elements)) > 1 else True

# Using built-in function all(iterable, /)
def all_the_same(elements: List[Any]) -> bool:
    return all(x==elements[0] for x in elements)

# Using built-in function any(iterable, /)
def all_the_same(elements: List[Any]) -> bool:
    if any(elements[i] != elements[i + 1] for i in range(len(elements) - 1)):
        return False
    else:
        return True
    
````

### My Solution
```` python
def all_the_same(elements: List[Any]) -> bool:
    # your code here
    try:
        first_element = elements[0]
    except:
        return True
    for element in elements:
        if element != first_element:
            return False
    return True
````


## Caesar Cipher
Your mission is to encrypt a secret message (text only, without special chars like "!", "&", "?" etc.) using Caesar cipher where each letter of input text is replaced by another that stands at a fixed distance. For example ("a b c", 3) == "d e f"

**Input:** A secret message as a string (lowercase letters only and white spaces)

**Output:** The same string, but encrypted

**Precondition:**
0 < len(text) < 50
-26 < delta < 26

### Best Solutons
````python
from string import ascii_lowercase as ascii
def to_encrypt(text, delta):
    return ''.join([ascii[(ascii.index(char) + delta) % len(ascii)] if char in ascii else char for char in text])

def to_encrypt(text, delta):
    alpha = [chr(x) for x in range(97,123)]
    txt = ''
    for char in text:
        if char in alpha:
            txt += alpha[(alpha.index(char)+delta)%26]
        else:
            txt += ' '        
    return txt
    
# Or Lambda function
to_encrypt=lambda t,d:''.join((c,chr(97+(ord(c)-97+d)%26))[ord(c)>>6]for c in t)

# Or similar to my response but with some simplification on the limits.
def to_encrypt(text, delta):
    #replace this for solution
    res = ''
    for e in text:
        if e != ' ':
            res += chr((ord(e)+delta -97) % 26 +97)
        else:
            res += ' '
    return res
````

### My Solution
```` python
def to_encrypt(text, delta):
    #replace this for solution
    crypto = ""
    for c in text:
        if ord(c) == 32:
            offset = 0
        elif (ord(c) + delta) > 122:
            offset = delta - 26
        elif (ord(c) + delta) < 97:
            offset = delta + 26
        else:
            offset = delta
        crypto = crypto + chr(ord(c)+offset)
    return crypto
````


## Sun Angle
Your task is to find the angle of the sun above the horizon knowing the time of the day. Input data: the sun rises in the
East at 6:00 AM, which corresponds to the angle of 0 degrees. At 12:00 PM the sun reaches its zenith, which means that the
angle equals 90 degrees. 6:00 PM is the time of the sunset so the angle is 180 degrees. If the input will be the time of the
night (before 6:00 AM or after 6:00 PM), your function should return - "I don't see the sun!".

**Input:** The time of the day.

**Output:** The angle of the sun, rounded to 2 decimal places.

**Precondition:**
00:00 <= time <= 23:59

### Best Solutons
````python
def sun_angle(time):
    h, m = map(int, time.split(':'))
    angle = 15 * h + m / 4 - 90
    return angle if 0 <= angle <= 180 else "I don't see the sun!"


def sun_angle(time):    
    t_min = (int(time[0:2])-6)*60 + int(time[3:5])    
    if t_min < 0 or t_min > 720:
        return 'I don\'t see the sun!' 
    return t_min/4.0
    
def sun_angle(time):
    n = ((int(time[:2])-6)*60+int(time[-2:]))*0.25
    return n if 0 <= n <= 180 else "I don't see the sun!"
    
def sun_angle(time):
    return "I don't see the sun!" if int(time[:2]) not in range(6, 18) else (((int(time[:2]) - 6) * 15) + (int(time[3:]) * 0.25))
    
# using lambda function
sun_angle=lambda t:["I don't see the sun!",int(t[:2])*15+int(t[~1:])/4-90][6<=int(t[:2])+int(t[~1:])/60<=18]

# using RegExp
import re
def sun_angle(time):
    minute=int((re.search(r":..",time)).group()[1:]) + (int((re.match(r"..",time)).group()))*60
    minute=(minute-360)/720*180
    if minute >= 0 and minute <= 180:
        return round(minute,2)
    return "I don't see the sun!"

````

### My Solution
```` python
def sun_angle(time):
    # 12hrs * 60 min = 720 minutes = 180deg
    # 4minutes = 1 deg.
    # 1 minute = 0.25 deg
    hours = int(time.split(":")[0])
    minutes = int(time.split(":")[1])
    
    if hours == 18 and minutes == 0:
        return 180
    if hours < 6 or hours > 17:
        return "I don't see the sun!"
    else:
        return ((hours - 6) * 60 + minutes) * 0.25
````

### Retrospective
- I could have used map to do hours and minutes at once. hours, minutes = map(int(time.split(":")))
- it would have been shorter to convert time to minutes before checking to save some if statements.

```` python
def sun_angle(time):
    hours, minutes = map(int(time.split(":")))
    time_in_minutes = hours*60 + minutes
    if time_in_minutes < 360 or time in minuted > 1080":
        return "I don't see the sun!"
    return (time_in_minutes - 360) * 0.25
````
    
