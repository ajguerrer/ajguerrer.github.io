+++
title = "Part 3: Integration Testing"
date = 2020-01-06
[taxonomies]
categories = ["Unit Testing Principles, Practices, and Patterns"]
tags = ["testing", "unit-testing"]
[extra]
+++

## Role of Integration Tests 

Integration tests are medium sized tests that verify:

- Interactions with external dependencies.
- Edge cases with resource costs unsuitable for a unit test.

Each integration test should choose a happy path that exercises as many shared external dependencies 
as possible while staying within a single use case. If a single integration test doesn't cover all 
shared external dependencies, write more until all external dependencies are covered.

{% tip(header="Tip") %}
To keep maintenance costs low, check as many edge cases as possible with unit tests before resorting 
to an integration test.
{% end %}

{% info(header="Info") %}
Edge cases that can be checked before starting an operation and changing state, such as
preconditions, adhere to the *Fail Fast principle* and don't require an integration test. 
{% end %}

While unit tests are great for testing domain logic, integration tests are great for testing 
controllers.

<div class="svg-content">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="593px" height="472px" viewBox="-0.5 -0.5 593 472">
  <g>
    <rect x="63" y="88" width="220" height="160" class="outline-text" stroke-width="3" pointer-events="all" />
    <rect x="63" y="248" width="220" height="160" class="outline-text" stroke-width="3" pointer-events="all" />
    <rect x="283" y="88" width="220" height="160" class="outline-text" stroke-width="3" pointer-events="all" />
    <rect x="283" y="248" width="220" height="160" class="outline-text" stroke-width="3" pointer-events="all" />
    <path d="M 63 408 L 572.9 408" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke" />
    <path d="M 63 408 L 63 18.1" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke" />
    <path d="M 579.65 408 L 570.65 412.5 L 572.9 408 L 570.65 403.5 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all" />
    <path d="M 63 11.35 L 67.5 20.35 L 63 18.1 L 58.5 20.35 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all" />
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 218px; height: 1px; padding-top: 328px; margin-left: 284px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <font style="font-size: 28px">
              Controller layer
              <br />
              -
              <br />
              Integration test
            </font>
          </div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 218px; height: 1px; padding-top: 168px; margin-left: 64px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <font style="font-size: 28px">
              Domain layer
              <br />
              -
              <br />
              Unit test
            </font>
          </div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 218px; height: 1px; padding-top: 328px; margin-left: 64px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <font style="font-size: 28px">
              Trivial code
              <br />
              -
              <br />
              Don't test
            </font>
          </div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 218px; height: 1px; padding-top: 168px; margin-left: 284px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <font style="font-size: 28px">
              Overcomplicated Code
              <br />
              -
              <br />
              Avoid writing
            </font>
          </div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 298px; height: 1px; padding-top: 438px; margin-left: 134px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Number of collaborators</div>
        </div>
      </div>
    </foreignObject>
    <g transform="rotate(-90 33 238)">
      <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
        <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 298px; height: 1px; padding-top: 238px; margin-left: -116px;">
          <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
            <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">Control flow complexity</div>
          </div>
        </div>
      </foreignObject>
    </g>
  </g>
  <switch>
    <g requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" />
    <a transform="translate(0,-5)" xlink:href="https://www.diagrams.net/doc/faq/svg-export-text-problems" target="_blank">
      <text text-anchor="middle" font-size="10px" x="50%" y="100%">Text is not SVG - cannot display</text>
    </a>
  </switch>
</svg>
</div>

#### Interfaces

Genuine abstractions are *discovered*, not *invented*. For an interface to be genuine, it must have 
at least two implementations. Otherwise, the the added cognitive complexity isn't worth it.

Interfaces enable mocking. Write an interface to mock shared external dependencies. 
However, not all external dependencies need an interface. Private external dependencies 
don't need mocks and therefore don't benefit from an interface, unless there is a need to be to swap
them with another implementation in production.

#### Layering

