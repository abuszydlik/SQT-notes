SOFTWARE TESTING AUTOMATION
AAA structure of automated tests:
Arrange - define all input values that will be passed to class/method under test
Act - call behavior under test passing the previously set input values
Assert - check whether the system behaved as was expected

Failure - component of the system not behaving as expected.
Fault (defect, bug) - underlying flaw in a component that caused the system to behave incorrectly.
Error (mistake) - human action that caused the system to run not as expected.

Validation (are we building the right software?) - concerns features
Verification (are we building the system right?) - concerns proper system behavior

Principles of software testing:
a) exhaustive testing is impossible
b) bugs are not uniformly distributed
c) program testing can be used to show the presence of bugs, but not their absence
d) no single testing strategy can guarantee that the software under test is bug-free (pesticide paradox)
e) testing is context-dependent

SPECIFICATION-BASED TESTING
Specification-based testing uses requirements of the program (UML, user stories) as input for testing,
each input tackles one part (partition) of the program. Because it does not require knowledge of the software structure, it is also referred to as "black-box testing".

To devise a good test suite, the program is split into classes where (1) each class is unique and (2) it is easy to verify whether the behavior for a given input is correct or not.

Equivalence partitioning is an idea that a precise input for a particular partition does not matter because many inputs exercise the software in the same way.

Category-Partition method (systematic way of driving test cases):
a) identify parameters of the program (received classes and methods)
b) derive characteristics of each parameter
c) add constraints to minimize the test suite (invalid combinations, exceptional behaviors)
d) generate combinations of the input values

BOUNDARY TESTING
Boundary is a specific point where the input changes from one partition to another:
a) on-point - the value that is exactly on the boundary (seen directly in condition)
b) off-point - the value that is closest to the boundary and flips the condition
c) in-points - the values that make the condition true
d) out-points - the values that make the condition false

On and off points are also in and out points.

JUnit offers an @ParameterizedTest annotation which enables to provide a list of input from a source (for example CSV or a generator method) for a particular method to exercise boundary test automatically for each tuple.

CORRECT way of testing boundaries:
a) conformance - test what happens when data does not conform to a required format
b) ordering - make sure the program works even if data is not supplied in required order
c) range - test what happens when inputs are outside of an expected range
d) reference - when testing a method consider its references from outside of scope, external dependencies and other necessary conditions
e) existence - check what happens if something that is expected to exist doesn't
f) cardinality - test loops with zero, one, and multiple iterations
g) time - does the system handle timeouts and concurrency well

Domain testing (combined equivalent classes analysis and boundary testing) strategy:
a) read the requirements
b) identify the input and output variables
c) identify dependencies among input variables
d) perofrm equivalent class analysis
e) explore boundaries of found classes
f) find a strategy to derive test cases (minimize costs and maximize fault detection)
g) generate a set of test cases for system under test

STRUCTURAL TESTING
Techniques of testing which utilize the source code itself as a source of information to create tests are called structural testing. Because of coverage criteria it is "easy" to know when to stop testing a software system while doing structural tests. It is more objective and implementation-aware compared to specification-based testing, structural testing should ideally be used to complement specification-based tests.

There exist different coverage criteria (percentage of production code that is exercised by tests):
a) line (statement) coverage
b) block coverage
c) branch (decision) coverage
d) condition (basic and condition+branch) coverage
e) path coverage
f) MC/DC coverage

Line and statement coverage looks at the number of lines of code that are covered by at least one test, one of the biggest downsides of this type of coverage is that the number of lines may vary between developers and code conventions (nevertheless some development tools measure the coverage at the level of unique instructions for JVM which is more unified).

Control-flow graph is a tool used to represent all paths that may be taken during the execution of a piece of code, it consists of basic blocks (statements executed together no matter what happens), decision blocks (statements that can create different branches), edges connect blocks (basic block always has one outgoing edge, decision blocks have two edges for "true" and "false").

Block coverage counts the percentage of control-flow graph blocks covered, it does not depend on programing style of a developer.

Branch (decision) coverage counts the percentage of possible decision outcomes covered, a test suite achieves 100% branch coverage when all true/false edges on a control-flow graph are covered.

