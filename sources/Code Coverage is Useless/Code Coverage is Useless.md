# Source

[[Article] Code Coverage is Useless](https://dev.to/johnpreese/code-coverage-is-useless-1h3h)

# Intro

Not too long ago there were talks around the office regarding a new  testing initiative. Now, by itself, this is fantastic news. Who wouldn't want to actually spend some time and get our testing story up to par?

The problem lies within the approach that was proposed, going so far as to say: **"We need to ensure that we have at least 80% test coverage."**

While the intention is a good one, code coverage is unfortunately useless.

Now, that is a pretty bold statement, so let me clarify a little bit. Code coverage **goals** are useless. You shouldn't strive for X% coverage on a given codebase. There are a few reasons for this, so let me explain.

# It is possible to test enough

Not all code bases are created equal. One could be for an application that sees millions of hits in a day and is grossly complicated. Another could be for a tiny application that services a couple users a day, if  that.

Now, it would be a silly little to say that every application should have at least 80% code coverage. Why? **Opportunity cost.** While I am a huge proponent of testing, I don't like to test just  because. We should aim to test enough. Test enough so that we have  enough confidence that our application will function as we expect it to.

*Note: I feel like at this point some individuals may be a little  confused as to how adding tests would be invaluable. There's a whole  development methodology called TDD that creates a high level of coverage just by following the red, green, refactor cycle.The points I make here generally refer to going back and adding tests because someone dictated that the code bases coverage percentage was too low. If you're doing  TDD to begin with, then setting a target really won't help. It's just a  byproduct.*

It's all about context. We can't generalize a percentage of coverage in our code base, because each code base is different.

Anyway...

In the same vein, not everything needs a test around it. Let's say we wanted to introduce a new public member into our codebase, something  simple

```c#
public FirstName { get; set; } 
```

Introducing this line of code, if not called in any of our tests will drop code coverage. Maybe even below our beloved 80%. The fix?

```c#
[Fact]
public void FirstName_ByDefault_CanBeSet()  
{
  var myClass = MyClass();
  myClass.FirstName = "testname";
  Assert.AreEqual("testname", myClass.FirstName)
}
```

At this point, we're just testing .NET -- something we definitely want  to avoid. I tend to only put tests around code that I know could  actually have the potential to change in a way that I do not want it to. Logical code.

# Code coverage is *easy*

Just because we have a lot of code coverage, does not necessarily  mean that we can have a lot of confidence that our application works as  we expect it to. Everything is always more clear with examples, so let's consider the following:

```c#
public class Flawless  
{
  public bool IsGuarenteedToWork()
  {
    // some code
  }
}
```

Now, methods usually have logic that we would normally want to test,  right? Conditionals, mathematical operations, you name it. Though, for  our example, it doesn't matter! We just want to increase code coverage.  That's our goal.

```c#
[Fact]
public void IsGuarenteedToWork_ByDefault_Works()  
{
  var flawless = new Flawless();

  var actual = flawless.IsGuarenteedToWork();
}
```

And there you have it! 100% code coverage. By default, tests that do not have an `Assert` will be considered passing. Now you're probably thinking.. oh come on, who would actually do this?

People do silly things when **incentivized.** My go-to  example is that of a scenario in which a company tells QA that for every bug they find at the end of the quarter, they will be given a bonus.  Seems pretty reasonable right? The flip side of that is the same company tells development that they will receive a bonus based on how few bugs  they introduce into the system.

This scenario incentivizes the failure of opposing groups. The  development organization doesn't really want to write any code for fear  of introducing a bug and wants QA to miss bugs in their analysis.  Whereas the QA group wants development to introduce bugs into the system so that they can find them and be rewarded for doing so.

The other thing that we need to keep in mind is that...

# Code coverage context matters

Let's consider that our developer wasn't just trying to game the  system, and actually put forth an honest effort to obtaining his code  coverage goal. Our implementation could be something like the following:

```c#
public class Flawless  
{
  public bool IsGuarenteedToWork()
  {
    for(var x = 0; x < int.MaxValue; x++) 
    {
      // Man, this is gonna work. I'll find that solution.. eventually.
    }
  }
}
```

.. and let's not forget the test.

```c#
[Fact]
public void IsGuarenteedToWork_ByDefault_Works()  
{
  var flawless = new Flawless();

  var actual = flawless.IsGuarenteedToWork();

  Assert.True(actual);
}
```

I hope it was obvious that the example above is far from performant.  But in this case, we've reached 100% code coverage and we're actually  asserting that the code is working as we intend it to. The  implementation works. The test is correct. Everyone is happy. Almost...

When it comes to testing, there are different stakeholders.

> Stakeholders are people whose lives you touch - Mark McNeil

This can be broken down further into the types of stakeholders.

1. Primary Stakeholder (who I'm doing it for) *Example: The customer who requested the feature.*
2. Secondary Stakeholder (others who are directly involved) *Example: Your boss and/or other developers on the project.*
3. Indirect Stakeholder (those who are impacted otherwise) *Example: The customers of your customer.*

As programmers, we are writing code to solve problems for other  people (sometimes ourselves if we can find the time). The same section  of code matters differently to different people. Person A only cares  that the answer is correct. Maybe they're notified when it's ready, but  they're pretty indifferent to when they receive it. Person B needs the  answer soon after requesting it. Our test only completely satisfies  Person A.

There can be a lot of stakeholders when it comes to writing code.  Unfortunately, we can't say with confidence, even at 100% code coverage, that our code is going to be compatible with everyone's needs.

After all of the harping on why code coverage is useless as a *target*. I need to wrap up by saying...

# Code coverage can actually be useful

I prefer to leverage code coverage as a **metric**. Coverage is something that we're aware of, something that we can use to make informed decisions about each codebase.

If we notice that one codebase is consistently dropping in coverage,  we can take that as a sign to look a little deeper into what's going on. Is the codebase incredibly hard to test? Are the developers just not  putting forth the effort to test, even when it makes sense? Maybe it's  actually what we would expect from that code base, so everything is  gravy.

Coverage can also just let us know if we're doing an adequate amount  of testing. If a mission-critical application only has 10% coverage, we  should investigate the reasons for that and potentially start a quality  initiative and gets some tests strapped on. It allows us to prioritize  our testing initiatives without just randomly picking a codebase and  start throwing tests at it.

The entire point of all of this is that setting coverage targets will just be counterproductive to your goals. We should be aware of coverage so that we can make informed decisions, but not let it impact the  quality of our code just for the sake of coverage attainment.

# References

-  [Test Coverage is a Lie](../Test Coverage is a Lie/Test Coverage is a Lie.md)