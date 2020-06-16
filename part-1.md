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