Condition coverage splits each compound condition into multiple decision blocks and requires exercising them separately, a test suite achieves 100% condition coverage when all the atomic conditions have been true and false at least once. In practice condition coverage is very often in form of branch+condition coverage which means that 100% of conditions and 100% of branches should be covered by test.

Path coverage considers the full combination of the conditions in a decision, it can be understood as the number of singular paths (unique paths) in a control-flow diagram, where each atomic condition multiplies the number of paths by 2. Achieving 100% path coverage is very difficult and often even impossible (for example because of unbounded loops).

Loop boundary adequacy criterion - a test suite satisfies this criterion if and only if every loop is exercised zero, one, and multiple times.

MC/DC (modified condition/decision coverage) - looks at the combinations of conditions like path coverage but instead of aiming to test all possible combinations it is supposed to exercise each condition in such a way that every parameter influences the outcome at least once. On average achieving 100% MC/DC coverage requires N+1 tests where N is the number of atomic conditions in a decision. More than one set of tests achieving 100% MC/DC coverage may exist, in some expressions it may be impossible to achieve 100% coverage.

Criteria subsumption - if a strategy X subsumes strategy Y, then all elements exercised by Y are also exercised by X, for example branch coverage subsumes line coverage (because having tested 100% of branches automatically means that 100% of lines must have been tested).

MODEL-BASED TESTING
Model is a simpler way to describe the program under test, it hold some of the attributes of the program for which it was built and gives a structured way to understand how the program operates.

Decision tables - used to model how a particular combination of conditions should lead to a certain action, they are easy to understand and can be validated by the client. Decision tables can be used to derive tests verifying whether the requirements were correctly implemented with respect to conditions.

Conditions represented in a decision table should always be independent of each other and their order should not matter. In some cases, the value of a condition may not influence the action - we call that a "don't care" (dc) value.

Number of conditions in a table can be limited by using default action. A default action should be the result if a combination of outcomes is not present in the table.

Deriving tests from decision tables:
a) all explicit variants - test case for each column (as many tests as there are columns)
b) all possible variant - test case for each possible combination (2^N tests, unrealistic)
c) every unique outcome - one test for each single outcome or action (as many tests as actions)
d) each condition T/F - each condition is true and false at least once (usually only two tests)
e) MC/DC - often optimal solution

Decision tables can be generalized to non-binary (non-boolean) choices which can be useful in small tables; however they also make the number of possible variants grow even faster. Some strategies like "pair-wise combinatorial testing" may be employed to achieve better scalability.

General guidelines for decision tables:
a) keep conditions independent
b) use DC values when possible
c) variants with DC values should not overlap (or at least have the same action)
d) add a default column (for all variants not in the decision table)
e) consider non-boolean conditions if conditions are mutually exclusive
f) if most conditions are non-boolean consider using a different technique

State machines - used to describe a system by its states and transitions between them. The states describe where a program is in execution, the transitions are actions that take the system from one state to another. State machine also has an initial state (state in which the system starts operating) and events (describe what has to happen for a transition to occur). Aa transition that happens only if a certain condition is met is called a conditional transition.

Errors types possible to find with state machines:
a) transition to a wrong state
b) wrong conditions in conditional transition
c) wrong action in a transition
d) variable behavior in a state

Main ways to define test coverage for a state machine based test suite:
a) state coverage - each state has to be reached at least once
b) transition coverage - each transition has to be exercised at least once
c) paths - combinations of transitions

Transition tree - a diagram spanning the graph of state machine where the root is the initial state and ancestors of a lowest level represent some unique path through (several) states. From a complete transition tree we can derive tests.

Sneak path is a path in the state machine that should not exist (that is a way to move between two states that is not a valid transition). Sneak paths can be found by using a transition table (states along the rows and triggering events along the columns).

Super state is a state that consists of its own state machine where any transition going into a super state goes to its initial state. Super states enable modularisation of a state machine by shifting focus to a particular part of system's behaviour.

A super state can be split into multiple orthogonal (independent) regions, each region contains one state machine and when the system enters the super state, it goes into the initial state of all internal regions.

OOP classes often correspond to small state machines, there exist two types of methods in such classes:
a) inspection methods - provide information about current state
b) trigger methods - bring the class into a new state
Because of that a test scenario utilizing a state machine is a series of calls to the class' trigger methods. If the state machine spans several classes it can be considered as an end-to-end test.

