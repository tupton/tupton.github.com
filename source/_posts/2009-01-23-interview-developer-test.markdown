---
date: '2009-01-23 18:02:19'
layout: post
slug: interview-developer-test
status: publish
title: Interview Developer Test
wordpress_id: '298'
categories:
- Code
- Howto
---

When I interviewed earlier this week, I was given a developer take-home test to complete as part of the application process. The test consists of six short coding problems, any three of which are required to be completed within 48 hours of receiving the test.

I do not think there are any issues with me posting the problems here, and I want to share my solutions. Just to be safe, I'll only post the three problems that I answered along with only some of my solution and methodology.

The problems could be answered in any of Java, C#, C/C++, Python, or PHP. I answered all three in Python. I wrote 227 lines of code for this test, including comments.

## Problem 1

> Write a program that justifies a given line of text to the specified width. The width is given on the first line of the input file, and the text in on the second.

Sample input:

    20
    The quick brown fox jumps over the lazy dog.

Sample output:

    The quick  brown fox
    jumps  over the lazy
    dog.

The algorithm I came up with split the input into a list of words. Then, I keep popping those words off of that list and into a new list until no more words can fit within the specified width. Using the length of each word in the list plus a space between each word (`len(" ".join(to_justify))`), one can figure out the number of required spaces.

I decided that I would distribute the required spaces one by one into each "slot" that requires spaces until there are no more remaining spaces. This means that for the first `x` slots, there would be `n` spaces, and for the remaining `number_of_slots - x` slots, there would be `n - 1` spaces. `number_of_slots` here is really just `len(to_justify) - 1`.

The formulas for coming up with `n` and `x` are related and fairly straightforward. In terms of modulo arithmetic, think of the slots as your modulo, adding a space to each slot as you traverse the slots. We can use an example to illustrate this. If you have a desired text width of 20 characters, and a current word length of 13 characters, you need 7 spaces. For the first `7 % 4 = 3` slots, you need `ceil(7 / 4) = 2` spaces, and for the last slot, you need 1 (which is also `floor(7  /4)` or just `7 / 4` in Python) space. You need to import the `math` module in order to access the `math.ceil` function.

A problem that arises with this method is words that are longer than a line. I got around this by printing the word in chunks of the maximum width until its length was less than that text width. `words_printed` is just a count of the number of words that have been printed on the line thus far. It was used instead of `len(to_justify)` for use in another section of the code.

    # Take care of words that are longer than our desired text width.
    while len(word) > text_width:
      if words_printed == 0:
        print word[:text_width]
        word = word[text_width:]

      else:
        # Here we fill the rest of the line with as much of the word as
        # we can, given that there is already text on the current line.
        print " ".join(to_justify) + " " + word[:(text_width -
          len(" ".join(to_justify)) - 1)]
        word = word[(text_width - len(to_justify) - 1):]
        to_justify = []
        words_printed = 0

## Problem 2

> Write a program that compacts a string in place. It should remove all whitespace and replace duplicate characters with only the first character.

Sample input:

    abb cddpddef gh

Sample output:

    abcdpdefgh

The `re` module made this one extremely easy. After grabbing the line from the input file, my entire solution consisted of one line.

    print re.compile(r'(.)1+', re.IGNORECASE).sub(r'g<1>', "".join(text.split()))

`split()` defaults to a whitespace delimiter and `join()` pushes an array into a string with the given delimiter. This removes all the whitespace in a string.

`re.compile()` takes a regular expression string as its argument and returns an `re` object instance. This object instance can then utilize a great many methods related to regular expressions. A lot more information can be found on [the Python docs site] [re pydoc]. Building a string with `r' '` tells Python to treat the string literally, e.g. the string `r'\n'` is two characters long (`\` then `n`). I passed the `re.IGNORECASE` flag to, well, ignore the case of the searched string.

[re pydoc]: http://docs.python.org/library/re.html "re -- Regular expression operations -- Python v2.6.1 documentation"

We can break down the regular expression into three segments. First, `(.)` creates a character group that matches nearly any non-whitespace character. This lets me match both numbers and letters. `1` lets me match that first character group exactly. `(..)` or `(.){2,}` or something similar would not work, as that would match any two characters in a row and not just duplicates. Lastly, `+` lets me match the previous group 1 or more times. In English, this regular expression says "match any character, then match that exact character 1 or more times directly after it."

The `sub` method allows me to substitute using regular expressions. I passed it our string that has gone through `split()` and `join()`, and tell it to replace any matches with the first group (`1`). Since this group only matches single characters, I am done. Regular expressions are powerful.

## Problem 3

> Print a 2D array in a clockwise spiral fashion.

Sample input:

    1 2 3
    4 5 6
    7 8 9

Sample output:

    1 2 3 6 9 8 7 4 5

I decided that this problem could be attacked using recursion. I quickly looked online for an example of what others had done, but they all seemed sloppy and inefficient. If I could write a function that printed the borders of an array in the desired order, I could recursively process the entire array.

I constructed the array by using `split()` on each each line of the input file, one by one. If the array passed to `print_borders()` has one row or one column, it prints it. If it has two rows, it prints the first row in order, and the second row backwards. Otherwise, it prints the top row in order, the right column in order, the bottom row in reverse, and the left column in reverse.

The `reversed()` top-level method produces an iterator on a list in reverse. This was very useful for the bottom row and left column. This is how I addressed the bottom row of the array:

    for item in reversed(array[-1]):
      "".join(item)
      
Using `array[-1]` also came in handy for the right column of the array:

    for item in array[1:-1]:
      print "".join(item[-1])
      
And also for extracting the part of the array that still needs to be processed:

    new_array = []
    for row in array[1:-1]:
      new_array.append(row[1:-1])

I think the idea of a developer test is great. It allows the interviewers a brief glimpse into what kind of worker or programmer you are, and it gives the applicant a sense of accomplishment. The fact that I knew how to attack and solve these problems also gave me some confidence in my own programming skills. Being a recent college graduate and not having much professional programming experience meant that I had no idea what to expect. Now I do, and I'm excited. I got the job, and I very much look forward to starting work.
