# SOFTWARE TESTING AUTOMATION  
## AAA structure of automated tests:  
* Arrange - define all input values that will be passed to class/method under test  
* Act - call behavior under test passing the previously set input values  
* Assert - check whether the system behaved as was expected  

Failure - component of the system not behaving as expected.  
Fault (defect, bug) - underlying flaw in a component that caused the system to behave incorrectly.  
Error (mistake) - human action that caused the system to run not as expected.  

Validation (are we building the right software?) - concerns features  
Verification (are we building the system right?) - concerns proper system behavior  

## Principles of software testing:  
* exhaustive testing is impossible  
* bugs are not uniformly distributed  
* program testing can be used to show the presence of bugs, but not their absence  
* no single testing strategy can guarantee that the software under test is bug-free (pesticide paradox)  
* testing is context-dependent  

# SPECIFICATION-BASED TESTING  
Specification-based testing uses requirements of the program (UML, user stories) as input for testing,
each input tackles one part (_partition_) of the program. Because it does not require knowledge of the software structure, it is also referred to as _black-box testing_.

To devise a good test suite, the program is split into classes where (1) each class is unique and (2) it is easy to verify whether the behavior for a given input is correct or not.

_Equivalence partitioning_ is an idea that a precise input for a particular partition does not matter because many inputs exercise the software in the same way.

## Category-Partition method (systematic way of driving test cases):  
* identify parameters of the program (received classes and methods)  
* derive characteristics of each parameter  
* add constraints to minimize the test suite (invalid combinations, exceptional behaviors)  
* generate combinations of the input values  

# BOUNDARY TESTING
Boundary is a specific point where the input changes from one partition to another:  
* on-point - the value that is exactly on the boundary (seen directly in condition)  
* off-point - the value that is closest to the boundary and flips the condition  
* in-points - the values that make the condition true  
* out-points - the values that make the condition false  

On and off points are also in and out points.  

JUnit offers an _@ParameterizedTest_ annotation which enables to provide a list of input from a source (for example CSV or a generator method) for a particular method to exercise boundary test automatically for each tuple.  

## CORRECT way of testing boundaries:  
* conformance - test what happens when data does not conform to a required format  
* ordering - make sure the program works even if data is not supplied in required order  
* range - test what happens when inputs are outside of an expected range  
* reference - when testing a method consider its references from outside of scope, external dependencies and other necessary conditions  
* existence - check what happens if something that is expected to exist doesn't  
* cardinality - test loops with zero, one, and multiple iterations  
* time - does the system handle timeouts and concurrency well  

## Domain testing (combined equivalent classes analysis and boundary testing) strategy:  
1. read the requirements  
2. identify the input and output variables  
3. identify dependencies among input variables  
4. perofrm equivalent class analysis  
5. explore boundaries of found classes  
6. find a strategy to derive test cases (minimize costs and maximize fault detection)  
7. generate a set of test cases for system under test  

# STRUCTURAL TESTING
Techniques of testing which utilize the source code itself as a source of information to create tests are called _structural testing_.   Because of coverage criteria it is "easy" to know when to stop testing a software system while doing structural tests. It is more objective and implementation-aware compared to specification-based testing, structural testing should ideally be used to complement specification-based tests.

## There exist different _coverage criteria_ (percentage of production code that is exercised by tests):  
* line (statement) coverage  
* block coverage  
* branch (decision) coverage  
* condition (basic and condition+branch) coverage  
* path coverage  
* MC/DC coverage  

__Line and statement coverage__ looks at the number of lines of code that are covered by at least one test. One of the biggest downsides of this type of coverage is that the number of lines may vary between developers and code conventions (nevertheless some development tools measure the coverage at the level of unique instructions for JVM which is more unified).

__Control-flow graph__ is a tool used to represent all paths that may be taken during the execution of a piece of code, it consists of _basic blocks_ (statements executed together no matter what happens), _decision blocks_ (statements that can create different branches), and _edges_ connecting blocks (basic block always has one outgoing edge, decision blocks have two edges for `true` and `false`).

