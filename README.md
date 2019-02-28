# Testing
From the 25th February - 1st of March, BGL put on a learning and development week, where we could spend the week on our own personal learning and development. I decided on spend that week on learning testing.

## Testing Documentation
* [BGL Testing Documentation](http://confluence.bglgroup.net/pages/viewpage.action?pageId=38914119)
  * [Unit Testing](http://confluence.bglgroup.net/display/TE/Unit+Testing)
    + [Naming Conventions and Project Structure](http://confluence.bglgroup.net/display/TE/Unit+Testing#UnitTesting-NamingConventionandProjectStructure)
* [Types of Testing](/Notes/testing-types.md)
* [Unit Tests](/Notes/Unit-Tests.md)
  * [What are Unit Tests?](/Notes/Unit-Tests.md#what-are-unit-tests-)
  * [Benefits](/Notes/Unit-Tests.md#benefits)
    + [Less Time Performing Functional Test](/Notes/Unit-Tests.md#less-time-performing-functional-test)
    + [Protection Against Regression](/Notes/Unit-Tests.md#protection-against-regression)
    + [Executable Documentation](/Notes/Unit-Tests.md#executable-documentation)
    + [Less Coupled Code](/Notes/Unit-Tests.md#less-coupled-code)
  * [Characteristics of Good Unit tests](/Notes/Unit-Tests.md#characteristics-of-good-unit-tests)
  * [Terminology](/Notes/Unit-Tests.md#terminology)
  * [Best practices](/Notes/Unit-Tests.md#best-practices)
    + [Naming tests](/Notes/Unit-Tests.md#naming-tests)
    + [Arranging Tests - AAA](/Notes/Unit-Tests.md#arranging-tests---aaa)
    + [Write Minimal Passing tests](/Notes/Unit-Tests.md#write-minimal-passing-tests)
    + [Avoid Magic Strings](/Notes/Unit-Tests.md#avoid-magic-strings)
    + [Avoid Logic in Tests](/Notes/Unit-Tests.md#avoid-logic-in-tests)
    + [Prefer Helper Methods to Setup and Teardown](/Notes/Unit-Tests.md#prefer-helper-methods-to-setup-and-teardown)
    + [Avoid Multiple asserts](/Notes/Unit-Tests.md#avoid-multiple-asserts)
    + [Validate Private Methods by Testing Public Methods](#validate-private-methods-by-testing-public-methods)
    + [Stub Static References](#stub-static-references)
* [NUnit](/Notes/NUnit.md)
  * [Creating a Test Project](/Notes/NUnit.md#creating-a-test-project)
    + [Command Line](/Notes/NUnit.md#command-line)
    + [Visual Studio GUI](/Notes/NUnit.md#visual-studio-gui)
  * [Writing Unit Tests with NUnit](/Notes/NUnit.md#writing-unit-tests-with-nunit)
    + [Assertions](/Notes/NUnit.md#assertions)
      - [Multiple Assertions](/Notes/NUnit.md#multiple-assertions)
      - [ListMapper](/Notes/NUnit.md#listmapper)
      - [Warnings](/Notes/NUnit.md#warnings)
    + [Constraints](/Notes/NUnit.md#constraints)
      - [Comparison Constraints](/Notes/NUnit.md#comparison-constraints)
      - [String Constraints](/Notes/NUnit.md#string-constraints)
      - [Collection Constraints](/Notes/NUnit.md#collection-constraints)
      - [Conditional Constraints](/Notes/NUnit.md#conditional-constraints)
      - [Compound Constraints](/Notes/NUnit.md#compound-constraints)
      - [Directory / File Constraints:](/Notes/NUnit.md#directory---file-constraints-)
      - [Type / Reference Constraints:](/Notes/NUnit.md#type---reference-constraints-)
      - [Exceptions Constraints](/Notes/NUnit.md#exceptions-constraints)
    + [Attributes](/Notes/NUnit.md#attributes)
      - [TestFixture Attribute](/Notes/NUnit.md#testfixture-attribute)
      - [Test Attribute](/Notes/NUnit.md#test-attribute)
      - [TestCase Attribute](/Notes/NUnit.md#testcase-attribute)
      - [Test Exception](/Notes/NUnit.md#test-exception)
  * [SetUp and TearDown](/Notes/NUnit.md#setup-and-teardown)
  * [Fakes, Mocks and Stubbing](/Notes/NUnit.md#fakes--mocks-and-stubbing)
    + [Fakes](/Notes/NUnit.md#fakes)
    + [Mocks](/Notes/NUnit.md#mocks)
    + [Stubs](/Notes/NUnit.md#stubs)
* [Fixing Flaky tests](/Notes/Non-Deterministic-Tests.md)
  * [What is a Non-Deterministic Test?](/Notes/Non-Deterministic-Tests.md#what-is-a-non-deterministic-test-)
  * [Common Causes and Fixes](/Notes/Non-Deterministic-Tests.md#common-causes-and-fixes)
    + [Lack of Isolation](/Notes/Non-Deterministic-Tests.md#lack-of-isolation)
    + [Asynchronous Behaviour](/Notes/Non-Deterministic-Tests.md#asynchronous-behaviour)
      - [Callback](/Notes/Non-Deterministic-Tests.md#callback)
      - [Polling the Response](/Notes/Non-Deterministic-Tests.md#polling-the-response)
    + [Remote Services](/Notes/Non-Deterministic-Tests.md#remote-services)
    + [Time](/Notes/Non-Deterministic-Tests.md#time)
    + [Resource Leaks](/Notes/Non-Deterministic-Tests.md#resource-leaks)

## To Do
Please note that the links in this section link to material to be/ has been reviewed. The output of these tasks can be found in the other sections of this document.

- [x] Testing in General
  - [x] Types of testing
    - [x] Unit testing
    - [x] Integration testing
    - [x] Functional testing
    - [x] End to End testing
- [ ] Unit testing and NUnit
 - [x] Review Microsoft documentation
 - [x] Review NUnit Documentation
 - [x] [Unit testing with NUnit - MS Docs](https://docs.microsoft.com/en-gb/dotnet/core/testing/unit-testing-with-nunit?view=aspnetcore-2.2)
 - [ ] [Introduction to .NET Testing with NUnit 3 - Plural Sight](https://www.pluralsight.com/courses/nunit-3-dotnet-testing-introduction)
 - [x] [Creating unit tests for ASP.Net applications - MS Docs](https://docs.microsoft.com/en-us/aspnet/mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs )
 - [x] [Testing controller logic](https://docs.microsoft.com/en-gb/aspnet/core/mvc/controllers/testing?view=aspnetcore-2.2)
 - [ ] [Razor Page Unit Tests](https://docs.microsoft.com/en-gb/aspnet/core/test/razor-pages-tests?view=aspnetcore-2.2)
 - [ ] [Unit testing web APIs](https://docs.microsoft.com/en-us/aspnet/web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api)
 - [ ] [Unit testing controllers with Web APIs](https://docs.microsoft.com/en-us/aspnet/web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api)
 - [ ] [Integration Tests - MS Docs](https://docs.microsoft.com/en-gb/aspnet/core/test/integration-tests?view=aspnetcore-2.2)
 - [ ] Coverage metric
 - [x] Setup and tear down
 - [ ] Helper classes and functions
 - [ ] Mocking and stubbing
 - [x] Best practices
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
 - [ ] [Speed and performance](https://seleniumjava.com/2015/12/12/how-to-make-selenium-webdriver-scripts-faster/)
 - [ ] cheat sheet
