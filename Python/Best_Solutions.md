
## Say Hi
In this mission you should write a function that introduce a person with a given parameters in attributes.

**Input:** Two arguments. String and positive integer.

**Output:** String.

### Best Solution
```` python
def say_hi(name: str, age: int) -> str:
    return f"Hi. My name is {name} and I'm {age} years old"
````

### My Solution
```` python
def say_hi(name: str, age: int) -> str:
    return ("Hi, My name is{} and I'm {} years old".format(name, age))
````


## Correct Sentence
For the input of your function will be given one sentence. You have to return its fixed copy in a way so it’s always starts
with a capital letter and ends with a dot.

Pay attention to the fact that not all of the fixes is necessary. If a sentence already ends with a dot then adding another
one will be a mistake.

**Input:** A string.

**Output:** A string.

**Precondition:** No leading and trailing spaces, text contains only spaces, a-z A-Z , and .


### Best Solution
```` python
def correct_sentence(text: str) -> str:  
    return text[0].upper() + text[1:] + '.' * (not text.endswith('.'))
````

### My Solution
```` python
def correct_sentence(text: str) -> str:
    text_cap = text[0].upper() + text[1:]
    if text_cap[-1] != ".":
        text_dot = "".join(text_cap + ".")
        return text_dot
    else:
        return text_cap
````


## First Word
You are given a string where you have to find its first word.

When solving a task pay attention to the following points:

- There can be dots and commas in a string.
- A string can start with a letter or, for example, a dot or space.
- A word can contain an apostrophe and it's a part of a word.
- The whole text can be represented with one word and that's it.

**Input:** A string.

**Output:** A string.

**Precondition:** the text can contain a-z A-Z , . '



### Best solution
```` python
def first_word(text: str) -> str:
    return text.replace(".", " ").replace(",", " ").split()[0]
````

### My solution
```` python
def first_word(text: str) -> str:
    no_dots = text.replace(".", " ")
    words = no_dots.split()
    if words[0][0].isalpha():
        return words[0].strip(',')
    else:
        return words[1].strip(',')
````


## Second Index

You are given two strings and you have to find an index of the second occurrence of the second string in the first one.

Let's go through the first example where you need to find the second occurrence of "s" in a word "sims". It’s easy to 
find its first occurrence with a function index or find which will point out that "s" is the first symbol in a word "sims" 
and therefore the index of the first occurrence is 0. But we have to find the second "s" which is 4th in a row and that 
means that the index of the second occurrence (and the answer to a question) is 3.

**Input:** Two strings.

**Output:** Int or None

### Best Solution
```` python
def second_index(text: str, symbol: str) -> [int, None]:
    return text.find(symbol, text.find(symbol) + 1) if text.count(symbol) > 1 else None
````
### My Solution
```` python
def second_index(text: str, symbol: str) -> [int, None]:
    first = text.find(symbol)
    if first == -1:
        return None
    second = text.find(symbol, first+1)
    if second == -1:
        return None
    return second
````


## Between Markers
You are given a string and two markers (the initial and final). You have to find a substring enclosed between these two
markers. But there are a few important conditions:

The initial and final markers are always different.
- If there is no initial marker then the beginning should be considered as the beginning of a string.
- If there is no final marker then the ending should be considered as the ending of a string.
- If the initial and final markers are missing then simply return the whole string.
- If the final marker is standing in front of the initial one then return an empty string.

**Input:** Three arguments. All of them are strings. The second and third arguments are the initial and final markers.

**Output:** A string.

**Precondition:** can't be more than one final marker and can't be more than one initial



### Best Solutiom
```` python
return text[text.find(begin) + len(begin) if begin in text else 0:text.find(end) if end in text else len(text)]
````

### My solution
```` python
def between_markers(text: str, begin: str, end: str) -> str:
    start = text.find(begin)
    end = text.find(end)
    start_length = len(begin)
    if start == -1:
        start = 0
        start_length = 0
    if end == -1:
        end = len(text)
    #if end < start:
    #    return None
    return text[start+start_length:end]