__Block coverage__ counts the percentage of control-flow graph blocks covered, it does not depend on programing style of a developer.

__Branch (decision) coverage__ counts the percentage of possible decision outcomes covered, a test suite achieves 100% branch coverage when all true/false edges on a control-flow graph are covered.

__Condition coverage__ splits each compound condition into multiple decision blocks and requires exercising them separately, a test suite achieves 100% condition coverage when all the atomic conditions have been `true` and `false` at least once. In practice condition coverage is very often in form of branch+condition coverage which means that 100% of conditions and 100% of branches should be covered by test.

__Path coverage__ considers the full combination of the conditions in a decision, it can be understood as the number of singular paths (unique paths) in a control-flow diagram, where each atomic condition multiplies the number of paths by 2. Achieving 100% path coverage is very difficult and often even impossible (for example because of unbounded loops).

_Loop boundary adequacy criterion_ - a test suite satisfies this criterion if and only if every loop is exercised zero, one, and multiple times.

__MC/DC (modified condition/decision coverage)__ - looks at the combinations of conditions like path coverage but instead of aiming to test all possible combinations it is supposed to exercise each condition in such a way that every parameter influences the outcome at least once. On average achieving 100% MC/DC coverage requires `N+1` tests where `N` is the number of atomic conditions in a decision. More than one set of tests achieving 100% MC/DC coverage may exist. In some expressions it may be impossible to achieve 100% coverage.

_Criteria subsumption_ - if a strategy X subsumes strategy Y, then all elements exercised by Y are also exercised by X For example branch coverage subsumes line coverage (because having tested 100% of branches automatically means that 100% of lines must have been tested).

# MODEL-BASED TESTING
_Model_ is a simpler way to describe the program under test, it hold some of the attributes of the program for which it was built and gives a structured way to understand how the program operates.

_Decision tables_ - used to model how a particular combination of conditions should lead to a certain action, they are easy to understand and can be validated by the client. Decision tables can be used to derive tests verifying whether the requirements were correctly implemented with respect to conditions.

Conditions represented in a decision table should always be independent of each other and their order should not matter. In some cases, the value of a condition may not influence the action - we call that a _don't care_ (_dc_) value.

Number of conditions in a table can be limited by using _default action_. A default action should be the result if a combination of outcomes is not present in the table.

## Deriving tests from decision tables:  
* all explicit variants - test case for each column (as many tests as there are columns)  
* all possible variant - test case for each possible combination (2^N tests, unrealistic)  
* every unique outcome - one test for each single outcome or action (as many tests as actions)  
* each condition T/F - each condition is true and false at least once (usually only two tests)  
* MC/DC - often optimal solution  

_Decision tables_ can be generalized to non-binary (non-boolean) choices which can be useful in small tables; however they also make the number of possible variants grow even faster. Some strategies like _pair-wise combinatorial testing_ may be employed to achieve better scalability.

## General guidelines for decision tables:  
* keep conditions independent  
* use DC values when possible  
* variants with DC values should not overlap (or at least have the same action)  
* add a default column (for all variants not in the decision table)  
* consider non-boolean conditions if conditions are mutually exclusive  
* if most conditions are non-boolean consider using a different technique  

_State machines_ - used to describe a system by its states and transitions between them. The _states_ describe where a program is in execution, the _transitions_ are actions that take the system from one state to another. State machine also has an _initial state_ (state in which the system starts operating) and _events_ (describe what has to happen for a transition to occur). A transition that happens only if a certain condition is met is called a _conditional transition_.

## Error types possible to find with state machines:  
* transition to a wrong state  
* wrong conditions in conditional transition  
* wrong action in a transition  
* variable behavior in a state  

## Main ways to define test coverage for a state machine based test suite:  
* state coverage - each state has to be reached at least once  
* transition coverage - each transition has to be exercised at least once  
* paths - combinations of transitions  

