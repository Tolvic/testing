# Non-Deterministic (Flakey) tests - Common Causes and Remeidies 

Sumamry of Eradicating Non-Determinism in Tests by Martin Fowler

https://martinfowler.com/articles/nonDeterminism.html

A non-deterministic tests is a test that sometimes passes and sometimes fails.  Such tests fail, then you re-run them and they pass. Test failures for such tests are seemingly random.

Non-deterministic tests are a problem because:
1. they do not meet their purpose of bug detection
2. You lose confidence in your entire testing suite
3. increases the time it take to run the tests an get code into production

## Common Causes
* lack of isolation
* asynchronous behaviour
* remote srvices
* time
* resource leak

### Lack of Isolation

In order to get tests to run reliably, you must have clear control over the environment in which they run, so you have a well-known state at the beginning of the test. 

If one test creates or modifies some data and leaves it lying around, it can corrupt the run of another test which may rely on a different state.

It is therefore really important to focus on keeping tests isolated. Properly isolated tests can be run in any sequence.

> To achieve isolation, either:
> * always rebuild your starting state from scratch,
> * ensure that each test cleans up properly after itself

When using databases, conduct your tests inside a transaction and then rollback the transaction at the end of the tests. This is quicker and as the transaction managers clearns up less error prone. 

Be are of static data or singltons (an object that is created once only and is used where ever it is requestsed)

If you have an explicit tear-down in a test, be wary of exceptions that occur during the tear-down. If this happens the test can pass, but cause isolation failures for subsequent tests. So ensure that if you do get a problem in a tear-down, it makes a loud noise.

### Asynchronous Behaviour
Asynchrony is a boon that allows you to keep software responsive while taking on long term tasks. Ajax calls allow a browser to stay responsive while going back to the server for more data, asynchronous message allow a server process to communicate with other system without being tied to their laggardly latency.

But in testing, asynchrony can be curse. The common mistake here is to throw in a sleep:

```
//pseudo-code
makeAsyncCall;
sleep(aWhile);
readResponse;
```

The problem is this is that:
1. you spend a lot of time idely waiting for the responsee, thus slowing down your tests
2. sometimes, however long you wait, it will not be enough

> Never use bare sleeps to wait for asynchonous responses: use a callback or polling.

There are two tactics for testing asynchronous responses:
1. the asynchronous service to take a callback which it can call when done.
2. Polling the response

#### Callback
The first method is for the asynchronous service to take a callback which it can call when done. 

This is the best since it means you'll never have to wait any longer than you need to. you'll still need a timeout in case you never get a reply but you will set that timeout to be pretty high

#### Polling the Response
This is looking periodically for the the response:

```
//pseudo-code
makeAsyncCall
startTime = Time.now;
while(! responseReceived) {
  if (Time.now - startTime > waitLimit) 
  {
    throw new TestTimeoutException;
  }
  sleep (pollingInterval);
}
readResponse
```

The point of this approach is that you can set the pollingInterval to a pretty small value, and know that that's the maximum amount of dead time you'll lose to waiting for a response. This means you can set the waitLimit very high, which minimizes the chance of hitting it unless something serious has gone wrong.

Make sure you use a clear exception class that indicates this is a test timeout that's failing. This will help make it clear what's gone wrong should it happen.

The time values, in particular the waitLimit, should never be literal values. Make sure they are always values that can be easily set in bulk, either by using constants or set through the runtime environment. That way if you need to tweak them (and you will) you can tweak them all quickly.

If you have a general failure in something asynchronous, such that it's not responding at all, then you'll always be waiting for timeouts and your test suite will take a long time to fail. To combat this it's a good idea to use a smoke test to check that the asynchronous service is responding at all and stop the test run right away if it isn't.

https://martinfowler.com/books/meszaros.html contains lots of good patterns for constructing tests.

### Remote Services
Often remote systems don't have test system we can call, which means hitting a live system. If there is a test system, it may not be stable enough to provide deterministic responses.

In this situation it's vital to ensure determinism, so it's time to reach for a [Test Double](https://martinfowler.com/bliki/TestDouble.html) - a component that looks like the remote service, but is really just a pretend version that mimics the remote system's behavior. 


### Time
Few things are more non-deterministic than a call to the system clock. Each time you call it, you get a new result, and any tests that depend on it can thus change. 

The most important thing here is to ensure that you always wrap the system clock with routines that can be replaced with a seeded value for testing. A clock stub can be set to particular time and frozen at that time, allowing your tests to have complete control over its movements. That way you can synchronize your test data to the values in the seeded clock.

> Always wrap the system clock, so it can be easily substituted for testing.

### Resource Leaks
A resource leak referes to any resource that the application has to manage by acquiring and releasing. 

Examples of resource that are expensive to create and maintain are database connections and threads

If the application has some kind of resource leak, it will lead to random tests failing since its just the test test that caused the resource leak to go over the limit that gets the failure. 

If it isn't a case of one test being non-deterministic, then resource leaks are a good candidtae to investigate. 

Usually the best way to handle these kind of resources is through a [Resource Pool](https://martinfowler.com/bliki/ResourcePool.html) configured with a pool size of 1 and making it throw an exception should it get a request for a resource when it has none left to give. 