DESIGN BY CONTRACTS
Self-testing moves a part of the test suite into the production code itself (without added redundancy of test classes). In-code assertions allow the system to check whether it is running correctly during the execution of a program, if anything is not working as intended an error will be thrown. Although assertions do not replace unit tests because they are more general in nature, they can be used as a valuable extra safety measure

Simplest form of self-testing are assertions created with Java keyword assert. They are a Boolean expression evaluated at run-time which should be true unless there is a bug in the program. In case when an assertion yields false, an AssertionError is thrown. Assertions aren't enabled by default in Java.

Hoare Triples - used to reason about a program with assertion, they consist of a set of pre-conditions {P}, a program A, and a set of post-conditions {Q}. A Hoare Triple is expressed as {P}A{Q} and says that if we know that {P} holds then after execution of A we end up in a state where {Q} holds. Conditions can be easily turned into assertions but it may be more optimal to do explicit checks in code.

If the method's pre-conditions were not fully satisfied, the method might not guarantee its post-conditions.

A type of condition that always has to hold (i.e. it is a pre and post-condition at the same time) is called an invariant - it should hold true throughout the entire lifetime of as system, object or data structure. Invariants can be checked by delegating the functionality to a checker method.

Class invariant - condition that holds since the object's construction, every method can assume that when they start the invariant holds; this means that a private method can leave the object with its invariant being false but the caller public method should then fix the object before terminating.

Design by contracts - in a system where a client uses the server's API, they are bound by contract; as long as the client uses methods properly, the server guarantees to return correct results. In such design, contracts are represented by interfaces used by client and implemented on the server. Following subcontracting principles have to hold:
a) pre-conditions of a new implementation have to be weaker (or as weak as) pre-conditions on interface
b) post-conditions of a new implementation should be stronger (or as strong as) on interface
c) invariant of a new implementation should be stronger (or as strong as) on interface
This is summarized with a rule "require no more, ensure no less"

Liskov Substitution Principle - stats that if we have a class T and this class has some sub-classes, the clients of class T should be able to choose any of T's sub-classes. To test whether the classes follow LSP one should create an inheritance hierarchy for test classes where the super class obtains a test suite for its public methods which are then executed by its sub-classes. This is called parallel class hierarchy.

Factory Method desing pattern - used to ensure that the super test class executes its tests in the correct subclass. Super test class defines an abstract method that returns an instance with interface type, this forces the child test classes to override it and all general interface test methods will be executed while running the test suite.

PROPERTY-BASED TESTING
Given that some properties of an object should always hold, it is possible to test them with any input that we like, for example by employing a generator to quickly try a large number of different inputs. If we find an input value that makes the assertion fail, we can check that the property doesn't hold. Original implementation of this idea was developed for Haskell as QuickCheck, Java uses jqwik.

Property-based testing startegy:
a) define properties with @Property
b) add parameters to the annotated method, they will be automatically generated by jqwik
c) jqwik handles the number of inputs
d) once jqwik finds a value that breaks the property, it tries to find a smaller input that still makes the property fail.
@Provide annotation can be used for a method that is source for developer-defined values.

THE TESTING PYRAMID
Testing a system may be done on several different levels:
a) unit testing - testing a single feature in isolation from other. 
b) integration testing - tests multiple components together focusing on their interaction
c) system testing - exercises the whole system at once
d) manual testing - difficult and error prone but sometimes cannot be avoided (exploratory testing)
Testing pyramid is a model with unit testing at the bottom and manual testing at the top. It states that testers should favour unit tests to integration tests, integration tests to system tests, system tests to manual testing. Climbing levels of testing pyramid increases complexity and reality of tests.

Unit tests are fast, easy to control and easy to write; however they lack reality and bugs stemming from integration of several components can never be caught. They should be written when a component is about an algorithm or a single piece of business logic in the system.

Integration tests aren't a lot more complex than unit tests while offering increased testing capabilities; however they may require additional effort while working with external dependencies (for example setting up an instance of a database). They should be written whenever a component interacts with other external components.

System testing is realistic and better at stimulating user interaction; however they are slower, more difficult to write and can be flaky (might pass or fail for the same configuration). System tests should be written for the most crucial components of the system.

