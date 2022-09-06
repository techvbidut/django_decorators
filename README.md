# DJANGO DECORATORS

## What is a decorator in python?
It is a design pattern, which allows us to modify behaviour of a function or class.

## Why decorators are required?
1. A decorater is required when we want to modify a function without modifying the function itself.
2. Can also be used when we need to run the same code on multiple functions.
3. Example: Adding logs, testing performance, perform caching, authentication, permissions etc.

## Things to know
1. Decorators are also caller <b>wrapper/wrapper function</b>.
2. The function which is decorated is called <b>wrapped function</b>.
3. Python treats functions as <b>first class objects</b> which means python supports the concept of First Class functions. We will learn about this.
4. This leads to many powerful ways to use functions.

### What is first class function?
An entity that can be constructed at run-time, passed as a parameter, returned from a function, or assigned into a variable.

### Properties of first class functions
1. A function is an instance of the Object type.
2. Functions can be stored in variables.
3. Functions can be passed as arguments in another function.
4. A function can be returned from a function.
5. Functions can be stored in data structures such as hash tables, lists etc.


Examples for understanding First Class functions in Python

1. <b>Function is object</b>: Assigning function to a variable. `shout` points to function object referenced by `say`.
```
def say(msg):
    return msg.upper()
  
print(say('Karki')) # prints KARKI
  
shout = say
  
print(shout('Karki')) # prints KARKI
```

2. <b>Functions can be passed as argument to other function</b>: Functions that can accept other functions as arguments are also called `higher-order functions`.
```
def upper(text):
    return text.upper()
  
def lower(text):
    return text.lower()
  
def wish_birthday(func):
    wish = func("Happy Birthday")
    print (wish)
  
wish_birthday(upper) # prints: HAPPY BIRTHDAY
wish_birthday(lower) # prints: happy birthday
```

3. <b>Function can return another function</b>: Because functions are object.
```
def create_wish(x):
    def wish(y):
        return x,y
  
    return wish
  
wish = create_wish("phone")
  
print (wish("laptop"))
```



