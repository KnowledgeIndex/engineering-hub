# Source

[[Article] [Kent C. Dodds] Write tests. Not too many. Mostly integration.](https://kentcdodds.com/blog/write-tests)

# Not too many

I've heard managers and teams mandating 100% code coverage for applications. That's a really bad idea. The problem is that you get diminishing returns on your tests as the coverage increases much beyond 70% (I made that number up... no science there). Why is that? Well, when you strive for 100% all the time, you find yourself spending time testing things that really don't need to be tested. Things that really have no logic in them at all (so any bugs could be caught by ESLint and Flow). Maintaining tests like this actually really slow you and your team down.

You may also find yourself testing implementation details just so you can make sure you get that one line of code that's hard to reproduce in a test environment. You really want to avoid testing implementation details because it doesn't give you very much confidence that your application is working and it slows you down when refactoring. You should very rarely have to change tests when you refactor code.

# Mostly integration

One thing that it doesn't show on the testing pyramid is that **as you move up the pyramid, the confidence quotient of each form of testing increases.** You get more bang for your buck. So while E2E tests may be slower and more expensive than unit tests, they bring you much more confidence that your application is working as intended.

# How to write more integration tests

The line between integration and unit tests is a little bit fuzzy. Regardless, I think the biggest thing you can do to write more integration tests is to stop mocking so much stuff. When you mock something you’re removing all confidence in the integration between what you’re testing and what’s being mocked. I understand that sometimes it can’t be helped (though some would disagree). You don’t actually want to send emails or charge credit cards every test, but most of the time you can avoid mocking and you’ll be better for it.

If you’re doing React, then this includes shallow rendering. For more on this, read Why I Never Use Shallow Rendering.

# References

- [Common Testing Mistakes.md](../Common Testing Mistakes/Common Testing Mistakes.md)
- [Static vs Unit vs Integration vs E2E Testing for Frontend Apps.md](../Static vs Unit vs Integration vs E2E Testing for Frontend Apps/Static vs Unit vs Integration vs E2E Testing for Frontend Apps.md) 
- [How to add testing to an existing project.md](../How to add testing to an existing project/How to add testing to an existing project.md) 
- [Eliminate an entire category of bugs with a few simple tools.md](../Eliminate an entire category of bugs with a few simple tools/Eliminate an entire category of bugs with a few simple tools.md)