Adding layers of indirection where they are not appropriate makes code hard to reason with. Aim
to have as few layers as possible. Most applications only need three layers: Domain, Controller, and 
Infrastructure. The Domain and Controller Layers are already familiar. The Infrastructure Layer
provides utility libraries such as logging and networking.

<div class="svg-content">
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="502px" height="212px" viewBox="-0.5 -0.5 502 212">
  <g>
    <rect x="270" y="50" width="20" height="160" rx="3" ry="3" class="background-fill-outline-text" stroke-width="3" pointer-events="all" />
    <rect x="330" y="50" width="20" height="160" rx="3" ry="3" class="background-fill-outline-text" stroke-width="3" pointer-events="all" />
    <rect x="390" y="50" width="20" height="160" rx="3" ry="3" class="background-fill-outline-text" stroke-width="3" pointer-events="all" />
    <path d="M 180 165 Q 180 165 209.9 165" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke" />
    <path d="M 216.65 165 L 207.65 169.5 L 209.9 165 L 207.65 160.5 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all" />
    <rect x="10" y="70" width="170" height="120" rx="18" ry="18" class="outline-text" stroke-width="3" pointer-events="all" />
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 168px; height: 1px; padding-top: 130px; margin-left: 11px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 12px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <span style="font-size: 28px ; white-space: normal">Infrastructure Layer</span>
          </div>
        </div>
      </div>
    </foreignObject>
    <rect x="220" y="140" width="240" height="50" rx="7.5" ry="7.5" class="background-fill-outline-text" stroke-width="3" pointer-events="all" />
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 238px; height: 1px; padding-top: 165px; margin-left: 221px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 12px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <span style="font-size: 28px ; white-space: normal">Domain Layer</span>
          </div>
        </div>
      </div>
    </foreignObject>
    <path d="M 220 95 Q 220 95 190.8 95" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke" />
    <path d="M 184.05 95 L 193.05 90.5 L 190.8 95 L 193.05 99.5 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all" />
    <path d="M 460 95 Q 500 95 500 130 Q 500 165 470.1 165" class="outline-text" stroke-width="3" stroke-miterlimit="10" pointer-events="stroke" />
    <path d="M 463.35 165 L 472.35 160.5 L 470.1 165 L 472.35 169.5 Z" class="solid-text" stroke-width="3" stroke-miterlimit="10" pointer-events="all" />
    <rect x="220" y="70" width="240" height="50" rx="7.5" ry="7.5" class="background-fill-outline-text" stroke-width="3" pointer-events="all" />
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 238px; height: 1px; padding-top: 95px; margin-left: 221px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 12px; line-height: 1.2; pointer-events: all; white-space: normal; overflow-wrap: normal;">
            <span style="font-size: 28px ; white-space: normal">Controller Layer</span>
          </div>
        </div>
      </div>
    </foreignObject>
    <foreignObject pointer-events="none" width="100%" height="100%" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" style="overflow: visible; text-align: left;">
      <div xmlns="http://www.w3.org/1999/xhtml" style="display: flex; align-items: unsafe center; justify-content: unsafe center; width: 1px; height: 1px; padding-top: 20px; margin-left: 340px;">
        <div style="box-sizing: border-box; font-size: 0px; text-align: center;">
          <div style="display: inline-block; font-size: 28px; line-height: 1.2; pointer-events: all; white-space: nowrap;">Use cases</div>
        </div>
      </div>
    </foreignObject>
  </g>
  <switch>
    <g requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility" />
    <a transform="translate(0,-5)" xlink:href="https://www.diagrams.net/doc/faq/svg-export-text-problems" target="_blank">
      <text text-anchor="middle" font-size="10px" x="50%" y="100%">Text is not SVG - cannot display</text>
    </a>
  </switch>
</svg>
</div>

#### Logging

Logging falls under two categories:

- **Support logging** - messages are intended to be consumed by support staff and system admins; 
  observable behavior.
- **Diagnostic logging** - messages are intended to be consumed by developers; implementation 
  detail.

Since *Support* logging is observable behavior, it is worth the testing effort. *Diagnostic* 
logging, however, is an implementation detail and isn't worth testing.

