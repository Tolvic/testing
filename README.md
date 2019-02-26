# Testing
From the 25th February - 1st of March, BGL put on a learning and development week, where we could spend the week on our own personal learning and development. I decided on spend that week on learning testing.

## To Do
Please note that the links in this section link to material to be/ has been reviewed. The output of these tasks can be found in the other sections of this document.

- [ ] Testing in General
  - [x] Types of testing
    - [x] Unit testing
    - [x] Integration testing
    - [x] Functional testing
    - [x] End to End testing
- [ ] Unit testing and NUnit
 - [x] Review Microsoft documentation
 - [ ] Review NUnit Documentation
 - [ ] [Creating unit tests for ASP.Net applications - MS Docs](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs )
 - [ ] [Unit testing with NUnit - MS Docs](https://docs.microsoft.com/en-gb/dotnet/core/testing/unit-testing-with-nunit?view=aspnetcore-2.2)
 - [ ] [Introduction to .NET Testing with NUnit 3 - Plural Sight](https://www.pluralsight.com/courses/nunit-3-dotnet-testing-introduction)
 - [ ] [Running Selective Unit Tests - MS Docs](https://docs.microsoft.com/en-gb/dotnet/core/testing/selective-unit-tests?view=aspnetcore-2.2)
 - [ ] [Razor Page Unit Tests](https://docs.microsoft.com/en-gb/aspnet/core/test/razor-pages-tests?view=aspnetcore-2.2)
 - [ ] [Testing controller logic](https://docs.microsoft.com/en-gb/aspnet/core/mvc/controllers/testing?view=aspnetcore-2.2)
 - [ ] [Unit testing web APIs](https://docs.microsoft.com/en-us/aspnet/web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api)
 - [ ] [Unit testing controllers with Web APIs](https://docs.microsoft.com/en-us/aspnet/web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api)
  - [ ] [Integration Tests - MS Docs](https://docs.microsoft.com/en-gb/aspnet/core/test/integration-tests?view=aspnetcore-2.2)
 - [ ] Coverage metric
 - [ ] Setup and tear down
 - [ ] Helper classes and functions
 - [ ] Mocking and stubbing
 - [ ] Best practices
 - [ ] Speed and performance
 - [ ] Cheat sheet
- [ ] Mocking and stubbing with Moq
 - [ ] Review documentation
 - [ ] Mocking with Moq - Plural Site](https://www.pluralsight.com/courses/mocking-with-moq)
 - [ ] [Mocking Entity Framework](https://docs.microsoft.com/en-us/aspnet/web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api)
 - [ ] Best practices
 - [ ] Speed and performance
 - [ ] Cheat sheet
- [ ] Unit testing with Jasmine
 - [ ] Review documentation
 - [ ] [Testing JavaScript with Jasmine - Plural Sight ](https://www.pluralsight.com/courses/javascript-jasmine-typescript)
 - [ ] Coverage metric
 - [ ] Setup and tear down
 - [ ] Helper classes and functions
 - [ ] Mocking and stubbing - spy
 - [ ] Best practices
 - [ ] Speed and performance
 - [ ] Cheat sheet
- [ ] Function testing with Selenium
 - [ ] [Automated web testing with Selenium - Plural Sight](https://www.pluralsight.com/courses/selenium)
 - [ ] Review documentation
 - [ ] Identify areas of further research
 - [ ] Coverage metric
 - [ ] Setup and tear down
 - [ ] Helper classes and functions
 - [ ] Mocking and stubbing - spy
 - [ ] Best practices
 - [ ] Speed and performance
 - [ ] cheat sheet

## Testing Overview
* [Types of testing](/Notes/testing-types.md)

## NUnit
### Attributes
NUnit supports a wide range of attributes. A full list can be found [here](https://github.com/nunit/docs/wiki/Attributes). Some notable attributes include:
* Combinational Attribute
* Description Attribute
* Explicit Attribute
* Ignore Attribute
* MaxTime Attribute
* OneTimeSetUp Attribute
* OneTimeTearDown Attribute
* Random Attribute
* Range Attribute
* SetUpFixture Attribute
* TearDown Attribute
* Test Attribute
* TestCase Attribute
* TestFixture Attribute
* Timeout Attribute

## Asertions
If an assertion fails, the method call does not return and an error is reported. If a test contains multiple assertions, any assertion that follow the one that failed will not be executed. For this reason, it's usually best to try for one assertion per test.

In NUnit 3.0, assertions are written primarily using the `Assert.That` method, which takes constraint objects as an argument.

![NUnit Assertion Constraint Model](/Images/NUnit_Assertion_Constraint_Model)

### Constraints
A list of constraints can be found [here](https://github.com/nunit/docs/wiki/Constraints).

### Multiple Assertions
Typically, once an assertion fails, the test to terminate.
To continue the test and accumulate any additional failures, multiple asserts are implemented using the `Assert.Multiple` method:

```csharp
[Test]
public void ComplexNumberTest()
{
    ComplexNumber result = SomeCalculation();

    Assert.Multiple(() =>
    {
        Assert.AreEqual(5.2, result.RealPart, "Real part");
        Assert.AreEqual(3.9, result.ImaginaryPart, "Imaginary part");
    });
}
```

This results in NUnit storing any failures encountered in the block and reporting all of them together upon exit from the block.

Note:
* Multiple assert blocks may be nested. Failure is not reported until the outermost block exits.
* The test will be terminated immediately if any exception is thrown that is not handled. An unexpected exception is often an indication that the test itself is in error, so it must be terminated.

### ListMapper
`ListMapper` is used to modify the actual value argument to `Assert.That()`. It transforms the actual value, which must be a collection, creating a new collection to be tested against the supplied constraint.

Example:

```csharp
string[] strings = new string[] { "a", "ab", "abc" };
int[] lengths = new int[] { 1, 2, 3 };

Assert.That(List.Map(strings).Property("Length"), Is.EqualTo(lengths));
```

### Warnings
Sometimes - especially in integration testing - it's desirable to give a warning message but continue execution. Beginning with release 3.6, NUnit supports this with the Warn class and the `Assert.Warn` method.

```csharp
// Use Warn with reversed condition
Warn.If(2+2 != 5);
Warn.If(() => 2 + 2 != 5);
Warn.If(2+2, Is.Not.EqualTo(5));
Warn.If(() => 2+2, Is.Not.EqualTo(5).After(3000));

// Use Warn with original condition
Warn.Unless(2+2 == 5);
Warn.Unless(() => 2 + 2 == 5);
Warn.Unless(2+2, Is.EqualTo(5));
Warn.Unless(() => 2+2, Is.EqualTo(5).After(3000));

// Issue a warning message
Assert.Warn("Warning message");
```

Each of the above items would fail. The test would continue to execute, however, and the warning messages would only be reported at the end of the test.

## SetUp and TearDown
Multiple SetUp, OneTimeSetUp, TearDown and OneTimeTearDown methods may exist within a class.

Setup methods (both types) are called on base classes first, then on derived classes. If any setup method throws an exception, no further setups are called.

Teardown methods (again, both types) are called on derived classes first, then on the base class. The teardown methods at any level in the inheritance hierarchy will be called only if a setup method at the same level was called. The following example is illustrates the difference.

```csharp
public class BaseClass
{
   [SetUp]
   public void BaseSetUp() { ... } // Exception thrown!

   [TearDown]
   public void BaseTearDown() { ... }
}

[TestFixture]
public class DerivedClass : BaseClass
{
   [SetUp]
   public void DerivedSetUp() { ... }

   [TearDown]
   public void DerivedTearDown() { ... }

   [Test]
   public void TestMethod() { ... }
}
```