````


## Best Stock
You are given the current stock prices. You have to find out which stocks cost more.

**Input:** The dictionary where the market identifier code is a key and the value is a stock price.

**Output:** A string and the market identifier code.
{
        'CAC': 10.0,
        'ATX': 390.2,
        'WIG': 1.2
    }
    
**Preconditions:** All the prices are unique.

### Best Solutiom
```` python
def best_stock(data):
    return max(data, key = data.__getitem__)

# Or with max
def best_stock(data):
    return max(data, key = data.get)
    
# OR with Lambda functions
def best_stock(data):
    return max(data, key=lambda stock: data[stock])

````

### My Solution
```` python
def best_stock(data):
    # your code here
    best_stock_name = None
    best_stock_price = 0
    for stock, price in data.items():
        if price > best_stock_price:
            best_stock_name = stock
            best_stock_price = price
    return best_stock_name
````


## Popular Words
In this mission your task is to determine the popularity of certain words in the text.

At the input of your function are given 2 arguments: the text and the array of words the popularity of which you need to determine.

When solving this task pay attention to the following points:

- The words should be sought in all registers. This means that if you need to find a word "one" then words like "one", "One", "oNe", "ONE" etc. will do.
- The search words are always indicated in the lowercase.
- If the word wasn’t found even once, it has to be returned in the dictionary with 0 (zero) value.

**Input:** The text and the search words array.

**Output:** The dictionary where the search words are the keys and values are the number of times when those words are occurring in a given text.

**Precondition:** The input text will consists of English letters in uppercase and lowercase and whitespaces.

### Best Solutioms
```` python
#Creating a buitin method to be used in dictionary comprehension.
def popular_words(text, words):
    lower_count = text.lower().split().count
    return {word: lower_count(word) for word in words}

# Shortest solution using map
def popular_words(text, words):
    return dict(zip(words, map(text.lower().split().count, words)))
    
def popular_words(text: str, words: list) -> dict:
    return dict([(i, text.lower().split().count(i)) for i in words])

#or

def popular_words(text: str, words: list) -> dict:
    return {w: text.replace('\n', ' ').lower().split(' ').count(w) for w in words}
````

### My Solution
```` python
def popular_words(text: str, words: list) -> dict:
    pop_words = {}
    text = " " + text.lower() + " "
    text = text.replace("\n", " ")
    
    for word in words:
        pop_words[word] = text.count(" " + word + " ")
    return pop_words
````


## Bigger Price
You have a table with all available goods in the store. The data is represented as a list of dicts

Your mission here is to find the TOP most expensive goods. The amount we are looking for will be given as a first argument and the whole data as the second one

**Input:** int and list of dicts. Each dicts has two keys "name" and "price"

**Output:** the same as the second Input argument.


### Best Solutioms
```` python
# Using Lambda function
def bigger_price(limit, data):
    return sorted(data, key=lambda x: x['price'], reverse=True)[:limit]

# using lambda function and list comprehension
def bigger_price(limit: int, data: list) -> list:
    return [x for x in sorted(data, key=lambda x: -x['price'])][:limit]

# using list function???    
def bigger_price(limit: int, data: list) -> list:
    return list(sorted(data, key=lambda x: x['price'], reverse=True))[:limit]
    
def bigger_price(limit, data):
    return sorted(data, key=lambda x: x.get("price"), reverse=True)[:limit]

# using itemgetter
from operator import itemgetter
def bigger_price(limit: int, data: list) -> list:
    return sorted(data, key = itemgetter("price"), reverse=True)[0:limit]
````

### My Solution
```` python
def bigger_price(limit: int, data: list) -> list:
    sorted_items = sorted(data, key=itemgetter('price'), reverse=True)        
    return [item for count, item in enumerate(sorted_items) if count < limit]
''''