_Transition tree_ - a diagram spanning the graph of state machine where the root is the initial state and ancestors of a lowest level represent some unique path through (several) states. From a complete transition tree we can derive tests.

_Sneak path_ - a path in the state machine that should not exist (that is a way to move between two states that is not a valid transition). Sneak paths can be found by using a _transition table_ (states along the rows and triggering events along the columns).

_Super state_ is a state that consists of its own state machine where any transition going into a super state goes to its initial state. Super states enable modularisation of a state machine by shifting focus to a particular part of system's behaviour.

A super state can be split into multiple orthogonal (independent) regions, each region contains one state machine and when the system enters the super state, it goes into the initial state of all internal regions.

## OOP classes often correspond to small state machines, there exist two types of methods in such classes:  
* inspection methods - provide information about current state  
* trigger methods - bring the class into a new state  

Because of that a test scenario utilizing a state machine is a series of calls to the class' trigger methods. If the state machine spans several classes it can be considered as an end-to-end test.

# DESIGN BY CONTRACTS
_Self-testing_ moves a part of the test suite into the production code itself (without added redundancy of test classes). In-code assertions allow the system to check whether it is running correctly during the execution of a program, if anything is not working as intended an error will be thrown. Although assertions do not replace unit tests because they are more general in nature, they can be used as a valuable extra safety measure

Simplest form of self-testing are _assertions_ created with Java keyword `assert`. They are a Boolean expression evaluated at run-time which should be true unless there is a bug in the program. In case when an assertion yields `false`, an `AssertionError` is thrown. Assertions aren't enabled by default in Java.

_Hoare Triples_ - used to reason about a program with assertion, they consist of a set of __pre-conditions__ {P}, a __program__ A, and a set of __post-conditions__ {Q}. A Hoare Triple is expressed as {P}A{Q} and says that if we know that {P} holds then after execution of A we end up in a state where {Q} holds. Conditions can be easily turned into assertions but it may be more optimal to do explicit checks in code.

If the method's pre-conditions were not fully satisfied, the method might not guarantee its post-conditions.

A type of condition that always has to hold (i.e. it is a pre and post-condition at the same time) is called an _invariant_ - it should hold `true` throughout the entire lifetime of as system, object or data structure. Invariants can be checked by delegating the functionality to a checker method.

_Class invariant_ - condition that holds since the object's construction, every method can assume that when they start the invariant holds; this means that a private method can leave the object with its invariant being `false` but the caller public method should then fix the object before terminating.

_Design by contracts_ - in a system where a client uses the server's API, they are bound by contract; as long as the client uses methods properly, the server guarantees to return correct results. In such design, contracts are represented by interfaces used by client and implemented on the server. Following subcontracting principles have to hold:  
1. pre-conditions of a new implementation have to be weaker (or as weak as) pre-conditions on interface  
2. post-conditions of a new implementation should be stronger (or as strong as) on interface  
3. invariant of a new implementation should be stronger (or as strong as) on interface  

This is summarized with a rule _require no more, ensure no less_  

_Liskov Substitution Principle_ - states that if we have a class `T` and this class has some sub-classes, the clients of class `T` should be able to choose any of `T`'s sub-classes. To test whether the classes follow LSP one should create an inheritance hierarchy for test classes where the super class obtains a test suite for its public methods which are then executed by its sub-classes. This is called _parallel class hierarchy_.

_Factory Method desing pattern_ - used to ensure that the super test class executes its tests in the correct subclass. Super test class defines an abstract method that returns an instance with interface type, this forces the child test classes to override it and all general interface test methods will be executed while running the test suite.

# PROPERTY-BASED TESTING
Given that some properties of an object should always hold, it is possible to test them with any input that we like, for example by employing a generator to quickly try a large number of different inputs. If we find an input value that makes the assertion fail, we can check that the property doesn't hold. Original implementation of this idea was developed for Haskell as __QuickCheck__, Java uses __jqwik__.