{% tip(header="Tip") %}
The Domain layer should not have dependencies, including loggers. The Controller layer may inject a 
logger, or the Domain layer may emit a log event.
{% end %}

Effective logging maximizes the signal to noise ratio. *Support* logging cannot be controlled 
because its a business requirement, but *Diagnostic* logging can. Minimize diagnostic logging
in the Domain layer, only adding it for debugging and then subsequently removing it once finished.

## Mocking Best Practices

- **Mocks are for integration tests only** - Mentioned in 
  [Hexagonal Architecture](/unit-testing/making_tests_work/#hexagonal-architecture), only shared 
  external dependencies should be mocked. Unit tests target the Domain layer which shouldn't
  communicate with external dependencies.
- **Never mock private external dependencies** - When a private external dependency is too 
  difficult or prohibitive to setup, don't try to mock it out. If that dependency cannot be tested 
  as-is, it defeats the point of integration testing and should not be tested at all.
- **Mock the furthest edges of the system** - Mock the type that directly communicates with a 
  shared dependency, not the wrapper. Verify the message sent to a shared dependency, not a call
  to a class you wrote. Doing so maximizes resistance against regressions.
- **Use as many mocks as necessary per test** -  One mock per test is a common misconception. It's 
  irrelevant how many mocks it takes to verify one unit of behavior. The number of mocks depends
  solely on the number of shared external dependencies participating in the unit of behavior.
- **Verify the absence of unexpected messages too** - It is not enough to verify your system is 
  sending the correct messages to shared external dependencies, unexpected messages are a
  source of bugs too.
- **Mock only the types you own** - If your using a third party library to communicate with a shared
  external dependency, write a adapter for it and mock the adapter instead. Third-party 
  libraries have arcane inner workings, so it's futile to try to emulate their behavior.

## Testing the Database

#### Setup

Be able to build your database from a series of migration scripts checked into source control. This 
includes tables, views, indexes, stored procedures, reference data and anything else critical to 
run the database in production.

{% info(header="Info") %}
*Reference data* is data that is necessary for the application to run, but isn't actively modified 
by the application.
{% end %}

Migration scripts should be used in both testing and production. Once committed to source control,
don't modify them. Make a new migration script instead of fixing the old one, unless fixing the old
one prevents data loss.

#### Transactions

Transactions are capable of updating sets of data within the same business operation atomically. 
When transactions are involved, applications need to make two separate types of decisions:

1. Which data gets updated to what?
2. Should a set of updates be kept or rolled back?

These decisions should be separated into two different responsibilities:

- **Repositories** - Can access and modify database data; lifetime lasts the duration of the query.
- **Transactions** - Commits or rolls back a set of updates in full; lifetime lasts the duration of
  the business operation.

{% info(header="Info") %}
Non-relational databases lack transactions. Instead, updates to the document are atomic. Related 
data should be placed in the same document so that updates don't span more than one document.
{% end %}


To replicate the production environment as closely as possible, integration tests should wrap the 
*act* section in a separate transaction from the *arrange* and *assert* sections.

#### Cleanup

There are many ways to cleanup a database between tests:

- **Restore a backup database before each test** - Works, but is slow.
- **Cleanup data at the end of the test** - Might be skipped when test fails.
- **Wrap each test in a transaction and then don't commit** - Makes behavior between production and 
  testing inconsistent.
- **Cleanup data at the beginning of the test** - Fast, consistent, and won't get skipped.
 
#### Best Practices

- **Avoid in-memory databases** - Behavior between production and testing should be consistent.
- **Reuse code in test sections** - *Arrange* with factory or builder pattern, *act* with decorator
  pattern, and *assert* with handmade mocks (a.k.a. Spys).
- **Skip trivial database read tests** - Writes are always important because the alter the state of
  the database. Reads that important or complex should be tested; disregard the rest.
- **Don't test repositories directly** - Repositories are a controller, so they shouldn't be 
  complex. The point of an integration test is to cover shared, external dependencies, not 
  controllers.
