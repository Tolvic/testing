# NUnit

* [Creating a Test Project](#creating-a-test-project)
* [Writing Unit Tests with NUnit](#writing-unit-tests-with-nunit)
  + [Assertions](#assertions)
    - [Multiple Assertions](#multiple-assertions)
    - [ListMapper](#listmapper)
    - [Warnings](#warnings)
  + [Constraints](#constraints)
    - [Comparison Constraints](#comparison-constraints)
    - [String Constraints](#string-constraints)
    - [Collection Constraints](#collection-constraints)
    - [Conditional Constraints](#conditional-constraints)
    - [Compound Constraints](#compound-constraints)
    - [Directory / File Constraints:](#directory---file-constraints-)
    - [Type / Reference Constraints:](#type---reference-constraints-)
    - [Exceptions Constraints](#exceptions-constraints)
  + [Attributes](#attributes)
    - [TestFixture Attribute](#testfixture-attribute)
    - [Test Attribute](#test-attribute)
    - [TestCase Attribute](#testcase-attribute)
    - [Test Exception](#test-exception)
* [SetUp and TearDown](#setup-and-teardown)
* [Fakes, Mocks and Stubbing](#fakes--mocks-and-stubbing)
  + [Fakes](#fakes)
  + [Mocks](#mocks)
  + [Stubs](#stubs)

## Creating a Test Project

1. Create the `ProjectName.Tests` directory. The following outline shows the directory structure:

```
/Project-Folder
    ProjectName.sln
    /ProjectName
        Source Files
        ProjectName.csproj
    /ProjectName.Tests
```

2. Move into the .Tests directory
3. Create a new project using the following command:

```
dotnet new nunit
```

The `dotnet new` command creates a test project that uses NUnit as the test library. The generated template configures the test runner in the `ProjectName.Tests.csproj` file:

```XML
<ItemGroup>
  <PackageReference Include="nunit" Version="3.10.1" />
  <PackageReference Include="NUnit3TestAdapter" Version="3.10.0" />
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.7.2" />
</ItemGroup>
```

The test project requires other packages to create and run unit tests. `dotnet new` in the previous step added the Microsoft test SDK, the NUnit test framework, and the NUnit test adapter.

4. Add the main project class library as another dependency to the test project. While in the Test directory use the `dotnet add reference` command:

```
dotnet add reference ../ProjectName/ProjectName.csproj
```

The following outline shows the final solution layout:

```
/ProjectName
    Projectname.sln
    /ProjectName
        Source Files
        ProjectName.csproj
    /ProjectName.Tests
        Test Source Files
        ProjectName.Tests.csproj
```

5. Add the test project to the overall solution by executing the following command in the root directory of the project:

```
dotnet sln add ./ProjectName.Tests/ProjectName.Tests.csproj

```

6. Rename the `UnitTest1.cs` file to `ProjectName_ClassNameShould.cs`

When you open the root solution, you should now see your project and test project in the solution panel of Visual Studio.

## Writing Unit Tests with NUnit
The below is an example of a unit tests that tests whether the method `IsPrime`, (from the class `PrimeServices` which is stored on the namespace `Prime.Services`) returns false when 1 is passed into it.

```csharp
using NUnit.Framework;
using Prime.Services;

namespace Prime.UnitTests.Services
{
    [TestFixture]
    public class PrimeService_IsPrimeShould
    {
        private readonly PrimeService _primeService;

        public PrimeService_IsPrimeShould()
        {
            _primeService = new PrimeService();
        }

        [Test]
        public void ReturnFalseGivenValueOf1()
        {
            var result = _primeService.IsPrime(1);

            Assert.IsFalse(result, "1 should not be prime");
        }
    }
}
```

### Assertions
Under the  constraint model, we use a single method `That` and specify constraints to check our expected response.

That method apply a constraint to actual value. If constraint is satisfied the our test case is succeed else failed.

If an assertion fails, the method call does not return and an error is reported. If a test contains multiple assertions, any assertion that follow the one that failed will not be executed. For this reason, it's usually best to try for one assertion per test.

Below are helper classes to provide a constraint to assert method.
* `Is`
* `Has`
* `Contains`
* `Does`
* `Throws`

For example:
```csharp
Assume.That(myString, Is.EqualTo("Hello"));
```
#### Multiple Assertions
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

#### ListMapper
`ListMapper` is used to modify the actual value argument to `Assert.That()`. It transforms the actual value, which must be a collection, creating a new collection to be tested against the supplied constraint.

Example:

```csharp
string[] strings = new string[] { "a", "ab", "abc" };
int[] lengths = new int[] { 1, 2, 3 };

Assert.That(List.Map(strings).Property("Length"), Is.EqualTo(lengths));
```

#### Warnings
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

### Constraints
Constraints can be divided into eight categories:
1. Comparison
    * Equal
    * Not Equal
    * Greater Than
    * Less Than
    * Ranges
2. String
    * Equals and Ignore Case
    * Sub string
    * Empty
    * Starts With / Ends With
    * Regex
3. Collection
    * All Items
        * Not Null
        * All Greater Than
        * All Less Than
        * Instance Of
    * No Items
    * Exactly n Items
    * Unique Items
    * Contains
    * Ordered
        * Ascending
        * Descending
        * By Single Property
        * Multiple Properties
    * Superset/Subset
4. Conditional
    * Null
    * Boolean (True / False)
    * Empty
5. Compound
    * And
    * Not
    * Or
6. Directory/File
    * File or Directory Exits
    * Same Path
    * Empty Directory
7. Type/Reference
    * Instance Of
    * Same Type
    * Assignable to another Type
8. Exceptions
    * Is Exception throws
    * Expected Exception
    * Exception Message Comparison


#### Comparison Constraints
Equal Constraint Example:

```csharp
Assert.That(result, Is.EqualTo(5));
````

Not Equal Constraint Example:

```csharp
Assert.That(result, Is.Not.EqualTo(7));
```

Greater Than Constraint Example:

```csharp
Assert.That(result, Is.GreaterThan(2));
Assert.That(result, Is.GreaterThanOrEqualTo(5));
```

Less Than Constraint Example:

```csharp
Assert.That(result, Is.LessThan(9));
Assert.That(result, Is.LessThanOrEqualTo(5));
```

Ranges Example:

```csharp
Assert.That(result, Is.InRange(5, 10));
```

#### String Constraints

String Equal / Not Equal Constraint Example:

```csharp
Assert.That(result, Is.EqualTo("abcdefg"));
Assert.That(result, Is.Not.EqualTo("mnop"));
```

String Equal Ignore Case Example:

```csharp
Assert.That(result, Is.EqualTo("ABCDEFG").IgnoreCase);
```

Substring Constraint Example:

```csharp
Assert.That(result, Does.Contain("def").IgnoreCase);
Assert.That(result, Does.Not.Contain("igk").IgnoreCase);
```

Empty Example:

```csharp
sert.That(result, Is.Empty);
Assert.That(result, Is.Not.Empty);
````

Starts With / Ends With Examples:

```csharp
Assert.That(result, Does.StartWith("abc"));
Assert.That(result, Does.Not.StartWith("efg"));

Assert.That(result, Does.EndWith("efg"));
Assert.That(result, Does.Not.EndWith("mno"));
```

Regex Constraint Example:

```csharp
string result = "abcdefg";

Assert.That(result, Does.Match("a*g"));
Assert.That(result, Does.Not.Match("m*n"));
```

#### Collection Constraints
All Items Examples

```csharp
int[] array = new int[] { 1, 2, 3, 4, 5 };
```

Not Null Example

```csharp
Assert.That(array, Is.All.Not.Null);
```

All Greater Than Example

```csharp
Assert.That(array, Is.All.GreaterThan(0));
```

All Less Than Example:

```csharp
Assert.That(array, Is.All.LessThan(10));
```

Instance Of Example:

```csharp
Assert.That(array, Is.All.InstanceOf<Int32>());
```

No Items Example:

```csharp
Assert.That(array, Is.Empty);
Assert.That(array, Is.Not.Empty);
```

Exactly n Items Example:

```csharp
Assert.That(array, Has.Exactly(5).Items);
```

Unique Items Example:

```csharp
Assert.That(array, Is.Unique);
```

Contains Item:

```csharp
Assert.That(array, Contains.Item(4));
```

Ordered Examples:

Ascending:

```csharp
Assert.That(array, Is.Ordered.Ascending);
```

Descending:

```csharp
Assert.That(array, Is.Ordered.Descending);
```

By Single Property:

```csharp
List<Employee> employees = new List<Employee>();
employees.Add(new Employee { Age = 32 });
employees.Add(new Employee { Age = 49 });
employees.Add(new Employee { Age = 57 });

Assert.That(employees, Is.Ordered.Ascending.By("Age"));
Assert.That(employees, Is.Ordered.Descending.By("Age"));
```

By Multiple Properties:

```csharp
Assert.That(employees, Is.Ordered.Ascending.By("Age").Then.Descending.By("Name"));
```

SuperSet / SubSet Examples

```csharp
int[] array = new int[] { 1, 2, 3, 4, 5 };
int[] array2 = { 3, 4 };
Assert.That(array2, Is.SubsetOf(array));
```

#### Conditional Constraints
Null Constraint Examples:

```csharp
Assert.That(array, Is.Null);
Assert.That(array, Is.Not.Null);
```

Boolean (True / False)

```csharp
bool result = array.Length > 0;
Assert.That(result, Is.True);

Assert.That(result, Is.False);
```

Empty Constraint Example:

```csharp
Assert.That(array, Is.Empty);
```

#### Compound Constraints
AND constraint example:

```csharp
Assert.That(result, Is.GreaterThan(4).And.LessThan(10));
```

OR example:

```csharp
Assert.That(result, Is.LessThan(1).Or.GreaterThan(4));
```

NOT example:

```csharp
Assert.That(result, Is.Not.EqualTo(7));
```

#### Directory / File Constraints:

File or Directory exists:

```csharp
Assert.That(new FileInfo(path), Does.Exist);
Assert.That(new FileInfo(path), Does.Not.Exist);

Assert.That(new DirectoryInfo(path), Does.Exist);
Assert.That(new DirectoryInfo(path), Does.Not.Exist);
```

Same Path Example:

```csharp
Assert.That(path, Is.SamePath(@"c:\documents\imp1").IgnoreCase);
```

Empty Directory. Is.Empty returns true when directory has no files.

```csharp
Assert.That(new DirectoryInfo(path), Is.Empty);
```

#### Type / Reference Constraints:

Instance of example:

```csharp
IEmployee emp = new Employee();

Assert.That(emp, Is.InstanceOf<IEmployee>());
Assert.That(emp, Is.Not.InstanceOf<string>());
```

Exact Same Type Constraint:

```csharp
Assert.That(emp, Is.TypeOf<Employee>());
```

Assignable to another Type. For e.g. interface to implemented class.

```csharp
Assert.That(emp, Is.AssignableTo<Employee>());
```

#### Exceptions Constraints

Is Exception Throws By Method:

```csharp
IEmployee emp = new Employee();
emp.Age = 0;

Assert.That(emp.IsSeniorCitizen(), Throws.Exception);
```

Expected / Same Type Exception

```csharp
Assert.That(emp.IsSeniorCitizen(), Throws.TypeOf<ArgumentException>());
```

Exception Message Comparison:

```csharp
Exception ex = Assert.Throws<ArgumentException>(() => emp.IsSeniorCitizen());
Assert.That(ex.Message, Is.EqualTo("Age can not 0."));
```

### Attributes
NUnit supports a wide range of attributes. A full list can be found [here](https://github.com/nunit/docs/wiki/Attributes). Some notable attributes include:
* Combinational Attribute
* Description Attribute
* Explicit Attribute
* Ignore Attribute
* MaxTime Attribut
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

#### TestFixture Attribute
The `[TestFixture]` attribute denotes a class that contains unit tests.

#### Test Attribute
The `[Test]` attribute indicates a method is a test method.

Test methods targeting .Net 4.0 or higher may be marked as async and NUnit will wait for the method to complete before recording the result and moving on to the next test. Async test methods must return Task if no value is returned, or Task<T> if a value of type T is returned.

If the test method returns a value, you must pass in the `ExpectedResult` named parameter to the Test attribute. This expected return value will be checked for equality with the return value of the test method.

The test Description may be specified as a named parameter. This is exactly equivalent to using the DescriptionAttribute on the test.

```csharp
namespace NUnit.Tests
{
  using System;
  using NUnit.Framework;

  [TestFixture]
  public class SuccessTests
  {
    // A simple test
    [Test]
    public void Add()
    { /* ... */ }

    // A test with a description property
    [Test(Description="My really cool test")]
    public void Add()
    { /* ... */ }

    // Alternate way to specify description as a separate attribute
    [Test, Description("My really really cool test")]
    public void Add()
    { /* ... */ }

    // A simple async test
    [Test]
    public async Task AddAsync()
    { /* ... */ }

    // Test with an expected result
    [Test(ExpectedResult = 4)]
    public int TestAdd()
    {
        return 2 + 2;
    }

    // Async test with an expected result
    [Test(ExpectedResult = 4)]
    public async Task<int> TestAdd()
    {
        await ...
        return 2 + 2;
    }
  }
}
 ```

#### TestCase Attribute
A [TestCase] attribute is used to create a suite of tests that execute the same code but have different input arguments. You can use the [TestCase] attribute to specify values for those inputs.

For example, the below will run the test passing in the numbers -1, 0 and 1.

```csharp
[TestCase(-1)]
[TestCase(0)]
[TestCase(1)]
public void ReturnFalseGivenValuesLessThan2(int value)
{
    var result = _primeService.IsPrime(value);

    Assert.IsFalse(result, $"{value} should not be prime");
}
```

In the above example, we have fixed the result to false. But by using ExpectedResult property, we can also specify different results for different parameters. Below is the example:

```csharp
[TestCase(29, ExpectedResult = false)]
[TestCase(0, ExpectedResult = false)]
[TestCase(60, ExpectedResult = true)]
[TestCase(80, ExpectedResult = true)]
[TestCase(90, ExpectedResult = true)]
public bool When_AgeGreaterAndEqualTo60_Expects_IsSeniorCitizeAsTrue(int age)
{
    Employee emp = new Employee();
    emp.Age = age;

    bool result = emp.IsSeniorCitizen();

    return result;
}
```

In this example, we change the return type of method to bool data type and also change the last line of test case method to return statement. In the parameters, we specify the ExpectedResult as bool data type matching return type of test method.

The `TestCase` Attribute supports a number of [additional named parameters](https://github.com/nunit/docs/wiki/TestCase-Attribute). Some notable named parameter include:
* `Description` sets the description property of the test.
* `ExpectedResult` sets the expected result to be returned from the method, which must have a compatible return type.
* `Ignore` causes the test case to be ignored and specifies the reason.
* `TestName` provides a name for the test. If not specified, a name is generated based on the method name and the arguments provided. See Template Based Test Naming.

#### Test Exception
When we expect our test to raise an exception, use NUnit’s ```[ExpectedException]``` attribute.

For example. the following EncodePassword method throws an exception if the password is empty:

```csharp
public string EncodePassword(string password)
{
   if (password==null || password.Length==0)
   {
      throw new ValidationException("Password is empty");
   }
   // do the encoding
   return encoded_password;
}
```

We expect our test to throw ValidationException with the “Password is empty” error message. The following shows an example of how to test exceptions:

```csharp
[Test]
[ExpectedException(ValidationException,"Password is empty")]
public void Encoding ()
{
    authenticator.EncodePassword(null);
}
```

## SetUp and TearDown
Setup refers to some setup activities that need to be carried out before each test runs and tear down refers to finishing activities that need to be carried out after all tests have run.

Examples include:
1. Create a database connection before execution of each test
2. Create an instance of a particular object before execution of each test
3. Delete all connection to the database after execution each tests
4. Deleting a particular file from the system before execution of any test

In the tests below there are two tests. In each an object of the SUT Calculator class is required before execution and is disposed of after execution of all of the tests.
```csharp
using NUnit.Framework;

namespace Calculator.Tests
{

   [TestFixture]
   public class CalculatorTest
    {
       ICalculator sut;
       [TestFixtureSetUp]
       public void TestSetup()
       {
           sut = new Calculator();
       }
       [Test]
       public void ShouldAddTwoNumbers()
       {

           int expectedResult = sut.Add(7, 8);
           Assert.That(expectedResult, Is.EqualTo(15));
       }
       [Test]
       public void ShouldMulTwoNumbers()
       {

           int expectedResult = sut.Mul(7, 8);
           Assert.That(expectedResult, Is.EqualTo(56));
       }

       [TestFixtureTearDown]
       public void TestTearDown()
       {
           sut = null;
       }

    }
}
```

In the above, note:
1. There is a method attributed with `[TestFixtureSetUp]`. This method will be executed before the execution of any test. The SUT Calculator object created inside of this method will be availble to all tests.
2. There is a method attributed with `[TestFixtureTearDown]`. This function will be executed after the execution of all the tests. We are disposing the object of SUT Calculator inside this function.

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

## Fakes, Mocks and Stubbing
### Fakes
A fake is a class that you can use instead of actual line of business code.  A fake implementation might look something like this:

```csharp
public class FakeLoginService:ILoginService
{
    public int ValidateUser(string userName, string password)
    {
        return 5;
    }
}
```

This code contains no real business functionality; it is hard coded to return 5. However, it does provide two important benefits:
1. It allows you to write your first unit test.
2. It eliminates the need for a real Login Service, isolating dependent code from integration issues.

### Mocks
Mock objects are objects that replace the real objects and return hard-coded values. This helps test the class in isolation.

Mock objects can be used in the following cases:
* The real object has a nondeterministic behavior (it produces unpredictable results, like a date or the current weather temperature)
* The real object is difficult to set up
* The behaviour of the real object is hard to trigger (for example, a network error)
* The real object is slow
* The real object has (or is) a user interface
* The test needs to ask the real object how it was used (for example, a test may need to confirm that a callback function was actually called)
* The real object does not yet exist (a common problem when interfacing with other teams or new hardware systems)

The three key steps to using mock objects for testing are:

1. Use an interface to describe the object.
2. Implement the interface for production code.
3. Implement the interface in a mock object for testing.

### Stubs
Stubs provide canned answers to calls made during the test

Mocks are used to validate that the interactions between objects behave as expected.

***
The most popular mocking framework is Moq: https://github.com/Moq/moq4

This is an example of Moq being used to double Database connections created using Entity framework

https://docs.microsoft.com/en-us/ef/ef6/fundamentals/testing/mocking