## Property-based testing startegy:  
1. define properties with @Property  
2. add parameters to the annotated method, they will be automatically generated by jqwik  
3. jqwik handles the number of inputs  
4. once jqwik finds a value that breaks the property, it tries to find a smaller input that still makes the property fail.  

@Provide annotation can be used for a method that is source for developer-defined values.  

# THE TESTING PYRAMID
## Testing a system may be done on several different levels:  
1. unit testing - testing a single feature in isolation from other  
2. integration testing - tests multiple components together focusing on their interaction  
3. system testing - exercises the whole system at once  
4. manual testing - difficult and error prone but sometimes cannot be avoided (_exploratory testing_)  

Testing pyramid is a model with unit testing at the bottom and manual testing at the top. It states that testers should favour unit tests to integration tests, integration tests to system tests, system tests to manual testing. Climbing levels of testing pyramid increases complexity and reality of tests.

__Unit tests__ are fast, easy to control and easy to write; however they lack reality and bugs stemming from integration of several components can never be caught. They should be written when a component is about an algorithm or a single piece of business logic in the system.

__Integration tests__ are a more complex than unit tests but offer increased testing capabilities; however they may require additional effort while working with external dependencies (for example setting up an instance of a database). They should be written whenever a component interacts with other external components.

__System testing__ is realistic and better at stimulating user interaction; however they are slower, more difficult to write and can be _flaky_ (might pass or fail for the same configuration). System tests should be written for the most crucial components of the system.

## Test sizes at Google:  
1. small tests - executed by a single process, don't use external components; fast and not flaky  
2. medium tests - can span multiple processes, use threads and make external calls  
3. large tests - may require calls to multiple machines, full end-to-end tests  

# TEST DOUBLES
_Doubles_ are objects used to mimic the behaviour of some component, it is possible to make them behave in such a way that would be expected in the particular context while not requiring construction of the whole dependency. 
## Advantages of test doubles:
* more control - they don't require complicated setup to achieve a particular behavior (for example throwing an exception)  
* faster - they don't require external communication with webservice or database  
It is important to be careful while using test doubles (mocks) since it may lead to testing mock and not the actual code

## Five types of test doubles:
1. dummy objects - passed to the class but never really used (for example as a required parameter)  
2. fake object - have a real implementation (it is simpler than that of a mimicked object)  
3. stubs - provide hard-coded answers to calls, they don't have an actual implementation  
4. spies - obseve and record the interaction with a dependency  
5. mocks - act like stubs but are pre-configured with explicit definition of interactions  

_Mockito_ is a testing framework which enables mocking, stubbing and spying on dependencies. It is a good practice to pass dependencies in the constructor of the class because it enables easier mocking. Mockito does not allow stubbing static methods (_"enemies of testability"_) so in order to test such a method we should create an abstraction on top of static call (interface). An interface can easily be stubbed or mocked.

_Command-Query Separation (CQS)_ - concept where __command methods__ (ones that perform action on a system) and __query methods__ (ones that return data to the caller) should be kept functionally separate, that is any one method should not be command and query at the same time.

## Dependencies primarily should be mocked when:  
* they are too slow  
* they communicate with external infrastructure  
* have hard to simulate behavior  

## Developers tend not to mock:  
* entities  
* native libraries and utility methods  

## Best practices when mocking for interaction testing:  
1. only mock types you own (so to mock a class from external library first build an abstraction on top) 
2. write tests for interactions that shouldn't happen as well (bad weather) 
3. focus on the part of interaction that really matters (no overspecification) 
4. don't mock objects which have no relationships 
5. don't add extra behaviour to mocks 
6. mock only closest neighbours of a class  
7. avoid too many mocks  

## Test doubles at Google:  
* using test doubles requires the system to be designed for testability  
* test doubles have to be as faithful as possible which may be challenging  
* if possible opt for the actual implementation  
* trade-offs to consider include execution time of the real implementation  
* prefer fakes over mocks  
* avoid excessive mocking  
* prefer state testing over interaction testing - assert the change in state  
* use interaction testing when state testing is not possible  
* avoid overspecified interaction tests  
* good interaction testing requires strict guidelines  
 
