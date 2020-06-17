## Software Engineering Good Practices
### Clean and Modular code

A clean code must be : Simple, readable, concise.
This gonna make yout code easier to reuse, read, collaborate and makes you write less code.

### Refactoring Code

Restructuring code to improve internal structure without changing external functionality.

**Why Refactor?**
+ Reduce workload in long run
+ Easier to maintain code
+ Reuse more of your code
+ Become a better developer

### Writing Clean Code

**Meaningful Names**
+ Be descriptive and imply type
+ Be consistent but crearly differentiate
+ Avoid abbreviations and especially single letters
+ Long names != descriptitve names

**Use whitespace properly**
+ Standard is to use 4 spaces for each indent.
+ This can be setup as default in most of IDEs, like limit your lines
to around 79 characters.

[More about this](https://www.python.org/dev/peps/pep-0008/?#code-lay-out)

**Clean Code Exercise 1 :**

Imagine you are writing a program that executes a number of tasks and categorizes each task based on its execution time. Below is a small snippet of this program. Which of the following naming changes could make this code cleaner?

```
t = end_time - start  # compute execution time
c = category(t)  # get category of task
print('Task Duration: {} seconds, Category: {}'.format(t, c)
```
After refactoring : 

```
execution_time = end_time - start_time
category = cateogirze_task(execution_time)
print('Task Duration: {} seconds, Category: {}'.format(execution_time, category)
````
### Writing Modular Code

+ DRY (Don`t repeat Yourself)
+ Abstract out logic to improve readability
+ Minimize the number of entities (functions, classes, modules, etc.)
+ Functions should do one thing (Atomize)
+ Arbitrary variable names can be more effective in certain functions
+ Try to use fewer than three arguments per function

**Clean Code Exercise 2 - Refactoring  :**

_You want to replace the spaces in the column labels with underscores to be able to reference columns with dot notation. Here's one way you could've done it._

First solution : 

```
new_df = df.rename(columns={'fixed acidity': 'fixed_acidity',
                             'volatile acidity': 'volatile_acidity',
                             'citric acid': 'citric_acid',
                             'residual sugar': 'residual_sugar',
                             'free sulfur dioxide': 'free_sulfur_dioxide',
                             'total sulfur dioxide': 'total_sulfur_dioxide'
                            })
new_df.head()
```

Refactor solution 1 : 

```
labels = list(df.columns)

for label in labels :
    label = label.replace(' ', '_')
df.columns = labels

df.head()
```

Refactor solution 2 (Better than before) : 

```
df.columns = [label.replace(' ', '_') for label in df.columns]
df.head()
```

### Efficient Code

+ Execute faster
+ Take up less space in memory/storage

[More about this - What makes sets faster than lists](https://stackoverflow.com/questions/8929284/what-makes-sets-faster-than-lists/8929445#8929445)

Optimizing Code: 

Given a code to find the common book ids in _books_published_last_two_years.txt_ and _all_coding_books.txt_ to obtain a list of recent books.

```
start = time.time()
recent_coding_books = []

for book in recent_books:
    if book in coding_books:
        recent_coding_books.append(book)

print(len(recent_coding_books))
print('Duration: {} seconds'.format(time.time() - start))

```

Result : 
96
Duration: 17.437314748764038 seconds

**Tip #1: Use vector operations over loops when possible**

Use numpy's intersect1d method to get the intersection of the recent_books and coding_books arrays.

```
import numpy as np

start = time.time()
recent_coding_books = np.intersect1d(recent_books, coding_books)
print(len(recent_coding_books))
print('Duration: {} seconds'.format(time.time() - start))

```

Result :
96
Duration: 0.031774282455444336 seconds

**Tip #2: Know your data structures and which methods are faster**

Use the set's `intersection` method to get the common elements in `recent_books` and `coding_books`.

```
start = time.time()
recent_coding_books = set(recent_books).intersection(coding_books)
print(len(recent_coding_books))
print('Duration: {} seconds'.format(time.time() - start))
```

Result : 
96
Duration: 0.0070879459381103516 seconds

