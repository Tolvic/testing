# Unit tests
  * [What are Unit Tests?](#what-are-unit-tests-)
  * [Benefits](#benefits)
    + [Less Time Performing Functional Test](#less-time-performing-functional-test)
    + [Protection Against Regression](#protection-against-regression)
    + [Executable Documentation](#executable-documentation)
    + [Less Coupled Code](#less-coupled-code)
  * [Characteristics of Good Unit tests](#characteristics-of-good-unit-tests)
  * [Terminology](#terminology)
  * [Best practices](#best-practices)
    + [Naming tests](#naming-tests)
    + [Arranging Tests - AAA](#arranging-tests---aaa)
    + [Write Minimal Passing tests](#write-minimal-passing-tests)
    + [Avoid Magic Strings](#avoid-magic-strings)
    + [Avoid Logic in Tests](#avoid-logic-in-tests)
    + [Prefer Helper Methods to Setup and Teardown](#prefer-helper-methods-to-setup-and-teardown)
    + [Avoid Multiple asserts](#avoid-multiple-asserts)
    + [Validate Private Methods by Testing Public Methods](#validate-private-methods-by-testing-public-methods)
    + [Stub Static References](#stub-static-references)

## What are Unit Tests?
Unit testing is where individual units/ components of a piece of software are tested.

The purpose of unit testing is to validate that each unit performs as designed.

A unit is the smallest testable part of any software. In object-oriented programming, the smallest unit is a method.

Tests should be written in isolation. Interactions with other methods, classes, databases should be mocked.

Unit testing frameworks include [NUnit](/NUnit.md) and Jasmine.

## Benefits

### Less Time Performing Functional Test
Functional tests are expensive to write and run. Unit tests, take milliseconds, can be run at the press of a button and do not necessarily require any knowledge of the system at large.

### Protection Against Regression
With unit testing, it's possible to rerun your entire suite of tests after you change a line of code. Giving you confidence that your new code does not break existing functionality.

### Executable Documentation
When you have a suite of well-named unit tests, each test should be able to clearly explain the expected output for a given input. In addition, it should be able to verify that it actually works.

### Less Coupled Code
Writing tests for your code will naturally decouple your code, because it would be more difficult to test otherwise.

## Characteristics of Good Unit tests
* **Fast** -  It is not uncommon for mature projects to have thousands of unit tests. Unit tests should take very little time to run. Milliseconds.
* **Isolated** - Unit tests are standalone, can be run in isolation, and have no dependencies on any outside factors such as a file system or database.
* **Repeatable** - Running a unit test should be consistent with its results, that is, it always returns the same result if you do not change anything in between runs.
* **Self-Checking** - The test should be able to automatically detect if it passed or failed without any human interaction.
* **Timely** - A unit test should not take a disproportionally long time to write compared to the code being tested. If you find testing the code taking a large amount of time compared to writing the code, consider a design that is more testable.

## Terminology
**Fake** - A fake is a generic term which can be used to describe either a stub or a mock object. Whether it is a stub or a mock depends on the context in which it's used. So in other words, a fake can be a stub or a mock.

**Mock** - A mock object is a fake object in the system that decides whether or not a unit test has passed or failed. A mock starts out as a Fake until it is asserted against.

**Stub** - A stub is a controllable replacement for an existing dependency (or collaborator) in the system. By using a stub, you can test your code without dealing with the dependency directly. By default, a fake starts out as a stub.

Stubs provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test. Stubs may also record information about calls, such as an email gateway stub that remembers the messages it 'sent', or maybe only how many messages it 'sent'.

## Best practices
### Naming tests
The name of your test should consist of three parts:
1. The name of the method being tested.
2. The scenario under which it's being tested.
3. The expected behavior when the scenario is invoked.

Naming standards are important because they explicitly express the intent of the test.

Tests are more than just making sure your code works, they also provide documentation. Just by looking at the suite of unit tests, you should be able to infer the behavior of the code without even looking at the code itself.

Additionally, when tests fail, you can see exactly which scenarios do not meet your expectations.

### Arranging Tests - AAA
Arrange, Act, Assert is a common pattern when unit testing. As the name implies, it consists of three main actions:
1. Arrange your objects, creating and setting them up as necessary.
2. Act on an object.
3. Assert that something is as expected.

This pattern:
* Clearly separates what is being tested from the arrange and assert steps.
* Has less chance to intermix assertions with "Act" code.

Readability is one of the most important aspects when writing a test. Separating each of these actions within the test clearly highlight the dependencies required to call your code, how your code is being called, and what you are trying to assert. While it may be possible to combine some steps and reduce the size of your test, the primary goal is to make the test as readable as possible.

### Write Minimal Passing tests
The input to be used in a unit test should be the simplest possible in order to verify the behavior that you are currently testing.

This makes tests more resilient to future changes in the codebase and means that tests are closer to testing behavior over implementation.

Tests that include more information than required to pass the test have a higher chance of introducing errors into the test and can make the intent of the test less clear.

When writing tests you want to focus on the behavior. Setting extra properties on models or using non-zero values when not required, only detracts from what you are trying to prove.

### Avoid Magic Strings
Magic strings are string values that are specified directly within application code that have an impact on the application’s behavior.

Unit tests should not contain magic strings.

This Prevents the need for the reader of the test to inspect the production code in order to figure out what makes the value special.

Magic strings can cause confusion to the reader of your tests.

When writing tests, you should aim to express as much intent as possible. In the case of magic strings, a good approach is to assign these values to constants.

### Avoid Logic in Tests
When writing your unit tests avoid manual string concatenation and logical conditions such as `if`, `while`, `for`, `switch`, etc.

When you introduce logic into your test suite, the chance of introducing a bug into it increases dramatically.

When a test fails, you should be confident that it is because there is something actually wrong with your code and not because of the logic in your test.

If logic in your test seems unavoidable, consider splitting the test up into two or more different tests.

### Prefer Helper Methods to Setup and Teardown
If you require a similar object or state for your tests, prefer a helper method than leveraging Setup and Teardown attributes if they exist.

This results in:
* Less confusion when reading the tests since all of the code is visible from within each test.
* Less chance of setting up too much or too little for the given test.
* Less chance of sharing state between tests which creates unwanted dependencies between them.

In unit testing frameworks, Setup generally leads bloated and hard to read tests. Each test will generally have different requirements in order to get the test up and running. Unfortunately, Setup forces you to use the exact same requirements for each test.

### Avoid Multiple asserts
Try to only include one Assert per test. Either:
* Create a separate test for each assert.
* Use parameterized tests.

This Ensures you are not asserting multiple cases in your tests. If one Assert fails, the subsequent Asserts will not be evaluated and it gives you the entire picture as to why your tests are failing.

A common exception to this rule is when asserting against an object. In this case, it is generally acceptable to have multiple asserts against each property to ensure the object is in the state that you expect it to be in.

### Validate Private Methods by Testing Public Methods
Private methods never exist in isolation. At some point, there is going to be a public facing method that calls the private method as part of its implementation. What you should care about is the end result of the public method that calls into the private one.

### Stub Static References
One of the principles of a unit test is that it must have full control of the system under test. This can be problematic when production code includes calls to static references (e.g. `DateTime.Now`).

In such cases, you'll need to introduce a seam into your production code. One approach is to wrap the code that you need to control in an interface and have the production code depend on that interface.

This will mean that the test suite has full control over `DateTime.Now` and can stub any value when calling into the method.

---

**Source:** https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices
