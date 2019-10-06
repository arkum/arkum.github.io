---
layout: post
title: "Test Driven Development for a CRM Plug-in"
subtitle: "Test Driven Development for a CRM Plug-in"
date: '2017-06-04 12:08:00 +1000'
background: 'https://source.unsplash.com/random'
categories:
- Technology
tags:
- TDD
- CRM
---

# Test Driven Development for a CRM Plug-in


It is just testable, isn't? So what? This has been one of the most common arguments I have heard from people who don't understand testable designs. The fact of the matter is, unit tests are your design, and they are executable too. You are writing your tests first not just to write unit tests; instead, unit tests help you to think about the design of the problem you are trying to solve, the code you are going to write, and verify their correctness, too.

## Red-Green-Refactor

In TDD, you start writing unit tests before writing a single line of your implementation code (of your plug-in). This test will fail and be marked as red. Next step is to implement the code to make this test pass or make it green. You continue to keep writing tests which fail (red) and refactor or implement your code to make these tests green. Finally, when you feel that you have completed the implementation and have enough code coverage, you are done.

The plug-in in question here is for pre-create of a contact entity which validates the birthday of a contact. For this sample, the plugin will prevent the creation of a contact with future birthdays.

I will be using xUnit as the testing framework. You may use any unit testing framework. Installing xUnit is dead easy. Install it from NuGet using package manager in Visual Studio. Install xunit.runners.visualstudio package to help Visual Studio discover your unit tests.

## First Failing Test

Below test checks whether the plugin fires for only the event it is registered for. Please note the FakeServiceProvider class of which instance is passed to the execute method.

![image](/uploads/2017/04/crmtdd1.png){: .img-fluid}

As expected, this test will fail.

![image](/uploads/2017/04/crmtdd2.png){: .img-fluid}

## Make the Test Green

Let us implement the plugin code and the FakeServiceProvider implementation to make the above test pass.

![image](/uploads/2017/04/crmtdd3.png){: .img-fluid}

Still, our test is failing. We haven't implemented FakeServiceProvider yet. From the above code, it is obvious that we should implement FakePluginExecutionContext as well. Implement IPluginExecutionContext and return PreCreate from the MessageName getter property.

![image](/uploads/2017/04/crmtdd4.png){: .img-fluid}

Return the new instance of FakePluginExecutionContext from GetService method of FakeServiceProvider

Hooray!!! Our test is passing.

![image](/uploads/2017/04/crmtdd5.png){: .img-fluid}

## Time to Refactor

Let us see whether we get the exception if we execute the plugin for a message for which it is not registered for.

![image](/uploads/2017/04/crmtdd6.png){: .img-fluid}

For the above test to pass, I had to refactor the FakeServiceProvider and FakePluginExecutionContext classes to accept parameters through their constructors. You can clearly see the power of composition here.

![image](/uploads/2017/04/crmtdd7.png){: .img-fluid}

![image](/uploads/2017/04/crmtdd8.png){: .img-fluid}

![image](/uploads/2017/04/crmtdd9.png){: .img-fluid}

Both our tests are passing. The second test captures the exception and compares the message.

![image](/uploads/2017/04/crmtdd10.png){: .img-fluid}

## Implementing Business Logic

Before we write unit tests for checking the business logic (contact's birth date should not be greater than today) we should write some infrastructure code and associated unit tests to check the existence of PreImage and the PreImage entity name. I am omitting those details for brevity, but you can check the source code for details 
[here][2].

Below are the two unit tests for checking the birthdate logic — one positive and one negative.

![image](/uploads/2017/04/crmtdd11.png){: .img-fluid}

Please note that the PreImage is passed as a parameter to the constructor of the FakePluginExecutionContext class. This way, we can influence the test results.

## More Refactoring

Our plugin code can be refactored to move some of the boilerplate code to a base class. Also, FakePluginExecutionContext and FakeServiceProvider classes can be made reusable and complete by implementing the missing methods. Currently, plugin and unit test code are in the same assembly. We can move the plugin code to its own assembly and add a reference in the unit test project.

## Some Gotchas

You might have already noticed that majority of our unit test methods do not have Asserts. For a TDD purist, this could be a big no no. But we have not much of a choice as Execute method returns void. What this means is, it is difficult to make some of the tests fail.

Anyhow, I hope you got some ideas about Test Driven Development especially in the context of writing a CRM plug-in. Hope you write many more unit tests for your plug-ins.

[1]: https://miro.medium.com/fit/c/96/96/1*dmbNkD5D-u45r44go_cf0g.png
[2]: https://github.com/arkum/CRMTDD
[3]: /uploads/2017/04/crmtdd1.png
[4]: [image](/uploads/2017/04/crmtdd2.png){: .img-fluid}
[5]: /uploads/2017/04/crmtdd3.png
[6]: /uploads/2017/04/crmtdd4.png
[7]: /uploads/2017/04/crmtdd5.png
[8]: /uploads/2017/04/crmtdd6.png
[9]: /uploads/2017/04/crmtdd7.png
[10]: /uploads/2017/04/crmtdd8.png
[11]: /uploads/2017/04/crmtdd9.png
[12]: /uploads/2017/04/crmtdd10.png
[13]: /uploads/2017/04/crmtdd11.png
