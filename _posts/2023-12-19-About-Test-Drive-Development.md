Long time no see!

It has been a while since I last wrote a post, I was working more on my own life, and now I feel better than ever~

# TDD, Extreme and Agile

As far as I have gotten in programming and designing systems. I always have been not quite happy with my performance, Sometimes to the point where it's sometimes demotivating.
And that is also why I am recently quite interested in TDD (Test Driven Development), [Extreme Programming](https://en.wikipedia.org/wiki/Extreme_programming) and [Agile Development](https://en.wikipedia.org/wiki/Agile_software_development).

There are several books I've read while trying it myself, a book to note is [Test-Driven Development By Example - Kent Beck](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530), it may be hard to read at first (believe me, temporary programming sins all over the place), but it does give some practical examples on how to do TDD by literally solving a complex example throughout the book.

My goal in this post is hoping to clear up peoples' understanding on what TDD is and what TDD IS NOT after trying it for almost half a year. (because people do seemingly misunderstood / misuse TDD)

# What is it?

In short for you folks who do not know about what TDD is:
> Test Drive development is a software development process which relies on converting requirements into test cases that can be potentially fully tested.

before we get into what exactly you do, in TDD and why, we should make very clear points on common misconceptions, pros and cons about TDD.

# Common Misconceptions, Pros & Cons

1. Thinking that TDD can replace design (no it cannot, it can only help influence you to design towards end users)
    - this is quite a bad misunderstanding on what TDD can actually do; Yes, it does help you with design, probably change your way on thinking your designing choices, BUT, that doesn't mean you don't need to think about design in the first place.
    - It is a great tool to help and influences your design, to help you think in the perspective of users of your system / code, writing what is only neccessary (which is [YAGNI - extreme programming principle](https://en.wikipedia.org/wiki/You_aren't_gonna_need_it))

2. we MUST follow TDD principles at all times
    - no, that, in my honest opinion, is just not worth it, if you are working on a very tiny method / small algorithm that is probably not critical, the cost of writing the unit test, time to running the test takes longer than just writing them in one go.
    - Principles are just Principles, they are meant to help you design, not to give burden, only use them when you know they can really help you reduce your work a ton / really shine.
  
3. We MUST TDD in all cases! (or in other words: to have near 100% test coverage!)
    - same idea as what I have said above in 2, it is just not worth your time and effort trying to write a unit test for just a few lines of code that may not be critical in your software
  
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

The heart and core of TDD is to do things **quickly**, and in **small enough increments**, and at the same time, making sure the code quality is maintained, and also making sure that everything is under control (of course, there are still situations where control is hard or near impossible to have)

Let's have a few real world examples on how this could be potentially applied (there also would be counter examples, so fear not)

I will be using C# for the following examples, but it should apply to other object-oriented languages as well! (maybe also functional languages perhaps?)

For the Test Framework, I will be using NUnit

# TDD - Looting System

Given the following requirements:
> Write a `Looting System` in which has a single procedure: `RandChoose` that, upon given a loot table with weights and item names, will return a random loot in the table, given the weight.

> The higher the weight is, the higher the probability that it will be chosen among all loot in the loot table.

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
In other words, think of how to use `RandChoose` in the most simplest way: An entirely empty table as an input can be a good start.

### First Test
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

Currently, we do not have the class `LootSystem`, the static method `RandChoose`,
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

Now When we run the test, it will fail. After seeing it fail, we will then make it succeed in the most simplest way!

```csharp
public static string RandChoose( List<(int weight, string itemName)> lootTable )
{
  return string.Empty;
}
```

We simply just return an empty string! Bam, baby and small incremental steps.

After making sure that the test pass, we can now continue to the most important part: Removing duplication

Alright, you ask: "where is the duplication?", "we only have a single string.Empty..."

Wait wait wait, now, hold there, look at where string.Empty occured... Yes~ It occurred both in the Test and in the implementation!

So to remove such duplication, what we usually do is to "generalize"(there's whole another topic around this, I will discuss it in another post). We can start thinking where does this `string.Empty` comes from?

It is an exceptional case! So right now, without other tests to help, we can't really generalize up, So let's not remove this duplication.. just yet :D

### Second Test
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

We then move onto the next easiest case, which is testing one single loot item, and we expect "Stone" being always the result.

Running the test, Expecting it fail.

Then let's move onto trying to do the easiest way to fix it! And by easiest... I meant changing it into if case!

```csharp
public static string RandChoose( List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  return "Stone";
}
```

The test should succeed, and let's check for duplication.

Aha! "Stone" is a duplication of the "Stone" in the test! How would one generalize a constant?
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

### Third Test
Moving onto the third test. This test will change a lot of things later on.

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

As you can see writing the third test isn't that easy, a problem arises. We cannot predict the output of a randomization!

So that **must** mean our design has some flaws, rendering it to be not testable. So, in order to continue, we should fix this immediately.

Our problem here is the test (user side) cannot determine the output of `RandChoose`, but we also do not want to expose the contents of **LootSystem**.

So, either we can inject a `Random` class into `LootSystem`, then mock that `Random` class to deterministic, or we could redesign the request to allow the input to also predict what the Choosing Method can handle!

Second one is much easier, so let's do that!

So let's allow `RandChoose` to receive another argument, that argument can be used to determine the output. That also means we have given the randomization work to the user!

That makes the system slightly more modular, as now the user can have customization on how to handle the determining factor of randomization, instead of the `LootSystem` handling it.

So before we continue, we know that our design is flawed, we should comment out the third test, because we cannot simply make it compile easily in the first step.

(Note: this may not work in some situations, where you can't really change the request, but rather only work around it. Here I am modifying the request)

Then we add a new argument that is used to determine the output of the choosing algorithm

```csharp
public static string RandChoose( float choiceValue, List<(int weight, string itemName)> lootTable )
```

And since, RandChoose isn't really `Random` anymore, we can safely remove the `Rand` prefix in the name of the method

```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
```

After that, our older tests are shouting to be fixed, so let's fix them by giving dummy values.

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

After ensuring that the tests still are successful, we can continue by uncommenting the third test, and write down the functionality we wanted.

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
You can see above in the comments of the code, we are selecting objects based on weight ranges.

Running the test and expecting a failure, let's continue by adding an if statement

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

Upon checking the tests still pass... Now onto removing duplication. Here you can see that "Sand" is an obvious duplication between implementation and the test, we can change it to be more generalized by using `lootTable[1].itemName`.

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

### Alternative Third Test

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

So as you can see, I only changed the choice Value, and after running it, definitely it will get an error.

To solve this... We yet give another if statement! This time, we check whether or not the choice Value is within the range of the first loot's weight.

```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  if ( lootTable.Count == 1 )
    return lootTable[ 0 ].itemName;

  if ( choiceValue <= 6f / 7f )
    return lootTable[ 0 ].itemName;

  return lootTable[ 1 ].itemName;
}
```

So now tests should run again, let's start cleaning up the code. First we can see 6f / 7f is a duplication between tests and the implementation.

```csharp
( 6, "Stone" ), // probability: 6/7 -> 0.857 <=> range: 85.7% -> 0 ~ 0.857
( 1, "Sand" ) // probability: 1/7 -> 0.143 <=> range: 14.3% -> 0.857 ~ (0.857 + 0.143) = 1
```

In our implementation, the 6f represents the 6 in the Stone's weight, so it could be represented as `lootTable[0].weight`, for 7f, it just means all the weights summed together, which is 6 + 1 = `lootTable[0].weight + lootTable[1].weight`

so in short `lootTable[0].weight / (lootTable[0].weight + lootTable[1].weight)`, do remember we should cast one side of the division as float, since dividing integers in C# returns a floored value.


```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  if ( lootTable.Count == 1 )
    return lootTable[ 0 ].itemName;

  if ( choiceValue <= ( float )lootTable[ 0 ].weight / ( lootTable[ 0 ].weight + lootTable[ 1 ].weight ) )
    return lootTable[ 0 ].itemName;

  return lootTable[ 1 ].itemName;
}
```

After running all the tests written so far and getting success, we can then refactor this much further, since we know we are always summing the entire loot table's weight.

```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  if ( lootTable.Count == 1 )
    return lootTable[ 0 ].itemName;

  int totalWeight = 0;
  
  foreach ( (int weight, string itemName) loot in lootTable )
    totalWeight += loot.weight;

  if ( choiceValue <= ( float )lootTable[ 0 ].weight / totalWeight )
    return lootTable[ 0 ].itemName;

  return lootTable[ 1 ].itemName;
}
```

The usual, Run test, still success. Now you can see that the line where we check `lootTable.Count == 1`, can be converted into the same logic below where the choiceValue is in the range of 0 ~ 1 (since we only have one single loot in the lootTable)

```csharp
if ( choiceValue <= ( float )lootTable[ 0 ].weight / totalWeight ) // ( lootTable.Count == 1 )
  return lootTable[ 0 ].itemName;
```

And you can see... It is simply the same logic as below! So it's counted as a duplication, so let's remove that.

```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  int totalWeight = 0;

  foreach ( (int weight, string itemName) loot in lootTable )
    totalWeight += loot.weight;

  if ( choiceValue <= ( float )lootTable[ 0 ].weight / totalWeight )
    return lootTable[ 0 ].itemName;

  return lootTable[ 1 ].itemName;
}
```

Run Test, Succeed.

### Fourth and Last Test
Now let's move on to the fourth test, this time, let's have 3 items to choose from.
```csharp
[Test]
public void TestThreeItems()
{
  TestChoice
  (
    0.5f,
    new List<(int weight, string itemName)>
    {
      ( 10, "Circle" ), // probability: 10/35 -> 0.286 <=> range: 0 ~ 0.286
      ( 20, "Triangle" ), // probability: 20/35 -> 0.571 <=> range: 0.286 ~ (0.286 + 0.571) = 0.857
      ( 5, "Box" ) // probability: 5/35 -> 0.143 <=> range: 0.857 ~ (0.857 + 0.143) = 1
    },
    "Triangle"
  );
}
```

As usual, let's run the test. But wait... We should expect failure isn't it? But checking the test, there's nothing wrong.
In this case, our implementation is able to support until 2 elements in the list, so it can select the Triangle.

But when we do TDD, each step is at least doing incremental progress on the implementation, so instead, we can make it select the third element in the list, and that will drive our implementation a bit further.

```csharp
[Test]
public void TestThreeItems()
{
  TestChoice
  (
    0.9f,
    new List<(int weight, string itemName)>
    {
      ( 10, "Circle" ), // probability: 10/35 -> 0.286 <=> range: 0 ~ 0.286
      ( 20, "Triangle" ), // probability: 20/35 -> 0.571 <=> range: 0.286 ~ (0.286 + 0.571) = 0.857
      ( 5, "Box" ) // probability: 5/35 -> 0.143 <=> range: 0.857 ~ (0.857 + 0.143) = 1
    },
    "Box"
  );
}
```

Running the test should fail now.

Let's get it run by our usual tactic, happy little if statements~ (Thanks Bob Ross!)

```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  int totalWeight = 0;

  foreach ( (int weight, string itemName) loot in lootTable )
    totalWeight += loot.weight;

  if ( choiceValue <= ( float )lootTable[ 0 ].weight / totalWeight )
    return lootTable[ 0 ].itemName;

  // Checks from the range 30/35 ~ 35/35 for the second item
  if ( choiceValue <= ( float )( 10 + 20 ) / totalWeight )
    return lootTable[ 1 ].itemName;

  return lootTable[ 2 ].itemName;
}
```

Run and getting tests green, we can then proceed to replace the constants into variables again.

Replacing (10 + 20) with the `lootTable[0].weight` and `lootTable[1].weight`

```csharp
// Checks from the range 30/35 ~ 35/35
if ( choiceValue <= ( float )( lootTable[ 0 ].weight + lootTable[ 1 ].weight ) / totalWeight )
  return lootTable[ 1 ].itemName;
```

Then if we observe more closely, especially these following lines:

```csharp
if ( choiceValue <= ( float )lootTable[ 0 ].weight / totalWeight )
  return lootTable[ 0 ].itemName;

// Checks from the range 30/35 ~ 35/35
if ( choiceValue <= ( float )( lootTable[ 0 ].weight + lootTable[ 1 ].weight ) / totalWeight )
  return lootTable[ 1 ].itemName;
```

We can see a common pattern here, where we search through all the weights, and compare whether or not it is in the given range.

```csharp
int currentMaxWeight = lootTable[ 0 ].weight;
if ( choiceValue <= ( float )currentMaxWeight / totalWeight )
  return lootTable[ 0 ].itemName;

// Checks from the range 30/35 ~ 35/35
currentMaxWeight += lootTable[ 1 ].weight;
if ( choiceValue <= ( float )currentMaxWeight / totalWeight )
  return lootTable[ 1 ].itemName;
```

When we extracted this logic out, we can obviously see the if statements below are very similar, the difference is just merely index, so let's try to extract that as well.
(Do note here I will be running tests and ensure I am still in the right track)

```csharp
int currentMaxWeight = lootTable[ 0 ].weight;
int index = 0;
if ( choiceValue <= ( float )currentMaxWeight / totalWeight )
  return lootTable[ index ].itemName;

// Checks from the range 30/35 ~ 35/35
currentMaxWeight += lootTable[ 1 ].weight;
index += 1;
if ( choiceValue <= ( float )currentMaxWeight / totalWeight )
  return lootTable[ index ].itemName;
```

And easily, we can see there's a duplication in the if statements, and there's an incrementing index, so turn that into a for loop!

```csharp
int currentMaxWeight = 0;

for ( int index = 0; index < lootTable.Count; index++ )
{
  currentMaxWeight += lootTable[ index ].weight;

  if ( choiceValue <= ( float )currentMaxWeight / totalWeight )
    return lootTable[ index ].itemName;
}
```

And since, in the for loop, we are only accessing the current element, isn't that just a foreach loop? let's change that as well!

```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  int totalWeight = 0;

  foreach ( (int weight, string itemName) loot in lootTable )
    totalWeight += loot.weight;

  int currentMaxWeight = 0;

  foreach ( (int weight, string itemName) loot in lootTable )
  {
    currentMaxWeight += loot.weight;

    if ( choiceValue <= ( float )currentMaxWeight / totalWeight )
      return loot.itemName;
  }

  return lootTable[ 2 ].itemName;
}
```

finally, did you realize, this is already the actual algorithm? except, we should remove the last return value and tell it to react to an error, since it is not expected to be out of the foreach loop. In C#, we would throw an `ArgumentException`


```csharp
public static string Choose( float choiceValue, List<(int weight, string itemName)> lootTable )
{
  if ( lootTable.Count == 0 )
    return string.Empty;

  int totalWeight = 0;

  foreach ( (int weight, string itemName) loot in lootTable )
    totalWeight += loot.weight;

  int currentMaxWeight = 0;

  foreach ( (int weight, string itemName) loot in lootTable )
  {
    currentMaxWeight += loot.weight;

    if ( choiceValue <= ( float )currentMaxWeight / totalWeight )
      return loot.itemName;
  }

  throw new ArgumentException();
}
```

And Bam! Run the tests one last time, and there we have it. A `LootSystem` that chooses a single item from the loot Table based on the loot's weights!

Of course, we can end this by extracting `(int weight, string itemName)` into it's own stucture / class, but I won't be doing that here.

After all that, I hope you can see how TDD can help design, and sometimes help you derive our a better way on writing an algorithm / system!

Even though it seems that TDD is really smooth along the way, but that isn't always the case, I won't go into detail in this blog since it's already very long.

So I will just show some Pros and Cons I have discovered while using TDD.



Have fun and stay tuned for the next blog, Cheers!