# DESIGN FOR TESTABILITY
Set of best practices that ensure a system is build in such a way that it is easy to write tests.

_Dependency injection_ - a class asks for the dependency (via constructor or setters) instead of instantiating the dependency by itself.  ## It improves the code by:  
* enabling to mock/stub the dependencies  
* making dependencies more explicit  
* better separation of concerns (class does not need to worry how to build its dependencies)  
* better extensibility (class may work with different implementations of dependencies)  

## Separation of domain and infrastructure:  
* domain is where the business rules, logic, services, etc. exist (core of the program)  
* infrastructure is code that handles communication with outside world (database, webservices, etc.)  

_Ports and adapters (hexagonal architecture)_ - idea that domain depends on _ports_ rather than directly on infrastructure, the ports define exactly what the infrastructure should do. _Adapters_ are close to infrastructure, they are implementations of ports that talk with external services. Communication happens between ports and adapters ensuring separation of domain and infrastructure concerns.

## Dependency inversion principle:  
* high level modules should not depend on low-level modules  
* abstractions should not depend on details  

## Tips on designing for testability:
1. cohesive classes (designed to do only one thing) are easier to test; non-cohesive classes can split into several smaller classes  
2. high coupling (large number of classes that a class depends on) decreases testability; refactoring workds by grouping dependencies together into a higher abstraction  
3. complex conditions decrease testability; can be refactored by spreading the problem into multiple smaller conditions  
4. private methods should be tested via their public methods; if a private method is complex and it seems that it needs to be tested separately, then it should be extracted and tested normally  
5. static methods decrease testability; if a system has to depend on a static method then a layer of abstraction should be added on top of it (e.g. when using external frameworks and libraries)  

High quality software is only achieved when the system is designed with testability in mind.

# MONITORING
_Monitoring_ is one of the ways to execute testing in production where the goal is to check the overall health of the system. It is important to note that effective monitoring should focus on only some core metrics such as an increase in the error rate or increase in latency. There currently exists a lot of research into log engineering, infrastructure and analysis.

# WEB TESTING
## Characteristics of web applications
1. Front-end is usually written in JavaScript:
   * programming paradigms may differ
   * very important to design for testability, for example mixing JS and HTML makes it difficult to test
   * helpful to use a framework (React, Angular, Vue.js) to achieve proper structure when creating new project

2. Application follows the client-server model:
   * communication over HTTP requires an interface which reduces coupling
   * important to conduct end-to-end tests which require a server running in a proper state

3. Everyone can access the application which requires specific testing scenarios:
   * usability testing - how easy it is to use the application
   * accessibility testing - can people with disabilities use the application comfortably
   * load testing - can the application support a large number of concurrent users
   * security testing - what happens when a user has malicious intent

4. Front end runs in a browser:
   * cross-browser testing is required to ensure that the application will work in all supported browsers
   * responsive web design should be tested to ensure that the web pages are rendered properly in different sizes on various devices
   * HTML should be designed for testability so that single elements can be selected and UI can be tested
   * UI component testing should be used that triggered events lead DOM into an expected state
   * snapshot testing is used to check whether the UI does not change unexpectedly
   * end-to-end tests can be performed for example with Selenium WebDriver and Cypress
   
5. Many web applications are asynchronous:
   * while the browser is awaiting for results, the application can still be used
   * waiting in tests can lead to unwanted flakiness
   
## Design for testability in web applications
There exist several heuristics used in order to achieve a testable code:
   * JavaScript code should be stored in a separate file (and not left inline with HTML)
   * logic and user interface code should be separate (achieved by splitting functions)
   * elements should be easy to locate with query selectors (for example by using labels)
   * good structure can be achieved by writing a proper MVC implementation

