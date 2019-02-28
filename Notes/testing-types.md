# Types of Tests
## Unit Tests
Unit testing is where individual units/ components of a piece of software are tested.

The purpose of unit testing is to validate that each unit performs as designed.

A unit is the smallest testable part of any software. In object-oriented programming, the smallest unit is a method.

Tests should be written in isolation. Interactions with other methods, classes, databases should be mocked.

Unit testing frameworks include [NUnit](/NUnit.md) and Jasmine.

More Info can be found [here](/Unit-Tests.md)

## Integration Tests
Integration testing is where individual units are combined and tested as a group.

The purpose of integration testing is to expose faults in the interaction between integrated units and to verify the combined functionality after integration.

![Alt Text](https://thumbs.gfycat.com/BriefUniformHornet-size_restricted.gif)

All units within an integration test should be unit tested before integration testing.

Integration tests can be written using NUnit and Jasmine

## Functional Tests
Functional testing is where a system is tested against the functional requirements/specifications.

Functions (or features) are tested by feeding them input and examining the output. This type of testing is not concerned with the internal logic of the system or how processing occurs, but only the results of processing. It simulates actual system usage but does not make any system structure assumptions.

Functional testing of web apps can be achieved using tools such as Selenium which uses a headless browser to simulate user clicks and interactions and check for the expected result.

## End To End Tests
End-to-end testing involves testing of a complete application environment in a situation that mimics real-world use, such as interacting with a database, using network communications, or interacting with other hardware, applications, or systems if appropriate.

End to End testing of a Gmail account may include the following steps:

1. Launching Gmail login page through URL.
2. Logging into Gmail account by using valid credentials.
3. Accessing Inbox. Opening Read and Unread emails.
4. Composing a new email, reply or forward an email.
5. Opening Sent items and checking emails.
6. Checking emails in the Spam folder
7. Logging out of Gmail application by clicking ‘logout’
