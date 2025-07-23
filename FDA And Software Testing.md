Software Testing
================

Development driven testing distinguishes itself from other forms of software
testing (e.g. QA-orchestrated testing; BAT, PEN, UX/UI, etc.) through being
created and oftentimes executed by software developers.

Examples are unit tests, domain integrated tests, and preliminary
presentation-layer test written by software developers and conducted either on
the developer's machine and/or in the feature integration pipeline (e.g. on pull
requests).

This guide is heavily reliant on FDA regulation ([General Principles of Software
Validation](https://www.fda.gov/regulatory-information/search-fda-guidance-documents/general-principles-software-validation))
and discusses practices to be applied by software developers when writing unit
and integration tests.

Introduction
============

Tests should be an integral part of the software development process, which
means that tests are written during software development and executed
continuously throughout the software development process. Software test plans
should identify the particular tasks to be conducted at each stage of
development and include justification of the level of effort represented by
their corresponding completion criteria.

The effort put into writing tests and the scope of edge cases these tests
account for should always take the [Level Of
Concern](https://www.fda.gov/media/73065/download) (LoC) if that particular
piece of code into account, a.k.a. "risk-based approach". Code whose failure
might implicate considerable harm to a user, or code which is critical for the
proper functioning of the application, must be tested thoroughly. Conversely,
code whose potential harm is negligible or that is not central to the proper
functioning of the application doesn't need to cover every edge case.

General Tenets
==============

Proper software (unit) testing should follow these tenets:

-   The expected test outcome is predefined.

-   A good test case has a high probability of exposing an error. Examining only
    the usual case is insufficient. Include edge cases and keep potential
    failures caused by future changes in mind.

-   A successful test is one that finds an error. Testing the obvious is not
    helpful.

-   Don't only test for what the application "should do" but also for what it
    "shouldn't do".

-   There is independence from coding. For example, a mere change in the
    implementation type should not break a test provided the logic produces the
    same result → use dependency injection and test against abstractions!

-   Both application (BA/PO/user) and software (architect/developer) expertise
    are employed. Always keep software quality criteria and overall system
    integration in mind.

-   Test documentation permits its reuse and independent confirmation of the
    pass/fail status of a test outcome during a subsequent review.

Levels of Testing
=================

In order to provide a thorough and rigorous examination of a software product,
development testing is typically organized into levels. As an example, a
software product's testing can be organized into unit, integration, and system
levels of testing.

Unit Tests
----------

Unit (module or component) level testing focuses on the early examination of
sub-program functionality and ensures that functionality not visible at the
system level is examined by testing. Unit testing ensures that quality software
units are furnished for integration into the finished software product.

The locus of responsibility for unit tests lies with the software developers,
ideally the ones who wrote the code being tested. This means that developers
represent the "central kernel" of the responsibility scope regarding unit tests.
Developers establish the implementational test cases, write, review, and
maintain unit tests. Other stakeholders influence unit test composition only
indirectly by means, such as establishing target metrics (see below) or
formulating requirements for edge cases.

Unit tests should be written continuously throughout the process of developing a
feature or bugfix. No feature or bugfix for which there are no unit tests yet
should be allowed to be merged back to *main* branches (develop, release,
hotfix). Exceptions apply to legacy code with the caveat that these
instances *must* be documented (including justification) and, if feasible, the
writing of unit tests for these cases must be scheduled for a future sprint.

Integration Tests
-----------------

Integration level testing focuses on the transfer of data and control across a
program's internal and external interfaces. External interfaces are those with
other software (including operating system software), system hardware, and the
users and can be described as communications links.

Because they rely on a complete and functioning component, integration tests are
typically written after the integration of a feature. Thus, they needn't be an
acceptance criterion for merging code back to the *main* branches (develop,
release, hotfix).

Integration tests should be written continuously as the software matures and new
features are introduced. The responsibility of writing integration tests lies
with both developers and QA.

It is also recommended to have the success of selected integration tests as an
acceptance criterion for pull-requests.

System-Level Tests
------------------

System-level testing demonstrates that all specified functionality exists and
that the software product is trustworthy. This testing verifies the as-built
program's functionality and performance with respect to the requirements for the
software product as exhibited on the specified operating platform(s). System
level software testing addresses functional and non-functional concerns such as
the following elements:

-   Performance issues (e.g., response times, reliability measurements)

-   Responses to stress conditions, e.g., behavior under maximum load,
    continuous use

-   Usability tests 

-   Operation of internal and external security features

-   Effectiveness of recovery procedures, including disaster recovery Usability 

-   Compatibility with other software products 

-   Behavior in each of the defined hardware configurations

-   Accuracy of documentation

System level testing also exhibits the software product's behavior in the
intended operating environment. The location of such testing is dependent upon
the software developer's ability to produce the target operating environment(s).
Depending upon the circumstances, simulation of hardware, environment or both
may be utilized. Test plans should identify the controls needed to ensure that
the intended coverage is achieved and that proper documentation is prepared when
planned system level testing is conducted at sites not directly controlled by
The organization developing the software (for example, when a PEN test is 
conducted at the site of the PEN-testing company).

The FDA requires that:

>   *Test procedures, test data, and test results should be documented in a
>   manner permitting objective pass/fail decisions to be reached. They should
>   also be suitable for review and objective decision making subsequent to
>   running the test, and they should be suitable for use in any subsequent
>   regression testing. Errors detected during testing should be logged,
>   classified, reviewed, and resolved prior to release of the software.
>   Software error data that is collected and analyzed during a development life
>   cycle may be used to determine the suitability of the software product for
>   release for commercial distribution. Test reports should comply with the
>   requirements of the corresponding test plans.*

Metrics
=======

The level of structural testing of a codebase is conventionally evaluated using
established
[metrics](https://docs.sonarqube.org/latest/user-guide/metric-definitions/)
which indicate how thoroughly the code is being tested. The metric most widely
known, and at the same time the metric which regulatory institutions such as the
FDA are most interested in is "code coverage". Quoting FDA guidelines:

>   *These metrics are typically referred to as "coverage" and are a measure of
>   completeness with respect to test selection criteria. The amount of
>   structural coverage should be commensurate with the level of risk posed by
>   the software.*

FDA & Metrics
-------------

The FDA differentiates between "white-box" and "black-box" testing. Below are
their definitions and respective metrics as proposed by the FDA.

### White-Box
White-Box testing refers to tests written against the code itself. The obvious
exemplars of "white-box" tests are unit tests. However, white-box tests also
include integration tests, especially when such tests evaluate the joined
functioning of two software components developed and managed by the company (e.g.
in-memory testing of co-dependent components from 2 different microservices).

The FDA defines white-box testing as follows:

>   *Code-based testing is also known as structural testing or "white-box"
>   testing. It identifies test cases based on knowledge obtained from the
>   source code, detailed design specification, and other development documents.
>   These test cases challenge the control decisions made by the program; and
>   the program's data structures including configuration tables. Structural
>   testing can identify "dead" code that is never executed when the program is
>   run. Structural testing is accomplished primarily with unit (module) level
>   testing, but can be extended to other levels of software testing.*

Writing white-box tests should be the responsibility of the software developers
working on the code being tested. Peers evaluating a pull-request should also
evaluate whether the unit tests are proper and sufficient.

The FDA defines the following metrics for white-box testing:

#### Statement Coverage
This criterion requires sufficient test cases for each program statement to
be executed at least once; however, its achievement is insufficient to
provide confidence in a software product's behavior. 

#### Decision (Branch) Coverage
This criterion requires sufficient test cases for each program decision or
branch to be executed so that each possible outcome occurs at least once. It
is considered to be a minimum level of coverage for most software products,
but decision coverage alone is insufficient for high-integrity applications.

#### Condition Coverage
This criterion requires sufficient test cases for each condition in a
program decision to take on all possible outcomes at least once. It differs
from branch coverage only when multiple conditions must be evaluated to
reach a decision.

#### Multi-Condition Coverage
This criterion requires sufficient test cases to exercise all possible
combinations of conditions in a program decision.

#### Loop Coverage
This criterion requires sufficient test cases for all program loops to be
executed for zero, one, two, and many iterations covering initialization,
typical running and termination (boundary) conditions.

#### Path Coverage
This criterion requires sufficient test cases for each feasible path, basis
path, etc., from start to exit of a defined program segment, to be executed
at least once. Because of the very large number of possible paths through a
software program, path coverage is generally not achievable. The amount of
path coverage is normally established based on the risk or criticality of
the software under test.

#### Data Flow Coverage
This criteria requires sufficient test cases for each feasible data flow to
be executed at least once. A number of data flow testing strategies are
available.

Software development should aim for a minimum of 80% overall coverage of the
entire code base. A pull-request should have 100% statement coverage. Lower
coverage percentages shall be justified and documented. 

### Black-Box
The FDA considers functional testing as "black-box" testing. That is, the
testing whether the macro-scale software module behaves in the expected manner,
resp. produces the expected result. In other words, they challenge the intended
use or functionality of a program, and the program's internal and external
interfaces.

Though mainly aimed at an integration level, black-box testing is not solely the
domain of QA and POs. The developers are expected to also write tests which
evaluate quality criteria and correctness at a functional level.

The FDA defines and recommends the following levels of effort for black-box
testing:

#### Normal Case
Testing with usual inputs is necessary. However, testing a software product
only with expected, valid inputs does not thoroughly test that software
product. By itself, normal case testing cannot provide sufficient confidence
in the dependability of the software product.

#### Output Forcing
Choosing test inputs to ensure that selected (or all) software outputs are
generated by testing.

#### Robustness
Software testing should demonstrate that a software product behaves
correctly when given unexpected, invalid inputs. Methods for identifying a
sufficient set of such test cases include Equivalence Class Partitioning,
Boundary Value Analysis, and Special Case Identification (Error Guessing) as
well as verifying error conditions (Error Forcing). While important and
necessary, these techniques do not ensure that all of the most appropriate
challenges to a software product have been identified for testing.

#### Combination of Inputs
The functional testing methods identified above all emphasize individual or
single test inputs. Most software products operate with multiple inputs
under their conditions of use. Thorough software product testing should
consider the combinations of inputs a software unit or system may encounter
during operation. Error guessing can be extended to identify combinations of
inputs, but it is an ad hoc technique. Cause-effect graphing is one
functional software testing technique that systematically identifies
combinations of inputs to a software product for inclusion in test cases.

Static Code Analysis
====================

The definitive coverage and testability metrics should arise from the selected
static code analysis (SA) tooling. SA must be automated and integrated into the
CI/CD pipeline. SA rulesets are chosen by a specialist in the targeted
technology stack (mostly an SD lead) in collaboration with QA and the software
architect. Code analysis is to run continuously and its reports to be archived.

Below is a compilation of recommendations involving SA from other professionals
working in the med-tech industry.

-   Employing SA: highly recommended, even though the use of SA is not strictly
    mandated by regulatory organs.

    -   Other major med-tech software and device companies were able to get IVD
        and FDA approval without SA. Be aware that these were
        also *older* projects (in Fortran, Delphi, Pascal). The consensus is
        that SA should be a requirement for newer projects, and it will increase
        your chances of getting approved.

    -   You are free to choose your preferred SA tooling.

    -   There is no specific regulation regarding which ruleset to use. Use the
        ones specific to a language/platform and recommended by the SA tool
        distributor / recommended by the community.

-   It is OK to disable rules as long as the disabling is documented with a
    justifiable rationale.

    -   When documenting disabled rulesets, remember to include potential risks
        involved with the disabling and what you have done to mitigate those
        risks.

    -   The evaluation of whether a rule should be disabled should be
        risk-based.

        -   The higher the risk, the less permissive you should be about
            disabling it.

    -   It is important to document the rulesets being applied.

        -   It is also very important that the documented rulesets
            are *really* applied ostensibly in the exact manner they have been
            documented: "document what you do, do what you document".

    -   Try to keep the rulesets consistent across repos. Don't enable something
        for one repo and disable it for another unless you can justify this
        discrepancy.

        -   This is not always possible, especially when enabling SA on a legacy
            repo which was developed without SA.

        -   Document ruleset discrepancies and their justification.

-   There are no strict requirements regarding specific
    SA [metrics](https://docs.sonarqube.org/latest/user-guide/metric-definitions/).
    Regulators are mostly interested in risk and vulnerability. Hence the most
    interesting metrics would relate to test coverage, reliability and
    vulnerability.

    -   On code coverage: application areas presenting a high risk to the patient must have a
        high density of line and condition coverage.

    -   On reliability: low number of new bugs, high reliability remediation
        effort.

    -   On security: important are vulnerabilities which could present a risk to
        the user's health or privacy.

-   Assign risk levels to the issues found by your SA tool, and have a clear,
    demonstrable process to include those issues on your development workflow
    and demonstrate that you are resolving those issues prioritized by risk.

    -   You need to be able to demonstrate by which processes you mitigate
        risks.

-   Document every "won't fix" with a risk assessment and justification.

Responsibilities
================

The responsibility for proper software testing is shared among many roles and
stakeholders, though the majority of it falls within the software development
teams and QA

Software Developers
-------------------

-   Write and review unit tests for developed modules during development and
    ensure required code coverage.

-   Write and review integration tests for developed modules during or after
    development.

-   Identify, document and report structural and functional test cases.

-   Evaluate which integration tests should be included as pull-request/merge
    criteria.

-   Resolve failing tests as they occur.

-   Maintain existing tests, evaluating their appropriateness and extending them
    as the codebase changes.

QA
--

-   Formulate test cases for- and implement functional, module encompassing unit
    tests (gray-box).

-   Formulate test cases for- and implement integration tests.

-   Collaborate with developers in determining the appropriate tests to be run
    as merge criteria.

-   Formulate test cases to be executed during nightly builds, releases, and
    hotfixes.

-   Ensure that test cases and results implemented by software developers are
    descriptive enough.

-   Collaborate with POs in formulating build acceptance tests.

-   Ensure that metrics are kept at an adequate level.

-   Collaborate with PSE to ensure that logging, test reports, and their
    troubleshooting directives are intelligible and sufficient.

Architect
---------

-   Ensure that technical requirements and software quality criteria are
    properly covered.

-   Define, communicate, and chaperon patterns and practices relating to
    software testing.

-   Define, communicate, and chaperon software patterns and practices which
    promote better testability.

-   Colaborate with SD, QA and DevOps in the selection of software testing tools
    in all levels (from mocking frameworks to chaos and stress tests).

DevOps
------

-   Provide and maintain the infrastructure necessary for conducting integration
    and system-level tests.

-   Ensure that the software is tested under different configurations.

-   Implement automated test execution in the CI/CD pipeline.

-   Ensure that test reports and builds are retained in accordance to their
    relevancy.

-   Ensure that test reports and artifacts are correctly versioned and
    correlated.

-   Collaborate with QA and SD in the maintenance of SA

PO
--

-   Define acceptance criteria and definition of done in regard to test results.

-   Collaborate with SD and QA to ensure the software functionality is properly
    tested.

-   Consider testability when designing use-cases and requirements.

Glossary
========

| **Term** | **Description**                                 |
|----------|-------------------------------------------------|
| SA       | Static code Analysis                            |
| QA       | Quality Assurance                               |
| CI/CD    | Continuous Integration / Continuous Delivery    |
| PR       | Pull request                                    |
| PO       | Product owner                                   |
| PSE      | Product Support Engineer                        |
| SD       | Software developer / Software development       |
| SW       | Software                                        |
| BAT      | Build Acceptance Test                           |
| PEN      | Penetration test                                |
| UX/UI    | User eXperience / User Interface                |
| LoC      | Level of Concern                                |
| BA       | Business Analyst                                |
| FDA      | Food and Drug Administration (regulatory organ) |