## Unit testing frameworks
In web development there is no industry standard framework. If the system at hand was previously written with a specific testing framework then it is usually best to stick to that. On the other hand when choosing a framework for a new application, it may be a good idea to use a one with large following for easier support. Some libraries/frameworks recommend a specific testing framework - for example the recommended testing framework for _React_ applications is _Jest_.

_Jest_ syntax example:
``` javascript
describe('dateToString', () => {
  test('returns the date in the form "yyyy-MM-dd"', () => {
    var date = new Date(2020, 4, 1);   // May 1st, 2020
    expect(dateToString(date)).toEqual("2020-05-01");
  });
});
```
In Jest `describe`can be used to group several tests together, `test` is used to write a single test case.
To test UI components in a virtual DOM it is beneficial to use `react-testing-library` which enables testing components in a way that the user would interact with them with functions like `getByText` to look up an element. Jest enables mocking of functions with `jest.mock('./{}')` which is especially useful for testing HTTP requests to back end.

## Snapshot testing
Snapshot testing is a good way to test whether UI behaves as expected. During the first run, a snapshot is taken and saved. In subsequent runs the rendered output is compared to the snapshot, the test fails if the versions differ and the developer is presented with differences.

## End-to-end testing
An end-to-end test for a web application contains a series of events that should be as similar to the flow of an actual user as possible. Manual tests are sometimes unavoidable but there exist good tools to automate at least part of the work including _Selenium WebDriver_ (the WebDriver API is now a W3C standard and several implementations exist) which acts as a remote control for the browser, instructing it what should be performed. WebDriver tests can be written in several supported languages including Java. By running the tests on different browsers we can additionally perform _cross-browser testing_. Another similar tool is _Cypress_ which integrates more closely with the browser and presents some additional features.

## Page Objects and State Objects
_Page objects_ are an abstraction created on top of the web application. They include only methods that are needed in the tests and are used to increase their readability. Methods in _page objects_ relate directly to the application rather than HTML elements.

It is also possible to go a step further and make the page objects correspond to the states in a _navigational state machine_ which describes the flow through the application. Each page is represented by a state. We call this specific type of page object a _state object_. Tests can be described by inspection, trigger, and self-checking methods.

## Behaviour-driven desing
In _behaviour-driven design_ the system is created based on a scenarions written in natural language. The scenarios usually follow a standard format:
   1. Title of the scenario
   2. Given ... : conditions that should hold at the start
   3. When ... : describes the triggering event 
   4. Then ... : result at the end
   
_Given_/_When_/_Then_ clauses can consist of several shorter statements linked with _And_. A scenario covers a single transition so to get an overview of the whole system, the entire state machine is still required.

## Usability and accessibility testing
Although they are closely related, __usability__ refers more to making the application easy to use while __accessibility__ is about making the content accessible for people with disabilities. _Web Content Accessibility Guidelines_ offer a summary of what the developer should take into account while designing an application. Additionally tools like _Axe_ can offer some help when diagnosing accessibility problems. Nevertheless a large part of testing still has to be conducted manually.

# TEST-DRIVEN DEVELOPMENT
## The TDD cycle
_Test-driven development_ offers an alternative approach to writing code. With given requirements we start by creating test cases which will take us closer to fulfilling the requirements. If the test fails, we work on the production code to add new features and make the test pass. Once this is achieved, we start refactoring the test and production code to increase its quality. Then the entire process is repeated until all requirements are fulfilled.

## Advantages of TDD
* prevents the developer from writing code that isn't directly required
* easier to control the pace of writing production code
* test cases are derived from requirements (possible to validate and verify at the same time)
* code is testable from the beginning
* quick feedback on the code (possible to easily identify new problems relating to small amount of code)
* encourages developers to work in small steps and according to a single strategy
* feedback on design (makes it easier to write testable code)

## When (not) to use test-driven development
* use it when you don't know how to design a particular part of the system
* use it when you face a complex problem in which you lack experience
* do not use it if there is nothing to be learned or explored (problem is familiar)