Test sizes at Google:
a) small tests - executed by a single process, don't use external components; fast and not flaky
b) medium tests - can span multiple processes, use threads and make external calls
c) large tests - may require calls to multiple machines, full end-to-end tests

TEST DOUBLES
Doubles are objects used to mimic the behaviour of some component, it is possible to make them behave in such a way that would be expected in the particular context while not requiring construction of the whole dependency. Advantages of test doubles:
a) more control - they don't require complicated setup to achieve a particular behavior (for example throwing an exception or using datetime)
b) faster - they don't require external communication with webservice or database
It is important to be careful while using test doubles (mocks) since it may lead to testing mock and not the actual code

Five types of test doubles:
a) dummy objects - passed to the class but never really used (for example as a required parameter)
b) fake object - have a real implementation (it is simpler than that of a mimicked object)
c) stubs - provide hard-coded answers to calls, they don't have an actual implementation
d) spies - obseve and record the interaction with a dependency
e) mocks - act like stubs but are pre-configured with explicit definition of interactions

Mockito is a testing framework which enables mocking, stubbing and spying on dependencies. It is a good practice to pass dependencies in the constructor of the class because it enables easier mocking. Mockito does not allow stubbing static methods (enemies of testability) so in order to test such a method we should create an abstraction on top of static call (interface). An interface can easily be stubbed or mocked.

Command-Query Separation (CQS) is a concept where command methods (ones that perform action on a system) and query methods (ones that return data to the caller) should be kept functionally separate, that is any one method should not be command and query at the same time.

Dependencies primarily should be mocked when:
a) they are too slow
b) they communicate with external infrastructure
c) have hard to simulate behavior

Developers tend not to mock:
a) entities
b) native libraries and utility methods

Best practices when mocking for interaction testing:
a) only mock types you own (so to mock a class from external library first build an abstraction on top)
b) write tests for interactions that shouldn't happen as well (bad weather)
c) focus on the part of interaction that really matters (no overspecification)
d) don't mock objects which have no relationships
e) don't add extra behaviour to mocks
f) mock only closest neighbours of a class
g) avoid too many mocks

Test doubles at Google:
a) using test doubles requires the system to be designed for testability
b) test doubles have to be as faithful as possible which may be challenging
c) if possible opt for the actual implementation
d) trade-offs to consider include execution time of the real implementation
e) prefer fakes over mocks
f) avoid excessive mocking
g) prefer state testing over interaction testing - assert the change in state
h) use interaction testing when state testing is not possible
i) avoid overspecified interaction tests
j) good interaction testing requires strict guidelines

DESIGN FOR TESTABILITY
Set of best practices that ensure a system is build in such a way that it is easy to write tests.

Dependency injection - a class asks for the dependency (via constructor or setters) instead of instantiating the dependency by itself. It improves the code by:
a) enabling to mock/stub the dependencies
b) making dependencies more explicit
c) better separation of concerns (class does not need to worry how to build its dependencies)
d) better extensibility (class may work with different implementations of dependencies)

Separation of domain and infrastructure:
- domain is where the business rules, logic, services, etc. exist (core of the program);
- infrastructure is code that handles communication with outside world (database, webservices, etc.)

Ports and adapters (hexagonal architecture) - idea that domain depends on "ports" rather than directly on infrastructure, the ports define exactly what the infrastructure should do. Adapters are close to infrastructure, they are implementations of ports that talk with external services. Communication happens between ports and adapters ensuring separation of domain and infrastructure concerns.

Dependency inversion principle:
- high level modules should not depend on low-level modules
- abstractions should not depend on details

Tips on designing for testability:
a) cohesive classes (designed to do only one thing) are easier to test; non-cohesive classes can split into several smaller classes
b) high coupling (large number of classes that a class depends on) decreases testability; refactoring workds by grouping dependencies together into a higher abstraction
c) complex conditions decrease testability; can be refactored by spreading the problem into multiple smaller conditions
d) private methods should be tested via their public methods; if a private method is complex and it seems that it needs to be tested separately, then it should be extracted and tested normally
e) static methods decrease testability; if a system has to depend on a static method then a layer of abstraction should be added on top of it (e.g. when using external frameworks and libraries)
High quality software is only achieved when the system is designed with testability in mind.
