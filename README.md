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
  
print (wish("laptop")) # returns: ('phone', 'laptop')
```

## Let us create a decorator!

```
def your_decorator(func):
    def inner(*args, **kwargs):
    
        print("Before execution of wrapped function")
        
        # executed wrapped function
        result = func(*args, **kwargs) 
        
        print("After execution of wrapped function")
        
        return result
         
    return inner
 
# The wrapped function
def your_function():
    print("Inside the function")
    
# assigning decorator
your_function = your_decorator(your_function) 

# executing function
your_function() 
```

### Let us try to understand the code written above.
1. The wrapped function here is `your_function()`
2. Decorater here is `your_decorater()` which takes a function as an argument.
3. This decorater has a inner funtion which taked any argument.
4. We execute the passed function `func` inside this `inner` function.
5. Before executing this `func` we print 'before execution' message, alternatively, you can manipulate the data or add your logic here.
6. We stored the `func` return value in `result` variable, which is eventually returned after the 'after execution' message.
7. Atlast, inner function is returned from the decorater.
8. The second last line, assigns the decorater to your function.
9. The last line executes or calls the function.
10. The result looks like:
```
Before execution of wrapped function
Inside the function
After execution of wrapped function
```

#### Note: Instead of assigning the decorater to your function like `your_function = your_decorator(your_function)`, you can simply write `@your_decorator` above your function definition.

### Understanding decoraters with practical example in django.
1. For debugging purpose, let us suppose we want to log some information like the number of queries or the execution time taken by every API view.
2. The best approach for implementing this will be making a custom decorator. 
3. Django provides us built in decorators as well, which we will not discuss in this tutorial. As understanding custom decorator is much important for a developer.
4. Let us now make a custom query debug decorator.

```
import time
import logging
from django.db import connection, reset_queries

log = logging.getLogger("django")

#making custom query debug decorator
def query_debug(func):

    def inner(*args, **kwargs):
    
        reset_queries()
        start_queries = len(connection.queries)
        start_time = time.perf_counter()

        result = func(*args, **kwargs)

        end_time = time.perf_counter()
        end_queries = len(connection.queries)

        queries = end_queries - start_queries
        time_taken = end_time - start_time

        log.info(f">>> QUERY DEBUG DECORATOR - Function : {func.__name__}")
        log.info(f">>> QUERY DEBUG DECORATOR - Number of Queries : {queries}")
        log.info(f">>> QUERY DEBUG DECORATOR - Finished in : {(time_taken):.2f}s")
        
        return result
    return inner
```

### Let us understand the code written above
1. The code above is simply a custom decorator. If you understood decorators in general then it will be easy for you  to understand the code above.
2. `reset_queries()` basically clears the query list manually.
3. `connection.queries` is a list of dictionaries in order of query execution. It contains `sql` (raw SQL statement) and `time` (execution time in seconds).
4. `len(connection.queries)` will give the number of queries executed.
5. `time.perf_counter()` returns the value (in fractional seconds) of a performance counter, i.e. a clock with the highest available resolution to measure a short duration. 
6. `__name__` is a built-in variable which evaluates to the name of the current module. (wrapped function's name in our case)

### Using this custom query debug decorator

```
@query_debug    
def your_function(self, request, *args, **kwargs):
    #your logic
    pass
```

<i>Your log will look something like this (depends on your function)</i>
```
>>> QUERY DEBUG DECORATOR - Function : your_function
>>> QUERY DEBUG DECORATOR - Number of Queries : 10
>>> QUERY DEBUG DECORATOR - Finished in : 1.81s
```

## Chaining decorators
<i>Using multiple decorators for a function</i>

```
def decor_outer(func):
	def inner():
		k = func()
		return k * k
	return inner

def decor_inner(func):
	def inner():
		k = func()
		return 3 * k
	return inner

@decor_outer
@decor_inner
def number():
	return 10

print(number()) # prints 900
```

<i>Let us understand the code above:</i>

1. First inner decorator multiplies 3 with 10 which gives 30.
2. The outer decorator gives square of the previous result i.e 30 * 30 = 900.
3. Therefore, we get the final result as 900.
4. I hope you got the order of execution of decorators.


