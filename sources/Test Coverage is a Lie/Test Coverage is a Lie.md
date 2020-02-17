# Source

[[Article] Test Coverage is a Lie](https://dev.to/smizell/test-coverage-is-a-lie-3i84)

# Intro

I have some bad news about test coverage—it doesn’t tell us how much of  our code is covered by tests, even though the name says it does. It  doesn’t help us improve the quality of our software. It doesn’t help us  find bugs. It feels like it’s a lie. And maybe it is.

# What is Test Coverage?

[Wikipedia](https://en.wikipedia.org/wiki/Code_coverage) has an underwhelming—though in my opinion correct—definition of test coverage.

> In computer science, test coverage is a measure used to describe the  degree to which the source code of a program is executed when a  particular test suite runs.

As its defined here, test coverage does not tell us which lines in  the code have been tested. It does not tell us whether all possible  conditions have been tested. It only tells which lines of code were  executed during a test run.

The definition makes no mention of quality, though it does go on to talk about bugs.

> A program with high test coverage, measured as a percentage, has had  more of its source code executed during testing, which suggests it has a lower chance of containing undetected software bugs compared to a  program with low test coverage.

This idea presented is that more coverage correlates with fewer bugs.  Correct or not, this is not the same as using coverage as a tool to find existing bugs or preventing bugs from making it to production. It only  tells us there is a lower chance of bugs because of the test coverage.

# The Reason Test Coverage Lies

Let’s look at a short example to see how test coverage gives us a false  sense of how well-tested our code is. Consider the code below. It has  four branches to test.

```javascript
// This function always returns 12
// ...or does it?
exports.alwaysTwelve = function(a, b) {
  let y, z;

  if (a === true) y = 4;
  else y = 6;

  if (b === true) z = 3;
  else z = 2;

  return y * z;
}
```

And here are some tests for it.

```javascript
const assert = require('assert');
const { alwaysTwelve } = require('.');

describe('Always Twelve', function () {
  context('when true', function () {
    it('returns 12', function () {
      const value = alwaysTwelve(true, true);
      assert.equal(value, 12);
    });
  });

  context('when false', function () {
    it('returns 12', function () {
      const value = alwaysTwelve(false, false);
      assert.equal(value, 12);
    });
  });
});
```

The code and tests above result in 100% test coverage because the two tests are enough to cover the four branches in the code. However, they  are not enough to cover all possible paths through the application code. They don’t help us catch an obvious bug in the logic of our code.

We can calculate the number of tests we need to execute all the  possible paths through the code. The possible paths is the result of  taking 2n where `n` is the number of conditions in the code. Therefore, in our code above with two conditions, we need  four tests, not two. As we have it, our tests only test the inputs:

- `true` and `true` (resulting in 12)
- `false` and `false` (resulting in 12)

However, they should also test the inputs:

- `true` and `false` (resulting in 8)
- `false` and `true` (resulting in 18)

If we had tested these conditions, we would have found our tests did  not completely test our logic, resulting in a bug. Sadly, with passing  tests and 100% coverage, we may have shipped this code and had to find  these bugs later in production. That’s no fun.

I've created [a repository showing this example](https://github.com/smizell/test-coverage) which you can checkout and try. You'll see the tests passing with 100% coverage.

# Code Executed Isn’t Code Covered

If test coverage only tells us what code is executed, it means we  could execute code and never test it. Let’s convert our test suite from  above to show this scenario.

```javascript
const assert = require('assert');
const { alwaysTwelve } = require('.');

describe('Always Twelve', function () {
  context('when called', function () {
    it('returns 12', function () {
      // Called twice, though nothing is asserted
      alwaysTwelve(true, true);
      alwaysTwelve(false, false);
    });
  });
});
```

This bad test will pass and result in 100% test coverage. Not only does  test coverage say “100%” when we may need to test more, it shows us  “100%” when we thought we tested everything but didn’t.

# What This Tells Us About Test Coverage

There are some unfortunate conclusions we can take from these examples.

1. **Executed and tested are two different concepts**. While we may know what code executed through test coverage, it doesn’t mean those lines are properly tested or tested at all.
2. **The percentage is a best case scenario**. If the test coverage says 80%, that tells us between 0-80% of the code is properly tested.
3. **Once a line of code is marked as covered, it ceases to be a helpful designation**. As shown above, unless we know why a line of code is considered covered, we can’t say it’s been properly tested.
4. **The number of lines unexecuted is the only useful metric**. This measurement can tell us what code needs tests or what areas of  code are weak on testing. The percentage of test covered tells us little more than which code was executed, which has some value but not much.  It's only useful in telling us what code wasn't executed.

Test coverage doesn’t help us improve code quality, explore the  design of our code, discover bugs, prevent bugs, or test the logic of  the code. It can only tell us that our code executed without errors with the inputs we gave it. That’s it really. That’s all we get from test  coverage. Maybe that’s worth something.

# A Better Way

Good testing starts with good software architecture. Gary Bernhardt has a great talk called [Boundaries](https://www.destroyallsoftware.com/talks/boundaries) where he discusses a model for thinking about and designing software  that can lead to helpful testing practices. The video does a much better job than I can at explaining his thoughts. But there are some things  I’d like to pull from it to talk about testing.

Gary has two categories of code. One category contains all of the  paths and conditionals but has no dependencies. This category of code  includes business logic and policies but doesn’t access the outside  world like databases, the internet, or disks. It lends itself well to  isolated unit testing, and it allows developers to test all possible  paths quickly (because 2n tests adds up). He calls the category the functional core.

The other category of code contains almost no paths yet defines all  of the dependencies. This category lends itself well to integration  testing since integration tests wire together databases, internet, and  disks and ensure they work. He calls this category the imperative shell.

With these categories in mind, we can look at code and look at  whether there are outside dependencies and whether all paths are  well-tested. We can write many fast unit tests since the paths are  isolated from the outside world and a few integration tests to ensure  the world is wired together correctly.

Therefore, code reviewers can move from relying on test coverage to thinking about paths and dependencies.

1. Have we introduced a dependency where it shouldn’t be?
2. Have we added paths in our code where they shouldn’t be?
3. Are we testing all of the conditionals in the code?
4. Are we writing small functions so we can reduce our testing surface area and create solid boundaries?

I’d also like to point to another video that I think gives many good thoughts on testing well called [Please Don’t Mock Me](http://blog.testdouble.com/posts/2018-03-06-please-dont-mock-me). It is a good addition to Gary Bernhardt’s Boundaries video and explores poor ways to use tests and one good way to use them.

# How Should We Use Test Coverage?

There is some usefulness to this measurement when it gives feedback  on unexecuted code during the development process. It can show TDD  practitioners where they are getting ahead of themselves. It can show  developers where they forgot to test before they open a pull request. It can show open source maintainers areas missing tests by a contribution. The measurement for the project may not say much as a whole, but during the changes to the code, the unexecuted value can give some insights.

And that is the important takeaway—the value is in knowing what code  went unexecuted during our test run. It's better to think of the  measurement as "10% of my code was unexecuted during my tests" rather  than "90% of my code is covered by tests" (which as we've seen may not  be true). Or better yet, "I've added 15 lines of code that aren't  executed during my tests." This to me is the more helpful—and more  honest—approach, and it helps us change code with some added confidence.

# References

-  [Code Coverage is Useless](../Code Coverage is Useless/Code Coverage is Useless.md) 