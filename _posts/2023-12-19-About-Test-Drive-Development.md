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
1. Write Tests
2. Expect Failure
3. Change in DIRTY Baby steps
4. Expect Succeeed
5. Refactor duplication and dirtyness
6. Move on / Rinse and Repeat

Red-Green-Refactor is another motto to understand this certain flow, Where Red means fail, Green means making it succeed, then the important Refactor~

The heart and core of TDD is to do things **quickly**, and in **small enough increments**, and at the same time, making sure the code quality is maintained, making sure everything is under control (of course, there are still situations where control is hard or near impossible to have)

Let's have a few real world examples on how this could be potentially applied (there also would be counter examples, so fear not)

# Example 1 - Looting System

Given the following requirements:
> Write a `Looting System` in which has a single procedure: `RandChooseLoot` that, upon given a loot table with weights and item names, will return a random loot in the table, given the weight.
> The higher the weight is, the more probability it will be chosen among all loot.
> Example:
> Suppose we have a table -> (3, stone) (1, fish)
> then it implies that => stone has a 3 out of 4 chance to be selected, fish has a 1 out of 4 chance to be selected
> so then if stone has been chosen, `RandChooseLoot` will then return the name of the loot, which is "stone"

How would one approach this system in a TDD way?

let's have a quick discussion before we start writing anything(they always help):
1. We would need a `Looting System`, it could be in a form of class
2. We would need a function with the name "RandChooseLoot" in that class
3. The inputs of `RandChooseLoot` requires a table of weights that matches with their item names, it could be in a form of pairs
4. 



# Pros
- It can remove the fear of changing, refactoring and deleting code, Since you know a lot of tests backs the code up.
- Write only what is **neccessary**.
- Tests acts as an example and documentation.
- It can help orient / influence your design code towards end users for your API since you are constantly writing Tests that uses the API.
- It helps decouple your code, since writing a test in the first place may require you to decouple in order to test.

# Cons
- The real benefits of TDD only comes at a pretty late stage of development
- The time and effort of using TDD is quite high compared to immediately write code and test it right after
- Some systems aren't easily / near impossible to test and mock, examples are such like: Simulations, Randomness and or multi-threading. (although if systems are designed with decoupling in mind, these could still be potentially tested, it all depends on the design of the system)
