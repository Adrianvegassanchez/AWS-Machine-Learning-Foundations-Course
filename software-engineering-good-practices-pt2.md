## Software Engineering Good Practices - pt 2
### Testing

### Unit Test: 

A type of test that covers a “unit” of code, usually a single function, independently from the rest of the program.

```
[...]
nearest_5 = nearest_square(5)
print("Nearest square <= 5 returns {} - Expected : 4".format(nearest_5))
assert(nearest_5 == 4)
[...]
```

### Test Driven Development (TDD) and Data Science:
 
+ Writing tests before you write the code that’s being tested. Your test would fail at first, and you’ll know you’ve
  finished implementing a task when this test passes.
+ Tests can check for all the different scenarios and edge cases you can think of, before even starting to write your
 function.
+ Tests also helps ensure that your function behavior is repeatable, regardless of external parameters, such as
 hardware and time.
 
 Some interesting links : 
 
 [Data Science TDD](https://www.linkedin.com/pulse/data-science-test-driven-development-sam-savage/)
 [TDD for Data Science](http://engineering.pivotal.io/post/test-driven-development-for-data-science/)
 [TDD is Essential for Good Data Science Here's Why](https://medium.com/@karijdempsey/test-driven-development-is-essential-for-good-data-science-heres-why-db7975a03a44)
 [Testing Your Code (general python TDD)](http://docs.python-guide.org/en/latest/writing/tests/)