# TEST CODE QUALITY AND ENGINEERING
## The FIRST Properties of Good Tests
* Fast - good tests should be fast to encourage the developer to run them often. Possible solutions for slow tests include usage of mocks/stubs, redesigning the production code or creating a new test suite.
* Isolated - ideally a single test method should test just a single functionality. Complex tests make it more difficult to notice which component is failing, so big test methods should be broken down. Furthermore it is necessary that one test does not depend on any others to run (tests should clean up after running).
* Repeatable - a single test should always give the same result because developers tend to lose their trust if a test is flaky. Common causes of flakiness include dependencies, asynchronicity and concurrency.
* Self-validating - tests should validate the result themselves by always including the assertion.
* Timely - the development team should have a mindset that their tests should run as often as possible.

## Test desiderata
According to Kent Beck good tests should be:
* isolated: tests should return the same results regardless of the order in which they are run.
* composable: if tests are isolated, then I can run 1 or 10 or 100 or 1,000,000 and get the same results.
* fast: tests should run quickly.
* inspiring: passing the tests should inspire confidence.
* writable: tests should be cheap to write relative to the cost of the code being tested.
* readable: tests should be comprehensible for their readers, and it should be clear why they were written.
* behavioural: tests should be sensitive to changes in the behaviour of the code under test. If the behaviour changes, the test result should change.
* structure-insensitive: tests should not change their result if the structure of the code changes.
* automated: tests should run without human intervention.
* specific: if a test fails, the cause of the failure should be obvious.
* deterministic: if nothing changes, the test result should not change.
* predictive: if the tests all pass, then the code under test should be suitable for production.

~~To remember all mentioned properties, one can use the anagram __bi-dwarf's pics__~~

## Test code smells
_Code smells_ relate to symptoms that indicate possible deep problems in the source code, according to research they negatively influence comprehensibility and maintainability of software system. Some examples of test code smells include:
* __Code duplication__ - to reduce duplication developers are encouraged to use `@BeforeEach` and `@ParameterizedTest`, additionally it is necessary to refactor the code and extract duplicated pieces to private methods or external classes.
* __Assertion Roulette__ - assertions and their failures should be easy to understand. If the test requires a complicated assertion statement, the developer should abstract it in a customized assert instruction. Other possibilities include writing comments to explain assertions and minimizing the number of assertions per test method.
* __Resource Optimism__ - happens when a test assumes that a necessary resource is readily available. To avoid this smell you need to ensure that the test itself is responsible for setting up the state, furthermore developers can avoid external resources (by mocking and stubbing) or providing additional robustness to tests. It is also important to use a proper CI tool like _Jenkins_, _CircleCI_ or _Travis_.
* __Test Run War__ - happens when two tests are fighting over the same resource, for example a centralized database. It is necessary to keep the tests _isolated_ by for example providing each developer with their own instance of the database.
* __General Fixture__ - a fixture is a set of inputs used to exercise the component under test, general fixture happens when a fixture is too complex and different tests have intersecting fixtures. To avoid this smell it may be usefyl to employ _Test Data Builder_ pattern.
* __Indirect tests and eager tests__ - a test should be as cohesive and as focused as possible. It is best practice to ensure that only a single unit is tested (by employing mocks, making sure that the assertion focuses on class under test, or that failures in dependencies are clearly explained) and that only a single behavior is tested.
* __Sensitive Equality__ - test code should be as resilient as possible to the implementation details of component under test. It may be better to create a method only to facilitate a test instead of relying on sensitive assertions.
* __Inappropriate assertions__ - assertions should be chosen in such a way that their error messages make it easy to undersand what went wrong. Good assertion is legible and as specific as possible.
* __Mystery Guest__ - tests which rely on external dependencies (guests) should make it explicit to the developer. It is necessary to give proper error messages that differentiate between a fail in the expected behaviour and a fail due to a problem in the dependency.
