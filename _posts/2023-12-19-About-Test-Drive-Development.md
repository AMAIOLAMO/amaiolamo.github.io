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

Red-Green-Refactor is another motto to understand this certain flow,
Where Red means fail, Green means making it succeed, then the important Refactor~

The heart and core of TDD is to do things **quickly**, and in **small enough increments**, and at the same time, making sure the code quality is maintained, making sure everything is under control (of course, there are still situations where control is hard or near impossible to have)

Let's have a few real world examples on how this could be potentially applied (there also would be counter examples, so fear not)

I will be using C# for the following examples, but it should apply to other object-oriented languages as well! (maybe also functional languages perhaps?)

For the Test Framework, I will be using NUnit

# TDD - Looting System

Given the following requirements:
> Write a `Looting System` in which has a single procedure: `RandChoose` that, upon given a loot table with weights and item names, will return a random loot in the table, given the weight.

> The higher the weight is, the more probability it will be chosen among all loot.

> Example:

> Suppose we have a table -> (3, stone) (1, fish)

> then it implies that => stone has a 3 out of 4 chance to be selected, fish has a 1 out of 4 chance to be selected.
> so then if stone has been chosen, `RandChoose` will then return the name of the loot, which is "stone".

How would one approach this system in a TDD way?

let's have a quick discussion before we start writing anything(they always help):
1. We would need a `Looting System`, it could be in a form of class
2. We would need a function with the name `RandChoose` in that class
3. The inputs of `RandChoose` requires a table of weights that matches with their item names, it could be in a form of pairs
4. Finally, `RandChoose` should simply return the name of the item randomly chosen

So, according to TDD, we should start with the most simplest as of right now we can think of
In another way, we can think of how to use `RandChoose` in the most simplest way, An entirely empty table!

let's start with a unit test!

```csharp
[TestFixture]
public class LootSystemTests
{
	[Test]
	public void TestEmpty()
	{
		string result = LootSystem.RandChoose( new List<(int weight, string itemName)>() );
		string expected = string.Empty;

		Assert.That( result, Is.EqualTo( expected ) );
	}
}
```

This test first calls the `RandChoose` static function from the `LootSystem` class, passes in an empty list of a `tuple` (int, string),
and expects an empty string return.

Currently, we definitely do not have the class `LootSystem`, the static method `RandChoose`,
so let's make it compile by adding the two things needed.

```csharp
public class LootSystem
{
	public static string RandChoose( List<(int weight, string itemName)> lootTable )
	{
		return "DONT COMPILE";
	}
}
```

note that you can see, I returned "DONT COMPILE" instead of an empty string, in TDD, we usually expect failure,
now I can hear some you may say: "Why should I do this when I already know the answer is right there?", and you are right.
I am just writing this to demonstrate how TDD can work, but doesn't mean that it should be enforced.

so continue, we can see that, easily, when we run the test, it definitely fails.

now we are in next stage, let's make this suceed in the easiest way!

```csharp
public static string RandChoose( List<(int weight, string itemName)> lootTable )
{
  return string.Empty;
}
```

Simply we just return an empty string! Bam, baby and small incremental steps.

After making sure that the test pass, make it green, we can now continue to the most important part: Remove duplication

Alright, you ask: "where is the duplication?", "we only have a single string.Empty..."

Wait wait wait, now, hold there, look at where string.Empty occured... Yes~ It occurred both in the Test and in the implementation!

So to remove such duplication, what we usually do is to "generalize". We can start thinking where does this `string.Empty` comes from?

It is an exceptional case! So right now, without other tests to help, we can't really generalize up, So let's not remove this duplication.. just yet :D

let's move onto test 2

```csharp
[Test]
public void TestOneItem()
{
  string result = LootSystem.RandChoose
  (
    new List<(int weight, string itemName)>
    {
      ( 10, "Stone" )
    }
  );
  string expected = "Stone";

  Assert.That( result, Is.EqualTo( expected ) );
}
```

So here, we move onto the next easiest case, which is testing one single loot item, and we expect "Stone" being always the result.

So running the test, Expecting it fail.

Now let's move onto trying to do the easiest way to fix it! And by easiest... I meant changing it into if case!

```csharp
public static string RandChoose( List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  return "Stone";
}
```

Now the test should succeed, and let's check for duplication.

Aha, "Stone" is a duplication of the "Stone" in the test! How would one generalize a constant?
One way is to generalize into a variable, so which variable stores the word "Stone"?

the `lootTable`'s first element!

so let's do just that.

```csharp
public static string RandChoose( List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  return lootTable[ 0 ].itemName;
}
```

Moving onto the third test, which will change a lot of things later on.

```csharp
[Test]
public void TestTwoItems()
{
  string result = LootSystem.RandChoose
  (
    new List<(int weight, string itemName)>
    {
      ( 6, "Stone" ),
      ( 1, "Sand" )
    }
  );
  string expected = ???; // how do we expect a randomized value?

  Assert.That( result, Is.EqualTo( expected ) );
}
```

Now, while writing the third test, you can immediately see one problem. We cannot simply test randomization!

So that must mean our design has some flaws, rendering it to be not testable. Thus, before we continue onwards, we should have another updated design discussion again.

