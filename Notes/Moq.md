# Moq
[Moq](https://www.nuget.org/packages/moq/) is a mocking framework for .NET

* [Basic Setup](#basic-setup)
 + [Returning a Value](#returning-a-value)
 + [Passing a Mock as a Dependency](#passing-a-mock-as-a-dependency)
* [Verification](#verification)
 + [Verifying that a Method is Called](#verifying-that-a-method-is-called)
 + [Verifying that a Method is Called x Number of Times](#verifying-that-a-method-is-called-x-number-of-times)
* [Return a Different Value Each Time a Mocked Method is called](#return-a-different-value-each-time-a-mocked-method-is-called)

Mock objects allow you to mimic the behavior of classes and interfaces, letting the code in the test interact with them as if they were real. For example, you can test a call to a database without having to actually talk to it.  This isolates the code you’re testing, ensuring that it works on its own and that no other code will make the tests fail.

With mocks, you can set up the object, including giving parameters and return values on method calls and setting properties. You can also verify that the methods you set up are being called in the tested code.

## Basic Setup
The below would take part in the arrange part of unit test.

1. Declare a new Mock Object:

```csharp
var mockObject = new Mock<IClassBeingMocked>();
```

2. setup what we to expect to occur:

The below example expects DoSomething to be called with any instance of a `Customer` object to be passed in

```csharp
var mockObject = new Mock<IClassBeingMocked>();
mockObject.setup(bar => bar.DoSomething(It.isAny<Customer>)))
```

### Returning a Value
The below example expects DoSomething to be called with a string of `"parameter"` to be passed in and it will return true.

```csharp
var mockObject = new Mock<IClassBeingMocked>();
mockObject.Setup(foo => foo.DoSomething("Parameter")).Returns(true);
```

### Passing a Mock as a Dependency
When a mocked object is passed into another object as a dependency, the mocked objected needs to be passed in with .object

```csharp
var mockDependency = new Mock<IClassBeingMocked>();

var classBeingTested = new ClassBeingTested(mockDependency.object);
```

This is because the mock object created by Moq is of type T rather than the type that the class being tested expects.

## Verification
When mocking with Moq, there are a number of simple verifications that can be carried out in the assert part of a test to confirm that the system under test (SUT) worked as you expected:
* Did the method on the mock object get called?
* How many times the method on the mock object get called?


### Verifying that a Method is Called

The below will verify that the DoSomething method is called but there is no expectation of how many times it should be called
```csharp
// Arrange
var mockDependency = new Mock<IClassBeingMocked>();

mockDependency.Setup(x => x.DoSomething(It.isAny<Customer>()));

var classBeingTested = new ClassBeingTested(mockDependency.object);

// Act
classBeingTested.someMethod();

// Assert
classBeingTested.Verify();
```


### Verifying that a Method is Called x Number of Times

The below will verify that the DoSomething method is called 3 times. Note that the expectation of the DoSomething method has been moved into the assertion rather than the arrange.

```csharp
// Arrange
var mockDependency = new Mock<IClassBeingMocked>();

var classBeingTested = new ClassBeingTested(mockDependency.object);

// Act
classBeingTested.someMethod();

// Assert
classBeingTested.Verify(x => x.DoSomething(It.isAny<Customer>()), Times.Exactly(3));
```

## Return a Different Value Each Time a Mocked Method is called

In the example below we are mocking an ID factory which has a `create` method that returns a new ID each time it is called. Therefore our mock needs to return a different value each time the `create` method is called.  

```csharp
var mockIdFactory = new Mock<IIdFactory>();

var i = 1;
mockIdFactory.setup(x => x.create())
  .Returns(() => i);
  .CallBack(() => i++);
```

The create method of the mockIdFactory returns a paramater value of `i` and the callback increments `i`.
