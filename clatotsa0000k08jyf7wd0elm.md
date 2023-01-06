# Learn to write better and stress-free code by using linters

## The Problem
As a developer, it's a common experience to be bogged down by syntax errors that could be very time consuming. Sometimes it's just a missing comma, a mismatched parenthesis or a simple mistype in a variable name that throws errors that are not always so obvious. Someone would say that such simple mishaps are very easy to debug, but it can be very frustrating if you are tired at the end of the day and end up being stuck for some minutes up to some days trying to catch a syntax error in a line that looks perfectly fine only to find out that the mismatched parenthesis that is causing the error is a few lines above.
Being told to be more focused or cautious when writing code does not help, as we are not always in the perfect mood or energy level when coding. It would be perfect if every developer had enjoyed 8 hours of sleep, had some physical workouts before starting to code and have a calm, happy mood, but this does not happen very often in the real world.

## Style stuck
Another point of frustration is following style guides that are certainly helpful and make your code cleaner, more readable and more professional, but are quite lengthy and they change depending on the particular language you are using, company that enforces them and even individual preferences step in.
As someone who is learning a particular style, it is helpful if someone could point to us what parts of our code need more attention. By correcting our mistakes at the same time we write code, it is possible to gradually learn the new ways on the fly instead of simply reading the style guides, which is certainly uninteresting and unproductive, as most of the effort will be dissolved by our forgetful minds in the long run.

## The Solution
I find surprising that I do not hear much about linters, especially when they can save countless of hours of debugging and rewriting code. Imagine writing a pretty complicated essay without the aid of auto-correct in your text editor. Sure, if you are a professional writer or someone who majored in English, you may think these kind of helpers as a nuisance, but for the rest of us, it surely brings some benefits.
Linters are pretty much like the text editor's auto-correct, but key one difference is that they are not activated by default in the code editors and one linter only covers a particular language. You cannot use the same linter of writing Python and Javascript, for example.
Having a fast feedback that highlights problems on the fly also helps the learning process as you don't have to run the code or rely on tests to receive an error. Also, using linters is the first line of defense for avoiding bugs, a complement of running tests.

## My experience with linters in Python
There are many linters that could be used with Python, but two popular ones are flake8 and pylint. They not only pick style errors, but also [anti-patterns](https://en.wikipedia.org/wiki/Anti-pattern) like:

```python
try:
  # Do something that could throw an error
except:
  # What to do if an error appears
``` 

Which could hide errors that are very difficult to debug, as the except will catch any kind of error, not only the error that you were thinking in the first place. This could lead to a situation where your except statement hides an unexpected error and causing strange behavior that does not raise an exception, instead of simply raising the error and prompting you to fix it. The code above can be improved by specifying the error type that you intend to catch and leaving other errors to appear:


```python
try:
  # Do something that could throw an ImportError
except ImportError:
  # What to do if an ImportError is raised, other errors will still be raised
``` 

It is like having a mentor for yourself, pointing out what could be improved in your code and training you to be a better developer!

## Closing thoughts
Do not despair if a linter throws a lot of problems the first time it is activated. It is not mandatory to solve every single problem, but it is worthwhile to take a look on the most urgent ones. The problems are categorized by importance, so a styling issue will not have the same weight as something that could prevent you code to run, like a module that does not exist, but it is being imported.
You could also include linters in your CI process by using them to check your code before pushing new code to a repository. This will further guarantee the you can produce reliable code consistently and effortlessly, by using automated processes.