Our problem here is the test (user side) cannot determine the output of `RandChoose`, but we also do not want to expose the contents of **LootSystem**.

So instead, we could redesign the request to allow the input to also predict what the Choosing Method can handle.

In our situation, `RandChoose` can receive another argument, that argument can be used to determine the output, although now, the randomization is given to the user to determine!

That makes the system more modular, as now the user can have customization on how to handle the determining factor of randomization, instead of the `LootSystem` handling this.

So before we continue, we know that our design is flawed, we should comment out the third test, because we cannot simply make it compile easily in the first step.

```csharp
public static string RandChoose( float choiceValue, List<(int weight, string itemName)> lootTable )
```

And since, RandChoose isn't really `Random` anymore, we can safely remove the `Rand` prefix in the name of the method

```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
```

Now, our older tests are shouting to be fixed, so let's fix them quickly.

```csharp
[Test]
public void TestEmpty()
{
  string result = LootSystem.Choose( 0f, new List<(int weight, string itemName)>() );
  string expected = string.Empty;

  Assert.That( result, Is.EqualTo( expected ) );
}
```

```csharp
[Test]
public void TestOneItem()
{
  string result = LootSystem.Choose
  (
    0f,
    new List<(int weight, string itemName)>
    {
     ( 10, "Stone" )
    }
  );
  string expected = "Stone";

  Assert.That( result, Is.EqualTo( expected ) );
}
```

Now after getting success from the tests, we can continue to uncomment the third test, and write down the functionality we wanted.

```csharp
[Test]
public void TestTwoItems()
{
  // stone is 0 ~ 0.857, but the choice Value is 0.88f, so it should be in the range of Sand, which is
  // (0.857 ~ 1)
  string result = LootSystem.Choose
  (
    0.88f,
    new List<(int weight, string itemName)>
    {
      ( 6, "Stone" ), // probability: 6/7 -> 0.857 <=> range: 85.7% -> 0 ~ 0.857
      ( 1, "Sand" )   // probability: 1/7 -> 0.143 <=> range: 14.3% -> 0.857 ~ (0.857 + 0.143) = 1
    }
  );
  
  string expected = "Sand";

  Assert.That( result, Is.EqualTo( expected ) );
}
```

So to be more clear, I wrote some more exact situation on how we would want to expect the `Choose` to should function given the `choiceValue`

Running the test and expecting a failure, let's continue converting it into an if statement

```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  if ( lootTable.Count == 1 )
    return lootTable[ 0 ].itemName;

  return "Sand";
}
```

Running the test, we have passed... Now onto removing duplication. Here you can see that "Sand" is an obvious duplication between implementation and the test, we can change it to be more generalized by using `lootTable[1].itemName`.

```csharp
if ( lootTable.Count == 1 )
  return lootTable[ 0 ].itemName;

return lootTable[ 1 ].itemName;
```

After making sure the test pass. Another duplication still exist, but where is it? A duplication exist in tests, where we always test a result and an expected variable through the `Choice` method.

```csharp
void TestChoice( float choiceValue, List<(int weight, string itemName)> lootTable, string expected )
{
  string result = LootSystem.Choose( choiceValue, lootTable );
  Assert.That( result, Is.EqualTo( expected ) );
}
```

then replacing all the tests using this newly extracted method inside the tests.

```csharp
[Test]
public void TestEmpty()
{
  TestChoice( 1f, new List<(int weight, string itemName)>(), string.Empty );
}

[Test]
public void TestOneItem()
{
  TestChoice
  (
    0f,
    new List<(int weight, string itemName)>
    {
      ( 10, "Stone" )
    },
    "Stone"
  );
}

[Test]
public void TestTwoItems()
{
  // stone is 0 ~ 0.857, but the choice Value is 0.88f, so it should be in the range of Sand, which is
  // (0.857 ~ 1)
  TestChoice
  (
    0.88f,
    new List<(int weight, string itemName)>
    {
      ( 6, "Stone" ), // probability: 6/7 -> 0.857 <=> range: 85.7% -> 0 ~ 0.857
      ( 1, "Sand" ) // probability: 1/7 -> 0.143 <=> range: 14.3% -> 0.857 ~ (0.857 + 0.143) = 1
    },
    "Sand"
  );
}
```
Running the test to make sure it's still green bar.

Then here's the interesting part. There's still more to generalize / remove duplication in this case. But how do we do that?
If you still cannot see it, we can continue to write another test, it will be similar to the third case, but slightly altered.

```csharp
[Test]
public void TestTwoItems2()
{
  // stone is 0 ~ 0.857, in which the choice value is within, so it should choose stone
  TestChoice
  (
    0.1f,
    new List<(int weight, string itemName)>
    {
      ( 6, "Stone" ), // probability: 6/7 -> 0.857 <=> range: 85.7% -> 0 ~ 0.857
      ( 1, "Sand" ) // probability: 1/7 -> 0.143 <=> range: 14.3% -> 0.857 ~ (0.857 + 0.143) = 1
    },
    "Stone"
  );
}
```

so as you can see, I only changed the choice Value, 

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
