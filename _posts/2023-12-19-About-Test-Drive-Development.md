(Post is not yet finished, this is a draft)
Long time no see!

It has been a while since I last wrote a post, I was working more on my own life, and now I feel better than ever~

# Something I've learnt

So enough chit chatting, I was recently playing with TDD (Test Driven Development), and am particularly interested in [Extreme Programming](https://en.wikipedia.org/wiki/Extreme_programming) with [Agile Development](https://en.wikipedia.org/wiki/Agile_software_development).

As far as I have gotten in programming, and designing systems. I have always been quite slow, and it's sometimes demotivating, so TDD, Extreme and Agile Development sparked my interest.

There are several books I've read while trying it myself, a book to note is [Test-Driven Development By Example - Kent Beck](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530), it may be hard to read at first (believe me, programming sins all over the place), but it does give some practical examples on how to tackle a TDD by literally solving a complex example throughout the book.

My goal in this post is hoping to clear up peoples' understanding on what TDD is and what TDD IS NOT after trying it for almost half a year. (because people do seemingly misunderstood / misuse TDD)


# What is it?

In short for you folks who do not know about what TDD is about:
> Test Drive development is a software development process which relies on converting requirements into test cases that can be potentially fully tested.

before we get into what exactly you do, in TDD and why, we should make very clear points on common misconceptions about TDD.

# Common Misconceptions

1. Thinking that TDD can replace design (no it cannot, it can only help influence you to design towards end users)
    - this is quite a bad way on expecting TDD can design; Yes it does help you with design, probably change your way on thinking your designing choices, BUT, that doesn't mean you don't need to think about design in the first place.
    - It is a great tool to help and influces your design, to help you think in the perspective of users of your system / code, writing what is only neccessary (which is [YAGNI - extreme programming principle](https://en.wikipedia.org/wiki/You_aren't_gonna_need_it))

2. we MUST follow TDD principles at all times
    - no, that, in my honest opinion, is just not worth it, due to the cost of writing the unit test, time to running the test, doesn't really help much.
    - Principles are just Principles, they are meant to help you design, not to give burden, only use them when you know they can really help you reduce your work a ton / really shine.
  
3. We MUST TDD in all cases! (or in other words: to have near 100% test coverage!)
    - same idea as what I have said above in 2, it is just not worth your time and effort trying to write a unit test for just a few lines of code that may not be critical in your software
  

With those out of the way! Let's finally introduce you on the steps exactly on how to use TDD~

# Steps
TDD is quite a rabbit hole to go through, I probably will not cover all of TDD but rather the mere basis, to get familiar with the rhythm of TDD :P
According to the book Test-Driven Development By Example, these are the basic rhythm of TDD:
1. Quickly add a test
2. Run all tests and see the new one fail
3. Make a little change
4. Run all tests and see them all succeed
5. Refactor to remove duplication

Which, in other words, basically boils down to:
> Write Tests -> Expect Failure -> Change in DIRTY Baby steps -> Expect Succeeed -> Refactor duplication and dirtyness (rinse and repeat)

In other terms, Red-Green-Refactor is an easier to remember  

The heart and core of TDD is to do things **quickly**, and in **small enough increments**, making sure everything is under control (of course, there are still situations where control is hard or near impossible to be under)


This is not really particularly useful nor interesting, so let's have a few real world examples on how this could be potentially applied (there also would be counter examples, so fear not)

# Example 1 - 

# Pros

# Cons
