# Clean ABAP

[**Internal**](#internal) &nbsp;·&nbsp; [**Optional**](#optional) &nbsp;·&nbsp;  [**Continuous Release**](#continuous-release) &nbsp;·&nbsp; [**Inner Source**](#inner-source) &nbsp;·&nbsp; [**Grassroots Project**](#grassroots-project)

This guide is an adoption of [Robert C. Martin's _Clean Code_] for ABAP.

## Content

- [About this guide](#about-this-guide)
  - [Internal](#internal)
  - [Optional](#optional)
  - [Continuous Release](#continuous-release)
  - [Inner Source](#inner-source)
  - [Grassroots Project](#grassroots-project)
- [How-to](#how-to)
  - [Start](#start)
  - [Refactor](#refactor)
  - [Comply](#comply)
  - [Disagree](#disagree)
- [Names](#names)
  - [Use descriptive names](#use-descriptive-names)
  - [Prefer solution domain and problem domain terms](#prefer-solution-domain-and-problem-domain-terms)
  - [Use plural](#use-plural)
  - [Use pronounceable names](#use-pronounceable-names)
  - [Avoid abbreviations](#avoid-abbreviations)
  - [Use same abbreviations everywhere](#use-same-abbreviations-everywhere)
  - [Use nouns for classes and verbs for methods](#use-nouns-for-classes-and-verbs-for-methods)
  - [Avoid noise words such as "data", "controller", "object"](#avoid-noise-words-such-as-data-controller-object)
  - [Pick one word per concept](#pick-one-word-per-concept)
  - [Use pattern names only if you mean them](#use-pattern-names-only-if-you-mean-them)
  - [Avoid encodings, esp. Hungarian notation and prefixes](#avoid-encodings-esp-hungarian-notation-and-prefixes)
    - [Reasoning](#reasoning)
    - [Arguments](#arguments)
    - [Compromises](#compromises)
- [Language](#language)
  - [Mind the legacy](#mind-the-legacy)
  - [Mind the performance](#mind-the-performance)
  - [Prefer object orientation over imperative programming](#prefer-object-orientation-over-imperative-programming)
  - [Prefer functional over procedural language constructs](#prefer-functional-over-procedural-language-constructs)
  - [Use design patterns wisely](#use-design-patterns-wisely)
- [Constants](#constants)
  - [Use constants instead of magic numbers](#use-constants-instead-of-magic-numbers)
  - [Prefer enumeration classes over constants interfaces](#prefer-enumeration-classes-over-constants-interfaces)
    - [Constant Pattern](#constant-pattern)
    - [Object Pattern](#object-pattern)
    - [Anti-Pattern](#anti-pattern)
    - [Benefits](#benefits)
  - [If you don't use enumeration classes, group your constants](#if-you-dont-use-enumeration-classes-group-your-constants)
- [Variables](#variables)
  - [Prefer inline over up-front declarations](#prefer-inline-over-up-front-declarations)
  - [Don't declare inline in optional branches](#dont-declare-inline-in-optional-branches)
  - [Do not chain up-front declarations](#do-not-chain-up-front-declarations)
  - [Prefer REF TO over FIELD-SYMBOL](#prefer-ref-to-over-field-symbol)
- [Tables](#tables)
  - [Use the right table type](#use-the-right-table-type)
  - [Avoid DEFAULT KEY](#avoid-default-key)
  - [Prefer INSERT INTO TABLE over APPEND TO](#prefer-insert-into-table-over-append-to)
  - [Prefer LINE_EXISTS over READ TABLE](#prefer-line_exists-over-read-table)
- [Strings](#strings)
  - [Use ` to define literals](#use--to-define-literals)
  - [Use | to assemble text](#use--to-assemble-text)
- [Booleans](#booleans)
  - [Use ABAP_BOOL for Booleans](#use-abap_bool-for-booleans)
  - [Use ABAP_TRUE and ABAP_FALSE for comparisons](#use-abap_true-and-abap_false-for-comparisons)
  - [Use XSDBOOL to set Boolean variables](#use-xsdbool-to-set-boolean-variables)
- [Conditions](#conditions)
  - [Try to make conditions positive](#try-to-make-conditions-positive)
  - [Consider decomposing complex conditions](#consider-decomposing-complex-conditions)
  - [Consider extracting complex conditions](#consider-extracting-complex-conditions)
- [Ifs](#ifs)
  - [No empty IF branches](#no-empty-if-branches)
  - [Prefer CASE to ELSE IF for multiple alternative conditions](#prefer-case-to-else-if-for-multiple-alternative-conditions)
  - [Keep the nesting depth low](#keep-the-nesting-depth-low)
- [Regular expressions](#regular-expressions)
  - [Prefer simpler methods to regular expressions](#prefer-simpler-methods-to-regular-expressions)
  - [Prefer basis checks to regular expressions](#prefer-basis-checks-to-regular-expressions)
  - [Consider assembling complex regular expressions](#consider-assembling-complex-regular-expressions)
- [Classes](#classes)
  - [Object orientation](#object-orientation)
    - [Prefer objects to static classes](#prefer-objects-to-static-classes)
    - [Prefer composition over inheritance](#prefer-composition-over-inheritance)
    - [Don't mix stateful and stateless in the same class](#dont-mix-stateful-and-stateless-in-the-same-class)
  - [Scope](#scope)
    - [Global by default, local only in exceptional cases](#global-by-default-local-only-in-exceptional-cases)
    - [FINAL if not designed for inheritance](#final-if-not-designed-for-inheritance)
    - [Members PRIVATE by default, PROTECTED only if needed](#members-private-by-default-protected-only-if-needed)
    - [Consider using immutable instead of getter](#consider-using-immutable-instead-of-getter)
    - [Use READ-ONLY sparingly](#use-read-only-sparingly)
  - [Constructors](#constructors)
    - [Prefer NEW over CREATE OBJECT](#prefer-new-over-create-object)
    - [If your global class is CREATE PRIVATE, leave the CONSTRUCTOR public](#if-your-global-class-is-create-private-leave-the-constructor-public)
    - [Prefer multiple static factory methods over optional parameters](#prefer-multiple-static-factory-methods-over-optional-parameters)
    - [Use descriptive names for multiple constructor methods](#use-descriptive-names-for-multiple-constructor-methods)
    - [Make singletons only where multiple instances don't make sense](#make-singletons-only-where-multiple-instances-dont-make-sense)
- [Methods](#methods)
  - [Calls](#calls)
    - [Prefer functional over procedural calls](#prefer-functional-over-procedural-calls)
    - [Omit RECEIVING](#omit-receiving)
    - [Omit the optional keyword EXPORTING](#omit-the-optional-keyword-exporting)
    - [Omit the parameter name in single parameter calls](#omit-the-parameter-name-in-single-parameter-calls)
  - [Object orientation](#object-orientation-1)
    - [Prefer instance to static methods](#prefer-instance-to-static-methods)
    - [Public instance methods should be part of an interface](#public-instance-methods-should-be-part-of-an-interface)
  - [Parameter Number](#parameter-number)
    - [Aim for few IMPORTING parameters, at best less than three](#aim-for-few-importing-parameters-at-best-less-than-three)
    - [Split methods instead of adding OPTIONAL parameters](#split-methods-instead-of-adding-optional-parameters)
    - [Use PREFERRED PARAMETER sparingly](#use-preferred-parameter-sparingly)
    - [RETURN, EXPORT, or CHANGE exactly one parameter](#return-export-or-change-exactly-one-parameter)
  - [Parameter Types](#parameter-types)
    - [Prefer RETURNING over EXPORTING](#prefer-returning-over-exporting)
    - [RETURNING large tables is usually okay](#returning-large-tables-is-usually-okay)
    - [Use either RETURNING or EXPORTING or CHANGING, but not a combination](#use-either-returning-or-exporting-or-changing-but-not-a-combination)
    - [Use CHANGING sparingly, where suited](#use-changing-sparingly-where-suited)
    - [Split method instead of Boolean input parameter](#split-method-instead-of-boolean-input-parameter)
  - [Parameter Names](#parameter-names)
    - [Consider calling the RETURNING parameter RESULT](#consider-calling-the-returning-parameter-result)
  - [Parameter Initialization](#parameter-initialization)
    - [Clear or overwrite EXPORTING reference parameters](#clear-or-overwrite-exporting-reference-parameters)
    - [Don't clear VALUE parameters](#dont-clear-value-parameters)
  - [Method Body](#method-body)
    - [Do one thing, do it well, do it only](#do-one-thing-do-it-well-do-it-only)
    - [Focus on the happy path or error handling, but not both](#focus-on-the-happy-path-or-error-handling-but-not-both)
    - [Descend one level of abstraction](#descend-one-level-of-abstraction)
    - [Keep methods small](#keep-methods-small)
  - [Control flow](#control-flow)
    - [Fail fast](#fail-fast)
    - [CHECK or RETURN for input validation at method start](#check-or-return-for-input-validation-at-method-start)
    - [Avoid CHECK in other positions](#avoid-check-in-other-positions)
- [Error Handling](#error-handling)
  - [Return Codes](#return-codes)
    - [Prefer exceptions to return codes](#prefer-exceptions-to-return-codes)
    - [Don't let failures slip through](#dont-let-failures-slip-through)
  - [Exceptions](#exceptions)
    - [Exceptions are for errors, not for regular cases](#exceptions-are-for-errors-not-for-regular-cases)
    - [Use class-based exceptions](#use-class-based-exceptions)
  - [Throwing](#throwing)
    - [Use own super classes](#use-own-super-classes)
    - [Throw one type of exception](#throw-one-type-of-exception)
    - [Use sub-classes to enable callers to distinguish error situations](#use-sub-classes-to-enable-callers-to-distinguish-error-situations)
    - [Throw CX_STATIC_CHECK for manageable exceptions](#throw-cx_static_check-for-manageable-exceptions)
    - [Throw CX_NO_CHECK for usually unrecoverable situations](#throw-cx_no_check-for-usually-unrecoverable-situations)
    - [Consider CX_DYNAMIC_CHECK for avoidable exceptions](#consider-cx_dynamic_check-for-avoidable-exceptions)
    - [Dump for totally unrecoverable situations](#dump-for-totally-unrecoverable-situations)
    - [Prefer RAISE EXCEPTION NEW to RAISE EXCEPTION TYPE](#prefer-raise-exception-new-to-raise-exception-type)
  - [Catching](#catching)
    - [Wrap foreign exceptions instead of letting them invade your code](#wrap-foreign-exceptions-instead-of-letting-them-invade-your-code)
- [Comments](#comments)
  - [Express yourself in code, not in comments](#express-yourself-in-code-not-in-comments)
  - [Comments are no excuse for bad names](#comments-are-no-excuse-for-bad-names)
  - [Use methods instead of comments to segment your code](#use-methods-instead-of-comments-to-segment-your-code)
  - [Write comments to explain the why, not the what](#write-comments-to-explain-the-why-not-the-what)
  - [Design goes into the design documents, not the code](#design-goes-into-the-design-documents-not-the-code)
  - [Comment with ", not with *](#comment-with--not-with-)
  - [Put comments before the statement they relate to](#put-comments-before-the-statement-they-relate-to)
  - [Delete code instead of commenting it](#delete-code-instead-of-commenting-it)
  - [Use FIXME, TODO, and XXX and add your ID](#use-fixme-todo-and-xxx-and-add-your-id)
  - [Don't add method signature and end-of comments](#dont-add-method-signature-and-end-of-comments)
  - [Don't duplicate message texts as comments](#dont-duplicate-message-texts-as-comments)
  - [ABAP Doc only for public APIs](#abap-doc-only-for-public-apis)
- [Formatting](#formatting)
  - [Optimize for reading, not for writing](#optimize-for-reading-not-for-writing)
  - [Use the Pretty Printer before activating](#use-the-pretty-printer-before-activating)
  - [Use your Pretty Printer team settings](#use-your-pretty-printer-team-settings)
    - [Upper and lower case](#upper-and-lower-case)
  - [No more than one statement per line](#no-more-than-one-statement-per-line)
  - [Stick to a reasonable line length](#stick-to-a-reasonable-line-length)
  - [Condense your code](#condense-your-code)
  - [Add a single blank line to separate things, but not more](#add-a-single-blank-line-to-separate-things-but-not-more)
  - [Don't obsess with separating blank lines](#dont-obsess-with-separating-blank-lines)
  - [Downside of alignment](#downside-of-alignment)
  - [Align assignments to the same object, but not to different ones](#align-assignments-to-the-same-object-but-not-to-different-ones)
  - [Close brackets at line end](#close-brackets-at-line-end)
  - [Keep single parameter calls on one line](#keep-single-parameter-calls-on-one-line)
  - [Keep parameters behind the call](#keep-parameters-behind-the-call)
  - [If you break, indent parameters under the call](#if-you-break-indent-parameters-under-the-call)
  - [Line-break multiple parameters](#line-break-multiple-parameters)
  - [Align parameters](#align-parameters)
  - [Break the call to a new line if the line gets too long](#break-the-call-to-a-new-line-if-the-line-gets-too-long)
  - [Indent and snap to tab](#indent-and-snap-to-tab)
  - [Indent in-line declarations like method calls](#indent-in-line-declarations-like-method-calls)
  - [Don't align type clauses](#dont-align-type-clauses)
- [Testing](#testing)
  - [Principles](#principles)
    - [Write testable code](#write-testable-code)
    - [Enable others to mock you](#enable-others-to-mock-you)
    - [Readability rules](#readability-rules)
    - [Don't make copies or write test reports](#dont-make-copies-or-write-test-reports)
    - [Test publics, not private internals](#test-publics-not-private-internals)
    - [Don't obsess about coverage](#dont-obsess-about-coverage)
  - [Test Classes](#test-classes)
    - [Call local test classes by their purpose](#call-local-test-classes-by-their-purpose)
    - [Put tests in local classes](#put-tests-in-local-classes)
    - [Where to put test classes](#where-to-put-test-classes)
    - [How to execute test classes](#how-to-execute-test-classes)
  - [Code Under Test](#code-under-test)
    - [Name the code under test meaningfully, or default to CUT](#name-the-code-under-test-meaningfully-or-default-to-cut)
    - [Test interfaces, not classes](#test-interfaces-not-classes)
    - [Extract the call to the code under test to its own method](#extract-the-call-to-the-code-under-test-to-its-own-method)
  - [Injection](#injection)
    - [Use dependency inversion to inject test doubles](#use-dependency-inversion-to-inject-test-doubles)
    - [Use CL_ABAP_TESTDOUBLE](#use-cl_abap_testdouble)
    - [Exploit the test tools](#exploit-the-test-tools)
    - [Use test seams as temporary workaround](#use-test-seams-as-temporary-workaround)
    - [Use LOCAL FRIENDS to access the dependency-inverting constructor](#use-local-friends-to-access-the-dependency-inverting-constructor)
    - [Don't misuse LOCAL FRIENDS to invade the tested code](#dont-misuse-local-friends-to-invade-the-tested-code)
    - [Don't change the productive code to make the code testable](#dont-change-the-productive-code-to-make-the-code-testable)
    - [Don't sub-class to mock methods](#dont-sub-class-to-mock-methods)
    - [Don't mock stuff that's not needed](#dont-mock-stuff-thats-not-needed)
    - [Don't build test frameworks](#dont-build-test-frameworks)
  - [Test Methods](#test-methods)
    - [Test method names: reflect what's given and expected](#test-method-names-reflect-whats-given-and-expected)
    - [Use given-when-then](#use-given-when-then)
    - ["When" is exactly one call](#when-is-exactly-one-call)
    - [Don't add a TEARDOWN unless you really need it](#dont-add-a-teardown-unless-you-really-need-it)
  - [Test Data](#test-data)
    - [Make it easy to spot meaning](#make-it-easy-to-spot-meaning)
    - [Make it easy to spot differences](#make-it-easy-to-spot-differences)
    - [Use constants to describe purpose and importance of test data](#use-constants-to-describe-purpose-and-importance-of-test-data)
  - [Assertions](#assertions)
    - [Few, focused assertions](#few-focused-assertions)
    - [Use the right assert type](#use-the-right-assert-type)
    - [Assert content, not quantity](#assert-content-not-quantity)
    - [Assert quality, not content](#assert-quality-not-content)
    - [Use FAIL to check for expected exceptions](#use-fail-to-check-for-expected-exceptions)
    - [Forward unexpected exceptions instead of catching and failing](#forward-unexpected-exceptions-instead-of-catching-and-failing)
    - [Write custom asserts to shorten code and avoid duplication](#write-custom-asserts-to-shorten-code-and-avoid-duplication)

## About this guide

> [Clean ABAP](#clean-abap) > [Content](#content)

### Internal

> [Clean ABAP](#clean-abap) > [Content](#content) > [About this guide](#about-this-guide)

This document is **internal**, as described in our [SAP Global Information Classification & Handling Standard](https://wiki.wdf.sap.corp/wiki/pages/viewpage.action?pageId=1891700683), meaning you can share it with any SAP employee but should not disclose it to non-employees.

We don't see a general obstacle to sharing this information with outsiders.
However, [it has been pointed out](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode/issues/27) that this guide partly contradicts other guides.
Customers and partners could be confused by this seeming violation of SAP's One Voice principle.

### Optional

> [Clean ABAP](#clean-abap) > [Content](#content) > [About this guide](#about-this-guide)

Following this guide is **optional**, meaning you - or more precisely: your team - can choose whether you want to adhere to it.

This document focuses on content, not process.
Our aim is to find the cleanest possible way to code ABAP, not to enforce a new gold standard for code reviews.

The way you deal with this guide is up to you.
You are free to make this guide mandatory for your team or unit, but we will not force you to.
However, we suggest that you at least take this guide and discuss it with your co-developers.

### Continuous Release

> [Clean ABAP](#clean-abap) > [Content](#content) > [About this guide](#about-this-guide)

This guide is updated **continuously**, meaning any change is reviewed and immediately put "live", without special publication versions.

ABAP and our understanding of Clean Code evolve, and this document will evolve along.
As a consequence, this document is "work in progress" and will probably never see a status "finished".
As agile developers, we welcome this.

### Inner Source

> [Clean ABAP](#clean-abap) > [Content](#content) > [About this guide](#about-this-guide)

This guide is **inner source**, meaning it is written by a loose community of interested persons, and anybody is invited to contribute.

You can contribute by posting and answering questions in the SAP-wide [Slack channel #sap-dev-ase-abap](https://slack.com/app_redirect?channel=sap-dev-ase-abap).
[Create an issue](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode/issues/new) to challenge a specific topic.
[Pull requests](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode/compare) are most welcome!

The [contributors page of this repository](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode/graphs/contributors) lists all authors and their individual contributions.
We are developers, architects, quality engineers, and consultants, from associates to chief experts, from language creators to tool developers, from S/4HANA to the foundation.
We respect all roles, ranks, and units, and welcome any suggestions and improvements.

### Grassroots Project

> [Clean ABAP](#clean-abap) > [Content](#content) > [About this guide](#about-this-guide)

This guide is a **grassroots project**, meaning it was started and is still driven by programmers who spend their day coding, and want to get better at it.

In contrast meaning this was not created in response to a management goal, CTO Circle work-stream, or S/4HANA committee.
We were neither officially tasked to produce a new ABAP programming guideline nor do we pursue the aim to install a new Checkman standard.
We are simply coders who love to code and want to share their insights.

## How-to

> [Clean ABAP](#clean-abap) > [Content](#content)

### Start

> [Clean ABAP](#clean-abap) > [Content](#content) > [How-to](#how-to)

If you are new to Clean Code, the [Clean Code Developer initiative](https://clean-code-developer.com/) may help you getting started with a didactically smooth stepwise introduction to the topic in general.

We recommend you to start with things that are easily understood and broadly accepted, such as [Booleans](#booleans), [Conditions](#conditions), and [Ifs](#ifs).

You will probably benefit most from the section [Methods](#methods), especially [Do one thing, do it well, do it only](#do-one-thing-do-it-well-do-it-only) and [Small](#small), because these tremendously improve the overall structure of your code.

Some topics in here can spark difficult discussions in teams that are experienced in what they do but new to Clean Code; these topics are perfectly "healthy", but people may have problems making themselves comfortable with them in the beginning.

Continue to these more controversial topics later; especially [Comments](#comments), [Naming](#naming), and [Formatting](#formatting) can lead to near-religious disputes and should only be addressed by teams that already saw proof of Clean Code's positive effects.

### Refactor

> [Clean ABAP](#clean-abap) > [Content](#content) > [How-to](#how-to)

The topics [Booleans](#booleans), [Conditions](#conditions), [Ifs](#ifs), and [Methods](#methods) are most rewarding if you are working on a legacy project with tons of code that you cannot or do not want to change: they can be applied to new code without conflicts, and may be applied to old code following the boy scout rule.

The topic [Naming](#naming) is very demanding for legacy projects, as it may introduce a breach between old and new code, up to a degree where sections like [Avoid encodings, esp. Hungarian notation and prefixes](#avoid-encodings-esp-hungarian-notation-and-prefixes) are better ignored.

### Comply

> [Clean ABAP](#clean-abap) > [Content](#content) > [How-to](#how-to)

Our guide follows the _spirit_ of Clean Code, meaning we adjusted some things to the ABAP programming language e.g. [Throw CX_STATIC_CHECK for manageable exceptions](#throw-cx_static_check-for-manageable-exceptions).

Some facts are from the [ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenabap_pgl.htm), which this guide is mostly compatible to; deviations are indicated and always in the spirit of cleaner code.
This guide also respects the [DSAG's Recommendations for ABAP Development](https://www.dsag.de/sites/default/files/dsag_recommendation_abap_development.pdf), although we are more precise in most details.

This document is a rework of the original [SAP Assurance and Compliance _ABAP Style Guide_](https://wiki.wdf.sap.corp/wiki/display/ACS/ABAP+Style+Guide), which in turn was distilled from many many years of ABAP experience and thorough code reviews.
We rewrote it to work for _all_ ABAP developers by omitting our team-specific conventions and skipping code like `cl_fra_log_factory` that is not available to everyone.

### Disagree

> [Clean ABAP](#clean-abap) > [Content](#content) > [How-to](#how-to)

We wrote this style guide for readers who are already acquainted with Clean Code or who are right now working on that, with a strong focus on how to apply Clean Code _specifically to ABAP_.

Please mind that we therefore did not introduce all concepts in the same length and depth as the original book and related resources: these are still worth a read, especially if you disagree with things in here just because we didn't explain them very well.
Use the links in the sections to read up on the background of this guidance.

You are free to discuss and disagree with anything we say here.
One of the pillars of Clean Code is that _the team rules_.
Just be sure to give things a fair chance before you discard them.

## Names

> [Clean ABAP](#clean-abap) > [Content](#content)

### Use descriptive names

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

Use names that convey the content and meaning of things.

```ABAP
CONSTANTS max_wait_time_in_seconds TYPE i ...
DATA customizing_entries TYPE STANDARD TABLE ...
METHODS read_user_preferences ...
CLASS /clean/user_preference_reader ...
```

Do not focus on the data type or technical encoding.
They hardly contribute to understanding the code.

```ABAP
" anti-pattern
CONSTANTS sysubrc_04 TYPE sysubrc ...
DATA iso3166tab TYPE STANDARD TABLE ...
METHODS read_t005 ...
CLASS /dirty/t005_reader ...
```

[Do not attempt to fix bad names by comments.](#comments-are-no-excuse-for-bad-names)

> Read more in _Chapter 2: Meaningful Names: Use Intention-Revealing Names_ of [Robert C. Martin's _Clean Code_].

### Prefer solution domain and problem domain terms

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

Search for good names in the solution domain, i.e. computer science terms such as "queue" or "tree", and in the problem domain, i.e. business field terms such as "account" or "ledger".

Layers that are business-like will sound best when named according to the problem domain.
This is especially true for components that are design with Domain-Driven Design, such as APIs and business objects.

Layers that provide mostly technical functionality, such as factory classes and abstract algorithm, will sound best when named according to the solution domain.

In any case, do not attempt to make up your own language.
You need to be able to exchange information with your co-developers, product owners, and potentially customers, so choose names they can relate to without a customized dictionary.

> Read more in _Chapter 2: Meaningful Names: Use Solution Domain Names_ and _[...]: Use Problem Domain Names_ of [Robert C. Martin's _Clean Code_].

### Use plural

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

There is a legacy practice at SAP to name tables of things in singular, for example `country` for a "table of countries".
Common tendency in the outside world is to use the plural for lists of things.
We therefore recommend to prefer `countries` instead.

> Read more in _Chapter 2: Meaningful Names: Use Intention-Revealing Names_ of [Robert C. Martin's _Clean Code_].

### Use pronounceable names

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

We think and talk a lot about objects, so use names that you can pronounce, for example prefer `detection_object_types` to something cryptic like `dobjt`.

> Read more in _Chapter 2: Meaningful Names: Use Pronounceable Names_ of [Robert C. Martin's _Clean Code_]

### Avoid abbreviations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

If you have enough space, write out names in full.
Start abbreviating only if you exceed length limitations.

If you do have to abbreviate, start with the _unimportant_ words.

Abbreviating things may appear efficient at first glance, but becomes ambiguous very fast.
For example, does the "cust" in `cust` mean "customizing", "customer", or "custom"?
All three are common in SAP applications.

> Read more in _Chapter 2: Meaningful Names: Make Meaningful Distinctions_ of [Robert C. Martin's _Clean Code_].

### Use same abbreviations everywhere

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

People will search for keywords to find relevant code.
Support this by using the same abbreviation for the same thing.
For example, always abbreviate "detection object type" to "dobjt" instead of mixing "dot", "dotype", "detobjtype" and so on.

> Read more in _Chapter 2: Meaningful Names: Use Searchable Names_ of [Robert C. Martin's _Clean Code_].

### Use nouns for classes and verbs for methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

Use nouns or noun phrases to name classes, interfaces, and objects:

```ABAP
CLASS /clean/account
CLASS /clean/user_preferences
INTERFACE /clean/customizing_reader
```

Use verbs or verb phrases to name methods:

```ABAP
METHODS withdraw
METHODS add_message
METHODS read_entries
```

Starting Boolean methods with verbs like `is_` and `has_` yields nice reading flow:

```ABAP
IF is_empty( table ).
```

We recommend naming functions like methods:

```ABAP
FUNCTION /clean/read_alerts
```

### Avoid noise words such as "data", "controller", "object"

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

Omit noise words

    account  " instead of account_data
    alert    " instead of: alert_manager

or replace them with something specific that really adds value

    user_preferences          " instead of user_info
    response_time_in_seconds  " instead of response_time_variable

Classical noise words are

- 001, 002, ...
- abap, action, adapt, appl, calculate, class, constant, controller
- do, data, exec, execute, field, function, handler, info
- manager, method, object, perform, pre, process, sap, trigger, variable

This does not mean they are always noise, they only have a strong tendency.

> Read more in _Chapter 2: Meaningful Names: Make Meaningful Distinctions_ of [Robert C. Martin's _Clean Code_]

### Pick one word per concept

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

```ABAP
METHODS read_this.
METHODS read_that.
METHODS read_those.
```

Choose a term for a concept and stick to it; don't mix in other synonyms.
Synonyms will make the reader waste time on finding a difference that's not there.

```ABAP
" anti-pattern
METHODS read_this.
METHODS retrieve_that.
METHODS query_those.
```

> Read more in _Chapter 2: Meaningful Names: Pick One Word per Concept_ of [Robert C. Martin's _Clean Code_]

### Use pattern names only if you mean them

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

Don't use the names of software design patterns for classes and interfaces unless you really mean them.
For example, don't call your class `file_factory` unless it really implements the factory design pattern.
The most common patterns include: singleton, factory, facade, composite, decorator, iterator, observer, and strategy.

> Read more in _Chapter 2: Meaningful Names: Avoid Disinformation_ of [Robert C. Martin's _Clean Code_]

### Avoid encodings, esp. Hungarian notation and prefixes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names)

We encourage you to get rid of _all_ encoding prefixes.

```ABAP
METHOD add_two_numbers.
  result = a + b.
ENDMETHOD.
```

instead of the needlessly longer

```ABAP
METHOD add_two_numbers.
  rv_result = iv_a + iv_b.
ENDMETHOD.
```

> Read more in _Chapter 2: Meaningful Names: Avoid Encodings_ of [Robert C. Martin's _Clean Code_]. The examples in this document are written without prefixes to demonstrate the value.

#### Reasoning

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names) > [Avoid encodings, esp. Hungarian notation and prefixes](#avoid-encodings-esp-hungarian-notation-and-prefixes)

SAP has a bad, pervasive legacy practice of adding prefixes to each and everything, to encode things like

- kind, such as "cl_" for classes,
- direction, such as "is_" for an input parameter,
- scope, such as "mo_" for a class member,
- type, such as "lt_" for a table-like variable, and
- mutability, such as "sc_" for a constant.

This kind of prefixing is a relic from the early days of programming, when code was printed out and read on paper, and you didn't want to flip around just to find some variable's type.
Modern development environments give easy access to data types, signatures, and object navigation, such that it is no longer needed to get readable code.

#### Arguments

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names) > [Avoid encodings, esp. Hungarian notation and prefixes](#avoid-encodings-esp-hungarian-notation-and-prefixes)

Before you disagree, consider these:

- ABAP's 30 character limitation makes it hard enough to squeeze meaningful names into the available space without wasting another 3-4 characters for needless encodings.
- The disputes that arise over prefixes are not worth the effort: whether your constant starts with `sc_` or `cv_` does not really influence readability.
- Different team styles create confusion: is "lr_" an object reference or a range table?
  You'll stumble over this in code that connects different things, for example your determinations within BOPF.
- Changes create needless work: turning a table from `STANDARD` to `SORTED` shouldn't require you to rename all variables from "lt_" to "lts_".
- Prefixing doesn't make it easier to tell global from local things.
  If you fill a `gt_sum` from an `lt_sum`, both are still only sums and it's not clear what distinguishes the two.
  The better idea is to fill a `total_sum` from a `partial_sum`, or an `overall_result` from an `intermediate_result`.
  The name confusion described in [section _Program-Internal Names_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenprog_intern_names_guidl.htm) should thus be solved otherwise.
- If you follow Clean Code, your methods will become so short (3-5 statements) that prefixing is no longer necessary to tell importing from exporting parameters and local from global variables.
- The ABAP foundation doesn't prefix anymore, for example you won't find importing/exporting prefixes on the method parameters in `cl_abap_math`.
- Other languages like Java use absolutely no prefixes, and still Java code is perfectly readable.

#### Compromises

> [Clean ABAP](#clean-abap) > [Content](#content) > [Names](#names) > [Avoid encodings, esp. Hungarian notation and prefixes](#avoid-encodings-esp-hungarian-notation-and-prefixes)

There is only one prefix that ABAP forces you to use: your application's namespace, to avoid conflicts with objects from other teams in the global dictionary, where every thing needs a unique name.

If this rule is too hard for you, consider a compromise: avoid encodings in local contexts (within a method body, method parameters, local classes, etc.), and apply them only to global objects that are stored in the same global Dictionary namespace.

We agree that following this suggestion will work out only if the code is already _clean_ in some other aspects, especially short methods and good method and variable names.
While prefixes needlessly complicate a clean method with two statements, they may be your only remaining lifeline in a thousand-line legacy function with cryptic variable names.

> Read more in _Chapter 2: Meaningful Names: Avoid Encodings_ of [Robert C. Martin's _Clean Code_]. This section contradicts the sections [_Names of Repository Objects_](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenexit_procedure_guidl.htm) and [_Program-Internal Names_](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenexit_procedure_guidl.htm) of the ABAP Programming Guidelines which recommend to use prefixes. We think that avoiding prefixes is the more modern and readable variant and that the guideline should be adjusted.

## Language

> [Clean ABAP](#clean-abap) > [Content](#content)

### Mind the legacy

> [Clean ABAP](#clean-abap) > [Content](#content) > [Language](#language)

If you code for older ABAP releases, take the advice in this guide with care:
Many recommendations below make use of relatively new syntax and constructs that may not be supported in older ABAP releases.
Validate the guidelines you want to follow on the oldest release you must support.
Do not simply discard Clean Code as a whole - the vast majority of rules (e.g. naming, commenting) will work in _any_ ABAP version.

### Mind the performance

> [Clean ABAP](#clean-abap) > [Content](#content) > [Language](#language)

If you code high performance components, take the advice in this guide with care:
Some aspects of Clean Code may make things slower (more method calls) or consume more memory (more objects).
ABAP has some specialties that may intensify this, for example it compares data types when calling a method, such that splitting a single large method into many sub-methods may make the code slower.

However, we strongly recommend to not optimize prematurely, based on obscure fears.
The vast majority of rules (e.g. naming, commenting) has no negative impact at all.
Try to build things in a clean, object-oriented way.
If something is too slow, make a performance measurement.
Only then should you take a fact-based decision to discard selected rules.

### Prefer object orientation over imperative programming

> [Clean ABAP](#clean-abap) > [Content](#content) > [Language](#language)

Object-oriented programs (classes, interfaces) are segmented better and can be refactored and tested more easily than imperative code (functions, programs).
Although there are situations where you must provide imperative objects (a function for an RFC, a program for a transaction), these objects should do little more than call a corresponding class that provides the actual feature:

```ABAP
FUNCTION check_business_partner [...].
  DATA(validator) = NEW /clean/biz_partner_validator( ).
  result = validator->validate( business_partners ).
ENDFUNCTION.
```

### Prefer functional over procedural language constructs

> [Clean ABAP](#clean-abap) > [Content](#content) > [Language](#language)

They are usually shorter and come more natural to modern programmers.

> More [modern functional and expression based ABAP language constructs](modernABAP.md) supporting clean code

```ABAP
DATA(variable) = 'A'.
" MOVE 'A' TO variable.

DATA(uppercase) = to_upper( lowercase ).
" TRANSLATE lowercase TO UPPER CASE.

index += 1.         " >= NW 7.54
index = index + 1.  " < NW 7.54
" ADD 1 TO index.

DATA(object) = NEW /clean/my_class( ).
" CREATE OBJECT object TYPE /dirty/my_class.

result = VALUE #( FOR row IN input ( row-text ) ).
" LOOP AT input INTO DATA(row).
"  INSERT row-text INTO TABLE result.
" ENDLOOP.

DATA(line) = value_pairs[ name = 'A' ].
" READ TABLE value_pairs INTO DATA(line) WITH KEY name = 'A'.

DATA(exists) = xsdbool( line_exists( value_pairs[ name = 'A' ] ) ).
IF line_exists( value_pairs[ name = 'A' ] ).
" READ TABLE value_pairs TRANSPORTING NO FIELDS WITH KEY name = 'A'.
" DATA(exists) = xsdbool( sy-subrc = 0 ).
```

Many of the detailed rules below are just specific reiterations of this general advice.

### Use design patterns wisely

> [Clean ABAP](#clean-abap) > [Content](#content) > [Language](#language)

Where they are appropriate and provide noticeable benefit.
Don't apply design patterns everywhere just for the sake of it.

## Constants

> [Clean ABAP](#clean-abap) > [Content](#content)

### Use constants instead of magic numbers

> [Clean ABAP](#clean-abap) > [Content](#content) > [Constants](#constants)

```ABAP
IF abap_type = cl_abap_typedescr=>typekind_date.
```

is clearer than

```ABAP
" anti-pattern
IF abap_type = 'D'.
```

> Read more in _Chapter 17: Smells and Heuristics: G25: Replace Magic Numbers with Named Constants_ of [Robert C. Martin's _Clean Code_].

### Prefer enumeration classes over constants interfaces

> [Clean ABAP](#clean-abap) > [Content](#content) > [Constants](#constants)

There are two common patterns that reflect enumerations.

#### Constant Pattern

> [Clean ABAP](#clean-abap) > [Content](#content) > [Constants](#constants) > [Prefer enumeration classes over constants interfaces](#prefer-enumeration-classes-over-constants-interfaces)

```ABAP
CLASS /clean/message_severity DEFINITION PUBLIC ABSTRACT FINAL.
  PUBLIC SECTION.
    CONSTANTS:
      warning TYPE symsgty VALUE 'W',
      error   TYPE symsgty VALUE 'E'.
ENDCLASS.

CLASS /clean/message_severity IMPLEMENTATION.
ENDCLASS.
```

```ABAP
IF log_contains( /clean/message_severity=>warning ).
```

#### Object Pattern

> [Clean ABAP](#clean-abap) > [Content](#content) > [Constants](#constants) > [Prefer enumeration classes over constants interfaces](#prefer-enumeration-classes-over-constants-interfaces)

```ABAP
CLASS /clean/message_severity DEFINITION PUBLIC CREATE PRIVATE FINAL.
  PUBLIC SECTION.
    CLASS-DATA:
      warning TYPE REF TO /clean/message_severity READ-ONLY,
      error   TYPE REF TO /clean/message_severity READ-ONLY.
    CLASS-METHODS class_constructor.
    METHODS constructor IMPORTING value TYPE /clean/severity.
    METHODS equals
      IMPORTING
        value         TYPE symsgty
      RETURNING
        VALUE(result) TYPE abap_bool.
    DATA value TYPE symsgty READ-ONLY.
ENDCLASS.

CLASS /clean/message_severity IMPLEMENTATION.

  METHOD class_constructor.
    warning = NEW /clean/message_severity( 'W' ).
    error = NEW /clean/message_severity( 'E' ).
  ENDMETHOD.

  METHOD constructor.
    me->value = value.
  ENDMETHOD.

  METHOD equals.
    result = xsdbool( value = me->value ).
  ENDMETHOD.

ENDCLASS.
```

```ABAP
METHODS to_string IMPORTING severity TYPE REF TO /clean/message_severity.
IF /clean/message_severity=>warning->equals( worst_severity ).
object-severity = /clean/message_severity=>warning->value.
```

#### Anti-Pattern

> [Clean ABAP](#clean-abap) > [Content](#content) > [Constants](#constants) > [Prefer enumeration classes over constants interfaces](#prefer-enumeration-classes-over-constants-interfaces)

```ABAP
" anti-pattern
INTERFACE /dirty/common_constants.
  CONSTANTS:
    BEGIN OF message_severity,
      warning TYPE symsgty VALUE 'W',
      error   TYPE symsgty VALUE 'E',
    END OF message_severity.
ENDINTERFACE.
```

#### Benefits

> [Clean ABAP](#clean-abap) > [Content](#content) > [Constants](#constants) > [Prefer enumeration classes over constants interfaces](#prefer-enumeration-classes-over-constants-interfaces)

- An enumeration class groups all possible values for a data type at one glance.
- In contrast to the ABAP statement `ENUM`, these constants can be used on all levels, including data exchange with the database.
- Enumeration classes improve cohesion because you depend only on those constants that you need, not everything in a large interface.
- Enumeration classes can be found by their name via the data dictionary, using _Display other object..._ in SE24 and SE80 or _Ctrl+Shift+A_ in ADT.
- The `FINAL` and `ABSTRACT` prevents people from "inheriting" or "implementing" the constants list, which would sacrifice cohesion for a slightly shorter syntax.
- You can add type-related methods such as conversions, validation, etc. to the enumeration class.
- You can add unit tests to the enumeration, for example to assert that it's still in sync with the fixed values of its underlying DDIC Domain.
- The object-oriented pattern comes with value-safety, meaning it is not possible to provide a value that's not contained in the enumeration.

> Read more in _Chapter 17: Smells and Heuristics: J3: Constants versus Enums_ of [Robert C. Martin's _Clean Code_].

### If you don't use enumeration classes, group your constants

> [Clean ABAP](#clean-abap) > [Content](#content) > [Constants](#constants)

If you collect constants in a loose way, for example in an interface, group them:

```ABAP
CONSTANTS:
  BEGIN OF message_severity,
    warning TYPE symsgty VALUE 'W',
    error   TYPE symsgty VALUE 'E',
  END OF message_severity,
  BEGIN OF message_lifespan,
    transitional TYPE i VALUE 1,
    persisted    TYPE i VALUE 2,
  END OF message_lifespan.
```

Makes the relation clearer than:

```ABAP
CONSTANTS:
  warning      TYPE symsgty VALUE 'W',
  transitional TYPE i       VALUE 1,
  error        TYPE symsgty VALUE 'E',
  persisted    TYPE i       VALUE 2,
```

The group also allows you group-wise access, for example for input validation:

```ABAP
DO number_of_constants TIMES.
  ASSIGN COMPONENT sy-index OF STRUCTURE message_severity TO FIELD-SYMBOL(<constant>).
  IF <constant> = input.
    is_valid = abap_true.
    RETURN.
  ENDIF.
ENDWHILE.
```

> Read more in _Chapter 17: Smells and Heuristics: G27: Structure over Convention_ of [Robert C. Martin's _Clean Code_].

## Variables

> [Clean ABAP](#clean-abap) > [Content](#content)

### Prefer inline over up-front declarations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Variables](#variables)

If you follow these guidelines, your methods will become so short (3-5 statements) that declaring variables inline at first occurrence will look more natural

```ABAP
METHOD do_something.
  DATA(name) = 'something'.
  DATA(reader) = /clean/reader=>get_instance_for( name ).
  result = reader->read_it( ).
ENDMETHOD.
```

than declaring variables with a separate `DATA` section at the beginning of the method

```ABAP
" anti-pattern
METHOD do_something.
  DATA:
    name   TYPE seoclsname,
    reader TYPE REF TO /dirty/reader.
  name = 'something'.
  reader = /dirty/reader=>get_instance_for( name ).
  result = reader->read_it( ).
ENDMETHOD.
```

> Read more in _Chapter 5: Formatting: Vertical Distance: Variable Declarations_ of [Robert C. Martin's _Clean Code_].

### Don't declare inline in optional branches

> [Clean ABAP](#clean-abap) > [Content](#content) > [Variables](#variables)

```ABAP
" anti-pattern
IF has_entries = abap_true.
  DATA(value) = 1.
ELSE.
  value = 2.
ENDIF.
```

This works fine because ABAP handles inline declarations as if they were at the beginning of the method.
However, it is extremely confusing for readers, especially if the method is longer and you don't spot the declaration right away.
In this case, break with inlining and put the declaration up-front:

```ABAP
DATA value TYPE i.
IF has_entries = abap_true.
  value = 1.
ELSE.
  value = 2.
ENDIF.
```

> Read more in _Chapter 5: Formatting: Vertical Distance: Variable Declarations_ of [Robert C. Martin's _Clean Code_].

### Do not chain up-front declarations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Variables](#variables)

```ABAP
DATA name TYPE seoclsname
DATA reader TYPE REF TO /dirty/reader.
```

Chaining suggests the defined variables are related on a logical level.
To consistently use it, you would have to ensure that all chained variables belong together, and introduce additional chain groups to add variables.
While this is possible, it is usually not worth the effort.

Chaining also needlessly complicates reformatting and refactoring because each line looks different and changing them requires meddling with colons, dots, and commas, that are not worth the effort.

```ABAP
" anti-pattern
DATA:
  name   TYPE seoclsname,
  reader TYPE REF TO /dirty/reader.
```

> Also refer to [Don't align type clauses](#dont-align-type-clauses)

### Prefer REF TO over FIELD-SYMBOL

> [Clean ABAP](#clean-abap) > [Content](#content) > [Variables](#variables)

```ABAP
LOOP AT components REFERENCE INTO DATA(component).
```

instead of the equivalent

```ABAP
" anti-pattern
LOOP AT components ASSIGNING FIELD-SYMBOL(<component>).
```

except where you need field symbols

```ABAP
ASSIGN generic->* TO FIELD-SYMBOL(<generic>).
ASSIGN COMPONENT name OF STRUCTURE structure TO FIELD-SYMBOL(<component>).
ASSIGN (class_name)=>(static_member) TO FIELD-SYMBOL(<member>).
```

Code reviews demonstrate that people tend to choose between the two arbitrarily, "just because", "because we are always LOOPing that way", or "for no special reason".
Arbitrary choices make the reader waste time on the pointless question why one is used over the other and thus should be replaced with well-founded, precise decisions.
Our recommendation is based on this reasoning:

- Field symbols can do some things that references cannot, such as dynamically accessing the components of a structure. Likewise, references can do things that field symbols can't, such as constructing a dynamically typed data structure. In summary, settling for one alone is not possible.

- In object-oriented ABAP, references are all over the place and cannot be avoided, as any object is a `REF TO <class-name>`. In contrast, field symbols are only strictly required in few, special cases concerned with dynamic typing. References thus form a natural preference in any object-oriented program.

- Field symbols are shorter than references, but the resulting memory saving is so tiny that it can be safely neglected. Similarly, speed is not an issue. As a consequence, there is no performance-related reason to prefer one over the other.

> Read more in the article [_Accessing Data Objects Dynamically_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abendyn_access_data_obj_guidl.htm).

## Tables

> [Clean ABAP](#clean-abap) > [Content](#content)

### Use the right table type

> [Clean ABAP](#clean-abap) > [Content](#content) > [Tables](#tables)

- You typically use `HASHED` tables for **large tables** that are **filled in a single step**, **never modified**, and **read often by their key**.
  Their inherent memory and processing overhead makes hash tables only valuable for large amounts of data and lots of of read accesses.
  Each change to the table's content requires expensive recalculation of the hash, so don't use this for tables that are modified too often.

- You typically use `SORTED` tables for **large tables** that need to be **sorted at all times**, that are **filled bit by bit** or **need to be modified**, and **read often by one or more full or partial keys** or processed **in a certain order**.
  Adding, changing, or removing content requires finding the right insertion spot, but doesn't require adjusting the rest of the table's index.
  Sorted tables demonstrate their value only for large numbers of read accesses.

- Use `STANDARD` tables for **small tables**, where indexing produces more overhead than benefit, and **"arrays"**, where you either don't care at all for the order of the rows, or you want to process them in exactly the order they were appended.

> These are only rough guidelines. Find more details in the article [_Selection of Table Category_ in the ABAP Language Help](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/abenitab_kind.htm).

### Avoid DEFAULT KEY

> [Clean ABAP](#clean-abap) > [Content](#content) > [Tables](#tables)

```ABAP
" anti-pattern
DATA itab TYPE STANDARD TABLE OF row_type WITH DEFAULT KEY.
```

Default keys are often only added to get the newer functional statements working.
The keys themselves in fact are usually superfluous and waste resources for nothing.
They can even lead to obscure mistakes because they ignore numeric data types.

Either specify the key components explicitly

```ABAP
DATA itab2 TYPE STANDARD TABLE OF row_type WITH UNIQUE KEY comp1 comp2.
```

or resort to `EMPTY KEY` if you don't need a key at all.

```ABAP
DATA itab1 TYPE STANDARD TABLE OF row_type WITH EMPTY KEY.
```

> Following [Horst Keller's blog on _Internal Tables with Empty Key_](https://blogs.sap.com/2013/06/27/abap-news-for-release-740-internal-tables-with-empty-key/)

### Prefer INSERT INTO TABLE over APPEND TO

> [Clean ABAP](#clean-abap) > [Content](#content) > [Tables](#tables)

```ABAP
INSERT VALUE #( ... ) INTO TABLE itab.
```

`INSERT INTO TABLE` works with all table and key types, thus making it easier for you to refactor the table's type and key definitions if your performance requirements change.

Use `APPEND TO` only if you use a `STANDARD` table in an array-like fashion, if you want to stress that the added entry shall be the last row.

### Prefer LINE_EXISTS over READ TABLE

> [Clean ABAP](#clean-abap) > [Content](#content) > [Tables](#tables)

```ABAP
IF line_exists( my_table[ key = 'A' ] ).
```

expresses the intent clearer and shorter than

```ABAP
" anti-pattern
READ TABLE my_table TRANSPORTING NO FIELDS WITH KEY key = 'A'
IF sy-subrc = 0.

```

## Strings

> [Clean ABAP](#clean-abap) > [Content](#content)

### Use ` to define literals

> [Clean ABAP](#clean-abap) > [Content](#content) > [Strings](#strings)

```ABAP
CONSTANTS some_constant TYPE string VALUE `ABC`.
DATA(some_string) = `ABC`.  " --> TYPE string
```

Refrain from using `'`, as it adds a superfluous type conversion and confuses the reader whether he's dealing with a `CHAR` or `STRING`:

```ABAP
" anti-pattern
DATA some_string TYPE string.
some_string = 'ABC'.
```

`|` is generally okay, but cannot be used for `CONSTANTS` and adds needless overhead when specifying a fixed value:

```ABAP
" anti-pattern
DATA(some_string) = |ABC|.
```

### Use | to assemble text

> [Clean ABAP](#clean-abap) > [Content](#content) > [Strings](#strings)

```ABAP
DATA(message) = |Received HTTP code { status_code } with message { text }|.
```

String templates highlight better what's literal and what's variable, especially if you embed multiple variables in a text.

```ABAP
" anti-pattern
DATA(message) = `Received an unexpected HTTP ` && status_code && ` with message ` && text.
```

## Booleans

> [Clean ABAP](#clean-abap) > [Content](#content)

### Use ABAP_BOOL for Booleans

> [Clean ABAP](#clean-abap) > [Content](#content) > [Booleans](#booleans)

```ABAP
DATA has_entries TYPE abap_bool.
```

Don't use the generic type `char1`.
Although it is technically compatible it obscures the fact that we're dealing with a Boolean variable.

Also avoid other Boolean types as they often have strange side effects, for example `boolean` supports a third value "undefined" that results in subtle programming errors.

In some cases you may need a data dictionary element, for example for DynPro fields.
`abap_bool` cannot be used here because it is defined in the type pool `abap`, not in the data dictionary.
In this case, resort to `boole_d` or `xfeld`.
Create your own data element if you need a custom description.

> ABAP may be the one single programming language that does not come with a universal Boolean data type. However, having one is imperative. This recommendation is based on the ABAP Programming Guidelines.

### Use ABAP_TRUE and ABAP_FALSE for comparisons

> [Clean ABAP](#clean-abap) > [Content](#content) > [Booleans](#booleans)

```ABAP
has_entries = abap_true.
IF has_entries = abap_false.
```

Don't use the character equivalents `'X'` and `' '` or `space`; they make it hard to see that this is a Boolean expression:

```ABAP
" anti-pattern
has_entries = 'X'.
IF has_entries = space.
```

Avoid comparisons with `INITIAL` - it forces readers to recollect that `abap_bool`'s default is `abap_false`:

```ABAP
" anti-pattern
IF has_entries IS NOT INITIAL.
```

> ABAP may be the one single programming language that does not come with built-in "constants" for true and false. However, having them is imperative. This recommendation is based on the ABAP Programming Guidelines.

### Use XSDBOOL to set Boolean variables

> [Clean ABAP](#clean-abap) > [Content](#content) > [Booleans](#booleans)

```ABAP
DATA(has_entries) = xsdbool( line IS NOT INITIAL ).
```

The equivalent `IF`-`THEN`-`ELSE` is much longer for nothing:

```ABAP
" anti-pattern
IF line IS INITIAL.
  has_entries = abap_false.
ELSE.
  has_entries = abap_true.
ENDIF.
```

`xsdbool` is the best method for our purpose, as it directly produces a `char1`, which fits our boolean type `abap_bool` best.
The equivalent functions `boolc` and `boolx` produce different types and add an unnecessary implicit type conversion.

We agree that the name `xsdbool` is unlucky and misleading; after all, we're not at all interested in the "XML Schema Definition" stuff the "xsd" prefix suggests.

A possible alternative to `xsdbool` is the `COND` ternary form.
Its syntax is intuitive, but a little longer because it needlessly repeats the `THEN abap_true` segment, and requires knowledge of the implicit default value `abap_false` - which is why we suggest it only as secondary solution.

```ABAP
DATA(has_entries) = COND abap_bool( WHEN line IS NOT INITIAL THEN abap_true ).
```

## Conditions

> [Clean ABAP](#clean-abap) > [Content](#content)

### Try to make conditions positive

> [Clean ABAP](#clean-abap) > [Content](#content) > [Conditions](#conditions)

```ABAP
IF has_entries = abap_true.
```

For comparison, see how hard to understand the same statement gets by reversing it:

```ABAP
" anti-pattern
IF has_no_entries = abap_false.
```

The "try" in the section title means you shouldn't force this up to the point where you end up with something like this:

```ABAP
" anti-pattern
IF has_entries = abap_true.
ELSE.
  " only do something in the ELSE block, IF remains empty
ENDIF.
```

> Read more in _Chapter 17: Smells and Heuristics: G29: Avoid Negative Conditionals_ of [Robert C. Martin's _Clean Code_].

### Consider decomposing complex conditions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Conditions](#conditions)

Conditions can become easier when decomposing them into the elementary parts that make them up:

```ABAP
DATA(example_provided) = xsdbool( example_a IS NOT INITIAL OR
                                  example_b IS NOT INITIAL ).

DATA(one_example_fits) = xsdbool( applies( example_a ) = abap_true OR
                                  applies( example_b ) = abap_true OR
                                  fits( example_b ) = abap_true ).

IF example_provided = abap_true AND
   one_example_fits = abap_true.
```

instead of leaving everything in-place:

```ABAP
" anti-pattern
IF ( example_a IS NOT INITIAL OR
     example_b IS NOT INITIAL ) AND
   ( applies( example_a ) = abap_true OR
     applies( example_b ) = abap_true OR
     fits( example_b ) = abap_true ).
```

 > Tip: There is a quick fix in ADT which allows to extract conditions and create variables as shown above.

### Consider extracting complex conditions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Conditions](#conditions)

It's nearly always a good idea to extract complex conditions to methods of their own:

```ABAP
IF is_provided( example ).

METHOD is_provided.
  DATA(is_filled) = xsdbool( example IS NOT INITIAL ).
  DATA(is_working) = xsdbool( applies( example ) = abap_true OR
                              fits( example ) = abap_true ).
  result = xsdbool( is_filled = abap_true AND
                    is_working = abap_true ).
ENDMETHOD.
```

## Ifs

> [Clean ABAP](#clean-abap) > [Content](#content)

### No empty IF branches

> [Clean ABAP](#clean-abap) > [Content](#content) > [Ifs](#ifs)

```ABAP
IF has_entries = abap_false.
  " do some magic
ENDIF.
```

instead of

```ABAP
" anti-pattern
IF has_entries = abap_true.
ELSE.
  " do some magic
ENDIF.
```

### Prefer CASE to ELSE IF for multiple alternative conditions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Ifs](#ifs)

```ABAP
CASE type.
  WHEN type-some_type.
    " ...
  WHEN type-some_other_type.
    " ...
  WHEN OTHERS.
    RAISE EXCEPTION NEW /clean/unknown_type_failure( ).
ENDCASE.
```

`CASE` makes it easy to see a set of alternatives that exclude each other.
It is faster than a series of `IF`s because it translates to a different microprocessor command instead of a series of subsequently evaluated conditions.
You can introduce new cases quickly, without having to repeat the discerning variable over and over again.
The statement even prevents some errors that can occur when accidentally nesting the `IF`-`ELSEIF`s.

```ABAP
" anti-pattern
IF type = type-some_type.
  " ...
ELSEIF type = type-some_other_type.
  " ...
ELSE.
  RAISE EXCEPTION NEW /dirty/unknown_type_failure( ).
ENDIF.
```

### Keep the nesting depth low

> [Clean ABAP](#clean-abap) > [Content](#content) > [Ifs](#ifs)

```ABAP
" ani-pattern
IF <this>.
  IF <that>.
  ENDIF.
ELSE.
  IF <other>.
  ELSE.
    IF <something>.
    ENDIF.
  ENDIF.
ENDIF.
```

Nested `IF`s get hard to understand very quickly and require an exponential number of test cases for complete coverage.

Decision trees can usually be taken apart by forming sub-methods and introducing boolean helper variables.

Other cases can be simplified by merging IFs, such as

```ABAP
IF <this> AND <that>.
```

instead of the needlessly nested

```ABAP
" anti-pattern
IF <this>.
  IF <that>.
```

## Regular expressions

> [Clean ABAP](#clean-abap) > [Content](#content)

### Prefer simpler methods to regular expressions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Regular expressions](#regular-expressions)

```ABAP
IF input IS NOT INITIAL.
" IF matches( val = input  regex = '.+' ).

WHILE contains( val = input  sub = 'abc' ).
" WHILE contains( val = input  regex = 'abc' ).
```

Regular expressions become hard to understand very quickly.
Simple cases are usually easier without them.

Regular expressions also usually consume more memory and processing time because they need to be parsed into an expression tree and compiled at runtime into an executable matcher.
Simple solutions may do with a straight-forward loop and a temporary variable.

### Prefer basis checks to regular expressions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Regular expressions](#regular-expressions)

```ABAP
CALL FUNCTION 'SEO_CLIF_CHECK_NAME'
  EXPORTING
    cls_name = class_name
  EXCEPTIONS
    ...
```

instead of reinventing things

```ABAP
" anti-pattern
DATA(is_valid) = matches( val     = class_name
                          pattern = '[A-Z][A-Z0-9_]{0,29}' ).
```

> There seems to be a natural tendency to turn blind to the Don't-Repeat-Yourself (DRY) principle when there are regular expressions around, compare section _Chapter 17: Smells and Heuristics: General: G5: Duplication_ in [Robert C. Martin's _Clean Code_].

### Consider assembling complex regular expressions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Regular expressions](#regular-expressions)

```ABAP
CONSTANTS class_names TYPE string VALUE `CL\_.*`.
CONSTANTS interface_names TYPE string VALUE `IF\_.*`.
DATA(object_names) = |{ class_names }|{ interface_names }|.
```

Some complex regular expressions become easier when you demonstrate the reader how they are built up from more elementary pieces.

## Classes

> [Clean ABAP](#clean-abap) > [Content](#content)

### Object orientation

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes)

#### Prefer objects to static classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Object orientation](#object-orientation)

Static classes give up all advantages gained by object orientation in the first place.
They especially make it nearly impossible to replace productive dependencies with test doubles in unit tests.

If you think about whether to make a class or method static, the answer will nearly always be: no.

One accepted exception to this rule are plain type utils classes.
Their methods make it easier to interact with certain ABAP types.
They are not only completely stateless, but so basic that they look like ABAP statements or built-in functions.
The discriminating factor is that their consumers tie them into their code so tightly that they actually don't want to mock them in unit tests.

```ABAP
CLASS /clean/string_utils DEFINITION [...].
  CLASS-METHODS trim
    IMPORTING
      string        TYPE string
    RETURNING
      VALUE(result) TYPE string.
ENDCLASS.

METHOD retrieve.
  DATA(trimmed_name) = /clean/string_utils=>trim( name ).
  result = read( trimmed_name ).
ENDMETHOD.
```

#### Prefer composition over inheritance

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Object orientation](#object-orientation)

Avoid building hierarchies of classes with inheritance. Instead, favor composition.

Clean inheritance is hard to design because you need to respect rules like the [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle).
It is also hard to understand because people need to realize and digest the guiding principles behind the hierarchy.
Inheritance reduces reuse because methods tend to be made available only to sub-classes.
It also complicates refactoring because moving or changing members tend to require changes to the whole hierarchy tree.

Composition means that you design small, independent objects, each of which serves one specific purpose.
These objects can be recombined into more complex objects by simple delegation and facade patterns.
Composition may produce more classes, but has otherwise no further disadvantages.

Don't let this rule discourage you from using inheritance in the right places.
There are good applications for inheritance, especially the [composite design pattern](https://en.wikipedia.org/wiki/Composite_pattern).
Just ask yourself critically whether inheritance in your case will really provide more benefits than disadvantages.
If in doubt, composition generally is the safer choice.

#### Don't mix stateful and stateless in the same class

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Object orientation](#object-orientation)

Don't mix the stateless and the stateful programming paradigms in the same class.

In stateless programming, methods get input and produce output, _without any side effects_, i.e. methods produce the same result no matter when and in what order they are called.

```ABAP
CLASS /clean/xml_converter DEFINITION PUBLIC FINAL CREATE PUBLIC.
  PUBLIC SECTION.
    METHODS convert
      IMPORTING
        file_content  TYPE xstring
      RETURNING
        VALUE(result) TYPE /clean/some_inbound_message.
ENDCLASS.

CLASS /clean/xml_converter IMPLEMENTATION.
  METHOD convert.
    cl_proxy_xml_transform=>xml_xstring_to_abap(
      EXPORTING
        xml       = file_content
        ext_xml   = abap_true
        svar_name = 'ROOT_NODE'
      IMPORTING
        abap_data = result ).
  ENDMETHOD.
ENDCLASS.
```

In stateful programming, we manipulate the internal state of objects through their methods, meaning this is _full of side effects_.

```ABAP
CLASS /clean/log DEFINITION PUBLIC CREATE PUBLIC.
  PUBLIC SECTION.
    METHODS add_message IMPORTING message TYPE /clean/message.
  PRIVATE SECTION.
    DATA messages TYPE /clean/message_table.
ENDCLASS.

CLASS /clean/log IMPLEMENTATION.
  METHOD add_message.
    INSERT message INTO TABLE messages.
  ENDMETHOD.
ENDCLASS.
```

Both paradigms are okay and have their applications.
However, _mixing_ them in the same object produces code that is hard to understand and sure to fail with obscure carry-over errors and synchronicity problems.
Don't do that.

> Read more on this issue and how to avoid and remedy it in the [blog _Stateful vs. Stateless_](https://github.wdf.sap.corp/D049119/Blog/blob/master/StatefulVsStateless.md).

### Scope

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes)

#### Global by default, local only in exceptional cases

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Scope](#scope)

Work with global classes as default (meaning the ones that are visible in the dictionary).

Use local classes only in exceptional cases, for example for very specific data structures or to facilitate writing unit tests.

Local classes hinder reuse because they cannot be used elsewhere.
Although they are easy to extract, people will usually fail to even find them.

A clear indication that a local class should be made global is if you have the urge to write tests for it.
A local class is a natural private cogwheel in its greater global class that you will usually not test.
The need for tests indicates that the class is independent from its surrounding and so complex that it should become an object of its own.

#### FINAL if not designed for inheritance

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Scope](#scope)

Make classes that are not explicitly designed for inheritance `FINAL`.

When designing class cooperation, your first choice should be [composition, not inheritance](#prefer-composition-over-inheritance).
Enabling inheritance is not something that should be done lightly, as it requires you to think about things like `PROTECTED` vs. `PRIVATE` and the [Liskov substitution principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle), and freezes a lot of design internals.
If you didn't consider these things in your class design, you should thus prevent accidental inheritance by making your class `FINAL`.

There _are_ some good applications for inheritance, of course, for example the design pattern [composite](https://en.wikipedia.org/wiki/Composite_pattern).
Business Add-Ins can also become more useful by allowing sub-classes, enabling the customer to reuse most of the original code.
However, note that all of these cases have inheritance built in by design from the start.

Unclean classes that don't [implement interfaces](#public-instance-methods-should-be-part-of-an-interface) should be left non-`FINAL` to allow consumers mocking them in their unit tests.

#### Members PRIVATE by default, PROTECTED only if needed

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Scope](#scope)

Make attributes, methods, and other class members `PRIVATE` by default.

Make them only `PROTECTED` if you want to enable sub-classes that override them.

Internals of classes should be made available to others only on a need-to-know basis.
This includes not only outside callers but also sub-classes.
Making information over-available can cause subtle errors by unexpected redefinitions and hinder refactoring because outsiders freeze members in place that should still be liquid.

#### Consider using immutable instead of getter

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Scope](#scope)

An immutable is an object that never changes after its construction. For this kind of object consider using public read-only attributes instead of getter methods.

```ABAP
CLASS /clean/some_data_container DEFINITION.
  PUBLIC SECTION.
    METHODS constructor
      IMPORTING
        a TYPE i
        b TYPE c
        c TYPE d.
    DATA a TYPE i READ-ONLY.
    DATA b TYPE c READ-ONLY.
    DATA c TYPE d READ-ONLY.
ENDCLASS.
```

instead of

```ABAP
CLASS /dirty/some_data_container DEFINITION.
  PUBLIC SECTION.
    METHODS get_a ...
    METHODS get_b ...
    METHODS get_c ...
  PRIVATE SECTION.
    DATA a TYPE i.
    DATA b TYPE c.
    DATA c TYPE d.
ENDCLASS.
```

> **Caution**: For objects which **do** have changing values, do not use public read-only attributes. Otherwise this attributes always have to be kept up to date, regardless if their value is needed by any other code or not.

#### Use READ-ONLY sparingly

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Scope](#scope)

Many modern programming languages, especially Java, recommend making class members read-only wherever appropriate to prevent accidental side effects.

While ABAP _does_ offer the `READ-ONLY` addition for data declarations, we recommend to use it sparingly.

First, the addition is only available in the `PUBLIC SECTION`, reducing its applicability drastically.
You can neither add it to protected or private members nor to local variables in a method.

Second, the addition works subtly different from what people might expect from other programming languages:
READ-ONLY data can still be modified freely from any method within the class itself, its friends, and its sub-classes.
This contradicts the more widespread write-once-modify-never behavior found in other languages.
The difference may lead to bad surprises.

> To avoid misunderstandings: Protecting variables against accidental modification is a good practice. We would recommend applying it to ABAP as well if there was an appropriate statement.

### Constructors

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes)

#### Prefer NEW over CREATE OBJECT

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Constructors](#constructors)

```ABAP
DATA object TYPE REF TO /clean/some_number_range.
object = NEW #( '/CLEAN/CXTGEN' )
...
DATA(object) = NEW /clean/some_number_range( '/CLEAN/CXTGEN' ).
...
DATA(object) = CAST /clean/number_range( NEW /clean/some_number_range( '/CLEAN/CXTGEN' ) ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
DATA object TYPE REF TO /dirty/some_number_range.
CREATE OBJECT object
  EXPORTING
    number_range = '/DIRTY/CXTGEN'.
```

except where you need dynamic types, of course

```ABAP
CREATE OBJECT number_range TYPE (dynamic_type)
  EXPORTING
    number_range = '/CLEAN/CXTGEN'.
```

#### If your global class is CREATE PRIVATE, leave the CONSTRUCTOR public

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Constructors](#constructors)

```ABAP
CLASS /clean/some_api DEFINITION PUBLIC FINAL CREATE PRIVATE.
  PUBLIC SECTION.
    METHODS constructor.
```

We agree that this contradicts itself.
However, according to the article [_Instance Constructor_ of the ABAP Help](https://ldcifri.wdf.sap.corp:44300/sap/public/bc/abap/docu?input=guideline&format=standard&object=abeninstance_constructor_guidl&sap-language=EN&sap-client=100&full_text=X&tree=x), specifying the `CONSTRUCTOR` in the `PUBLIC SECTION` is required to guarantee correct compilation and syntax validation.

This applies only to global classes.
In local classes, make the constructor private, as it should be.

#### Prefer multiple static factory methods over optional parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Constructors](#constructors)

```ABAP
CLASS-METHODS describe_by_data IMPORTING p_data TYPE any [...]
CLASS-METHODS describe_by_name IMPORTING p_name TYPE any [...]
CLASS-METHODS describe_by_object_ref IMPORTING p_object_ref TYPE REF TO object [...]
CLASS-METHODS describe_by_data_ref IMPORTING p_data_ref TYPE REF TO data [...]
```

Don't try to "remedy" ABAP's missing support for [overloading](https://en.wikipedia.org/wiki/Function_overloading) by adding optional parameters.

```ABAP
" anti-pattern
METHODS constructor
  IMPORTING
    p_data       TYPE any OPTIONAL
    p_name       TYPE any OPTIONAL
    p_object_ref TYPE REF TO object OPTIONAL
    p_data_ref   TYPE REF TO data OPTIONAL
  [...]
```

The general guideline [_Split methods instead of adding OPTIONAL parameters_](#split-methods-instead-of-adding-optional-parameters) explains the reasoning behind this.

#### Use descriptive names for multiple constructor methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Constructors](#constructors)

```ABAP
CLASS-METHODS describe_by_data IMPORTING p_data TYPE any [...]
CLASS-METHODS describe_by_name IMPORTING p_name TYPE any [...]
CLASS-METHODS describe_by_object_ref IMPORTING p_object_ref TYPE REF TO object [...]
CLASS-METHODS describe_by_data_ref IMPORTING p_data_ref TYPE REF TO data [...]
```

instead of something meaningless like

```ABAP
CLASS-METHODS create_1 IMPORTING p_data TYPE any [...]
CLASS-METHODS create_2 IMPORTING p_name TYPE any [...]
CLASS-METHODS create_3 IMPORTING p_object_ref TYPE REF TO object [...]
CLASS-METHODS create_4 IMPORTING p_data_ref TYPE REF TO data [...]
```

Good words to start constructors are `new_`, `create_`, and `construct_`.
People intuitively connect them to the construction of objects.
They also add up nicely to verb phrases like `new_from_template`, `create_as_copy`, or `create_by_name`.

#### Make singletons only where multiple instances don't make sense

> [Clean ABAP](#clean-abap) > [Content](#content) > [Classes](#classes) > [Constructors](#constructors)

```ABAP
METHOD new.
  IF singleton IS NOT BOUND.
    singleton = NEW /clean/my_class( ).
  ENDIF.
  result = singleton.
ENDMETHOD.
```

Apply the singleton pattern where your object-oriented design says that having a second instance of something doesn't make sense.
Use it to ensure that every consumer is working with the same thing in the same state and with the same data.

Do not use the singleton pattern out of habit or because some performance rule tells you so.
It is the most overused and wrongly applied pattern and produces unexpected cross-effects and needlessly complicates testing.
If there are no design-driven reasons for a unitary object, leave that decision to the consumer - he can still reach the same by means outside the constructor, for example by means of a factory.

## Methods

> [Clean ABAP](#clean-abap) > [Content](#content)

These rules apply to methods in classes and function modules.

### Calls

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods)

#### Prefer functional over procedural calls

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls)

```ABAP
modify->update( node           = /clean/my_bo_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
CALL METHOD modify->update
  EXPORTING
    node           = /dirty/my_bo_c=>node-item
    key            = item->key
    data           = item
    changed_fields = changed_fields.
```

If dynamic typing forbids functional calls, resort to the procedural style

```ABAP
CALL METHOD modify->(method_name)
  EXPORTING
    node           = /clean/my_bo_c=>node-item
    key            = item->key
    data           = item
    changed_fields = changed_fields.
```

Many of the detailed rules below are just more specific variations of this advice.

#### Omit RECEIVING

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls)

```ABAP
DATA(sum) = aggregate_values( values ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
aggregate_values(
  EXPORTING
    values = values
  RECEIVING
    result = DATA(sum) ).
```

#### Omit the optional keyword EXPORTING

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls)

```ABAP
modify->update( node           = /clean/my_bo_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
modify->update(
  EXPORTING
    node           = /dirty/my_bo_c=>node-item
    key            = item->key
    data           = item
    changed_fields = changed_fields ).
```

#### Omit the parameter name in single parameter calls

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Calls](#calls)

```ABAP
DATA(unique_list) = remove_duplicates( list ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
DATA(unique_list) = remove_duplicates( list = list ).
```

There are cases, however, where the method name alone is not clear enough and repeating the parameter name may further understandability:

```ABAP
car->drive( speed = 50 ).
```

### Object orientation

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods)

#### Prefer instance to static methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Object orientation](#object-orientation)

Methods should be instance members by default.
Instance methods better reflect the "object-hood" of the class.
They can be mocked easier in unit tests.

```ABAP
METHODS publish.
```

Methods should be static only in exceptional cases, such as static constructor methods.

```ABAP
CLASS-METHODS create_instance
  RETURNING
    VALUE(result) TYPE REF TO /clean/blog_post.
```

#### Public instance methods should be part of an interface

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Object orientation](#object-orientation)

Public instance methods should always be part of an interface.
This decouples dependencies and simplifies mocking them in unit tests.

```ABAP
METHOD /clean/blog_post~publish.
```

In clean object orientation, having a method public without an interface, does not make much sense, with few exceptions, such as enumeration classes, which will never have an alternative implementation and will never be mocked in test cases.

### Parameter Number

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods)

#### Aim for few IMPORTING parameters, at best less than three

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Number](#parameter-number)

```ABAP
FUNCTION seo_class_copy
  IMPORTING
    clskey      TYPE seoclskey
    new_clskey  TYPE seoclskey
    config      TYPE class_copy_config
  EXPORTING
    ...
```

would be much clearer than

```ABAP
" anti-pattern
FUNCTION seo_class_copy
  IMPORTING
    clskey                 TYPE seoclskey
    new_clskey             TYPE seoclskey
    access_permission      TYPE seox_boolean DEFAULT seox_true
    VALUE(save)            TYPE seox_boolean DEFAULT seox_true
    VALUE(suppress_corr)   TYPE seox_boolean DEFAULT seox_false
    VALUE(suppress_dialog) TYPE seox_boolean DEFAULT seox_false
    VALUE(authority_check) TYPE seox_boolean DEFAULT seox_true
    lifecycle_manager      TYPE REF TO if_adt_lifecycle_manager OPTIONAL
    lock_handle            TYPE REF TO if_adt_lock_handle OPTIONAL
    VALUE(suppress_commit) TYPE seox_boolean DEFAULT seox_false
  EXPORTING
    ...
```

Too many input parameters let the complexity of a method explode because it needs to handle an exponential number of combinations.
Many parameters are an indicator that the method may do more than one thing.

You can reduce the number of parameters by combining them into meaningful sets with structures and objects.

#### Split methods instead of adding OPTIONAL parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Number](#parameter-number)

```ABAP
METHODS do_one_thing IMPORTING what_i_need TYPE string.
METHODS do_another_thing IMPORTING something_else TYPE i.
```

instead of trying to compensate ABAP's missing support for [overloading](https://en.wikipedia.org/wiki/Function_overloading) by adding optional parameters

```ABAP
" anti-pattern
METHODS do_one_or_the_other
  IMPORTING
    what_i_need    TYPE string
    something_else TYPE i.
```

Optional parameters confuse callers:
Which ones are really required?
Which combinations are valid?
Which ones exclude each other?

Multiple methods avoid this confusion by giving clear guidance which parameter combinations are valid and expected.

#### Use PREFERRED PARAMETER sparingly

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Number](#parameter-number)

The addition `PREFERRED PARAMETER` makes it hard to see which parameter is actually supplied, making it harder to understand the code.
Minimizing the number of parameters, especially optional ones, automatically reduces the need for `PREFERRED PARAMETER`.

#### RETURN, EXPORT, or CHANGE exactly one parameter

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Number](#parameter-number)

A good method does _one thing_, and that should be reflected by the method also returning exactly one thing.
If the output parameters of your method do _not_ form a logical entity, your method does more than one thing and you should split it.

There are cases where the output is a logical entity that consists of multiple things.
These are easiest represented by returning a structure or object:

```ABAP
TYPES:
  BEGIN OF check_result,
    result      TYPE result_type,
    failed_keys TYPE /bobf/t_frw_key,
    messages    TYPE /bobf/t_frw_message,
  END OF check_result.

METHODS check_business_partner
  IMPORTING
    business_partners TYPE business_partners
  RETURNING
    VALUE(result)     TYPE check_result.
```

instead of

```ABAP
" anti-pattern
METHODS check_business_partner
  IMPORTING
    business_partner TYPE business_partners
  EXPORTING
    result           TYPE result_type
    failed_keys      TYPE /bobf/t_frw_key
    messages         TYPE /bobf/t_frw_message.
```

Especially in comparison to multiple EXPORTING parameters, this allows people to use the functional call style, spares you having to think about `IS SUPPLIED` and saves people from accidentally forgetting to retrieve a vital `EV_ERROR_OCCURRED` information.

Instead of multiple optional output parameters, consider splitting the method according to meaningful call patterns:

```ABAP
TYPES:
  BEGIN OF check_result,
    result      TYPE result_type,
    failed_keys TYPE /bobf/t_frw_key,
    messages    TYPE /bobf/t_frw_message,
  END OF check_result.

METHODS check
  IMPORTING
    business_partners TYPE business_partners
  RETURNING
    VALUE(result)     TYPE result_type.

METHODS check_and_report
  IMPORTING
    business_partners TYPE business_partners
  RETURNING
    VALUE(result)     TYPE check_result.
```

### Parameter Types

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods)

#### Prefer RETURNING over EXPORTING

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types)

```ABAP
METHODS square
  IMPORTING
    number        TYPE i
  RETURNING
    VALUE(result) TYPE i.

DATA(result) = square( 42 ).
```

Instead of the needlessly longer

```ABAP
" anti-pattern
METHODS square
  IMPORTING
    number TYPE i
  EXPORTING
    result TYPE i.

square(
  EXPORTING
    number = 42
  IMPORTING
    result = DATA(result) ).
```

#### RETURNING large tables is usually okay

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types)

Although the ABAP language documentation and performance guides say otherwise, we rarely encounter cases where handing over a large or deeply-nested table in a VALUE parameter _really_ causes performance problems.
We therefore recommend to generally use

```ABAP
METHODS get_large_table
  RETURNING
    VALUE(result) TYPE /clean/some_table_type.

METHOD get_large_table.
  result = me->large_table.
ENDMETHOD.

DATA(my_table) = get_large_table( ).
```

Only if there is actual proof (= a bad performance measurement) for your individual case should you resort to the more cumbersome procedural style

```ABAP
" anti-pattern
METHODS get_large_table
  EXPORTING
    result TYPE /dirty/some_table_type.

METHOD get_large_table.
  result = me->large_table.
ENDMETHOD.

get_large_table( IMPORTING result = DATA(my_table) ).
```

> This section contradicts the ABAP Programming Guidelines and Code Inspector checks, both of whom suggest that large tables should be EXPORTED by reference to avoid performance deficits. We consistently failed to reproduce any performance and memory deficits. Our guess is that ongoing kernel optimization unconsciously made this superfluous. This section is under discussion in [Issue 22](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode/issues/22).

#### Use either RETURNING or EXPORTING or CHANGING, but not a combination

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types)

```ABAP
METHODS copy_class
  IMPORTING
    old_name      TYPE seoclsname
    new name      TYPE secolsname
  RETURNING
    VALUE(result) TYPE copy_result
  RAISING
    /clean/class_copy_failure.
```

instead of confusing mixtures like

```ABAP
" anti-pattern
METHODS copy_class
  ...
  RETURNING
    VALUE(result)      TYPE vseoclass
  EXPORTING
    error_occurred     TYPE abap_bool
  CHANGING
    correction_request TYPE trkorr
    package            TYPE devclass.
```

Different sorts of output parameters is an indicator that the method does more than one thing.
It confuses the reader and makes calling the method needlessly complicated.

An acceptable exception to this rule may be builders that consume their input while building their output:

```ABAP
METHODS build_tree
  CHANGING
    tokens        TYPE tokens
  RETURNING
    VALUE(result) TYPE REF TO tree.
```

However, even those can be made clearer by objectifying the input:

```ABAP
METHODS build_tree
  IMPORTING
    tokens        TYPE REF TO token_stack
  RETURNING
    VALUE(result) TYPE REF TO tree.
```

#### Use CHANGING sparingly, where suited

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types)

`CHANGING` should be reserved for cases where an existing local variable that is already filled is updated in only some places:

```ABAP
METHODS update_references
  IMPORTING
    new_reference TYPE /bobf/conf_key
  CHANGING
    bo_nodes      TYPE root_nodes.

METHOD update_references.
  LOOP AT bo_nodes REFERENCE INTO DATA(bo_node).
    bo_node->reference = new_reference.
  ENDLOOP.
ENDMETHOD.
```

Do not force your callers to introduce unnecessary local variables only to supply your `CHANGING` parameter.
Do not use `CHANGING` parameters to initially fill a previously empty variable.

#### Split method instead of Boolean input parameter

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Types](#parameter-types)

Boolean input parameters are often an indicator that the method does _two_ things instead of one.
Challenge these parameters and investigate whether it would make more sense to split the method.

```ABAP
METHODS update_without_saving.
METHODS update_and_save.
```

may be clearer than

```ABAP
" anti-pattern
METHODS update
  IMPORTING
    do_save TYPE abap_bool.
```

Common perception suggests that setters for Boolean variables are okay:

```ABAP
METHODS set_is_deleted
  IMPORTING
    new_value TYPE abap_bool.
```

### Parameter Names

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods)

#### Consider calling the RETURNING parameter RESULT

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Names](#parameter-names)

Good method names are usually so good that the `RETURNING` parameter does not need a name of its own.
The name would do little more than parrot the method name or repeat something obvious.
In these cases, simply call the parameter `RESULT`, or something like `RV_RESULT` with Hungarian notation.

Name the `RETURNING` parameter if it is _not_ obvious what it stands for, for example in methods that return `me` for method chaining, or in methods that create something but don't return the created entity but only its key or so.

### Parameter Initialization

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods)

#### Clear or overwrite EXPORTING reference parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Initialization](#parameter-initialization)

Reference parameters refer to existing memory areas that may be filled beforehand.
Clear or overwrite them to provide reliable data:

```ABAP
METHODS square
  EXPORTING
    result TYPE i.

" clear
METHOD square.
  CLEAR result.
ENDMETHOD.

" overwrite
METHOD square.
  result = cl_abap_math=>square( 2 ).
ENDMETHOD.
```

#### Don't clear VALUE parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Parameter Initialization](#parameter-initialization)

Parameters that work by VALUE are handed over as new, separate memory areas that are empty by definition.
Don't clear them again:

```ABAP
METHODS square
  EXPORTING
    VALUE(result) TYPE i.

METHOD square.
  " no need to CLEAR result
ENDMETHOD.
```

RETURNING parameters are always VALUE parameters, so you never have to clear them:

```ABAP
METHODS square
  RETURNING
    VALUE(result) TYPE i.

METHOD square.
  " no need to CLEAR result
ENDMETHOD.
```

### Method Body

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods)

#### Do one thing, do it well, do it only

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Method Body](#method-body)

A method should do one thing, and only one thing.
It should do it in the best way possible.

A method likely does one thing if

- it has [few input parameters](#aim-for-few-importing-parameters-at-best-less-than-three)
- that [don't include Boolean parameters](#split-method-instead-of-boolean-input-parameter)
- it has [exactly one output parameter](#return-export-or-change-exactly-one-parameter)
- it is [small](#keep-methods-small)
- it [descends one level of abstraction](#descend-one-level-of-abstraction)
- you cannot extract meaningful other methods
- you cannot meaningfully group its statements into sections

#### Focus on the happy path or error handling, but not both

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Method Body](#method-body)

As a specialization of the rule [Do one thing, do it well, do it only](#do-one-thing-do-it-well-do-it-only), a method should either follow the happy-path it's built for, or the error-handling-detour in case it can't, but probably not both.

```ABAP
" anti-pattern
METHOD append_xs.
  IF input > 0.
    DATA(remainder) = input.
    WHILE remainder > 0.
      result = result && `X`.
      remainder = remainder - 1.
    ENDWHILE.
  ELSEIF input = 0.
    RAISE EXCEPTION /dirty/sorry_cant_do( ).
  ELSE.
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
ENDMETHOD.
```

Can be decomposed into

```ABAP
METHOD append_xs.
  validate( input ).
  DATA(remainder) = input.
  WHILE remainder > 0.
    result = result && `X`.
    remainder = remainder - 1.
  ENDWHILE.
ENDMETHOD.

METHOD validate.
  IF input = 0.
    RAISE EXCEPTION /dirty/sorry_cant_do( ).
  ELSEIF input < 0.
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
ENDMETHOD.
```

or, to stress the validation part

```ABAP
" anti-pattern
METHOD append_xs.
  IF input > 0.
    result = append_xs_without_check( input ).
  ELSEIF input = 0.
    RAISE EXCEPTION /dirty/sorry_cant_do( ).
  ELSE.
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
ENDMETHOD.

METHOD append_xs_without_check.
  DATA(remainder) = input.
  WHILE remainder > 0.
    result = result && `X`.
    remainder = remainder - 1.
  ENDWHILE.
ENDMETHOD.
```

#### Descend one level of abstraction

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Method Body](#method-body)

Statements in a method should be one level of abstraction below the method itself.
Correspondingly, they should all be on the same level of abstraction.

```ABAP
METHOD create_and_publish.
  post = create_post( user_input ).
  post->publish( ).
ENDMETHOD.
```

instead of confusing mixtures of low level (`trim`, `to_upper`, ...) and high level (`publish`, ...) concepts like

```ABAP
" anti-pattern
METHOD create_and_publish.
  post = NEW blog_post( ).
  DATA(user_name) = trim( to_upper( sy-uname ) ).
  post->set_author( user_name ).
  post->publish( ).
ENDMETHOD.
```

A reliable way to find out what the right level of abstraction is is this:
Let the method's author explain what the method does in few, short words, without looking at the code.
The bullets (s)he numbers are the sub-methods the method should call or the statements it should execute.

#### Keep methods small

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Method Body](#method-body)

Methods should be very small, optimally around 3 to 5 statements.

```ABAP
METHOD read_and_parse_version_filters.
  DATA(active_model_version) = read_random_version_under( model_guid ).
  DATA(filter_json) = read_model_version_filters( active_model_version-guid ).
  result = parse_model_version_filters( filter_json ).
ENDMETHOD.
```

The following `DATA` declaration alone is sufficient to see that the surrounding method does way more than one thing:

```ABAP
" anti-pattern
DATA:
  class           TYPE vseoclass,
  attributes      TYPE seoo_attributes_r,
  methods         TYPE seoo_methods_r,
  events          TYPE seoo_events_r,
  types           TYPE seoo_types_r,
  aliases         TYPE seoo_aliases_r,
  implementings   TYPE seor_implementings_r,
  inheritance     TYPE vseoextend,
  friendships     TYPE seof_friendships_r,
  typepusages     TYPE seot_typepusages_r,
  clsdeferrds     TYPE seot_clsdeferrds_r,
  intdeferrds     TYPE seot_intdeferrds_r,
  attribute       TYPE vseoattrib,
  method          TYPE vseomethod,
  event           TYPE vseoevent,
  type            TYPE vseotype,
  alias           TYPE seoaliases,
  implementing    TYPE vseoimplem,
  friendship      TYPE seofriends,
  typepusage      TYPE vseotypep,
  clsdeferrd      TYPE vseocdefer,
  intdeferrd      TYPE vseoidefer,
  new_clskey_save TYPE seoclskey.
```

Of course there are occasions where it does not make sense to reduce a larger method further.
This is perfectly okay as long as the method remains [focused on one thing](#do-one-thing-do-it-well-do-it-only):

```ABAP
METHOD decide_what_to_do.
  CASE temperate.
    WHEN burning.
      result = air_conditioning.
    WHEN hot.
      result = ice_cream.
    WHEN moderate.
      result = chill.
    WHEN cold.
      result = skiing.
    WHEN freezing.
      result = hot_cocoa.
  ENDCASE.
ENDMETHOD.
```

However, you should still consider your options.
Sometimes, verbose code hides a more suitable pattern:

```ABAP
METHOD decide_what_to_do.
  result = VALUE #( spare_time_activities[ temperate = temperate ] OPTIONAL ).
ENDMETHOD.
```

### Control flow

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods)

#### Fail fast

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Control flow](#control-flow)

Validate and fail as early as possible:

```ABAP
METHOD do_something.
  IF input IS INITIAL.
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
  DATA(massive_object) = build_expensive_object_from( input ).
  result = massive_object->do_some_fancy_calculation( ).
ENDMETHOD.
```

Later validations are harder to spot and understand and may have already wasted resources to get there.

```ABAP
" anti-pattern
METHOD do_something.
  DATA(massive_object) = build_expensive_object_from( input ).
  IF massive_object IS NOT BOUND. " happens if input is initial
    RAISE EXCEPTION cx_sy_illegal_argument( ).
  ENDIF.
  result = massive_object->do_some_fancy_calculation( ).
ENDMETHOD.
```

#### CHECK or RETURN for input validation at method start

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Control flow](#control-flow)

There is no consensus on whether you should use `CHECK` or `RETURN` to exit a method if the input doesn't meet expectations.

While `CHECK` definitely provides the shorter syntax

```ABAP
METHOD read_customizing.
  CHECK keys IS NOT INITIAL.
  " do whatever needs doing
ENDMETHOD.
```

the statement's name is so obscure that people will probably understand the long form better:

```ABAP
METHOD read_customizing.
  IF keys IS INITIAL.
    RETURN.
  ENDIF.
  " do whatever needs doing
ENDMETHOD:
```

You can also avoid the question completely by reversing the validation and adopting Dijkstra's single-entry-single-exit pattern for structured programming

```ABAP
METHOD read_customizing.
  IF keys IS NOT INITIAL.
    " do whatever needs doing
  ENDIF.
ENDMETHOD:
```

In any case, consider whether returning nothing is really the appropriate behavior.
Methods should provide a meaningful result, meaning either a filled return parameter, or an exception.
Returning nothing is in many cases similar to returning `null`, which should be avoided.

> The [section _Exiting Procedures_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenexit_procedure_guidl.htm) recommends using `CHECK` in this instance. Community discussion suggests that the statement is so unclear that many people will not understand the program's behavior.

#### Avoid CHECK in other positions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Methods](#methods) > [Control flow](#control-flow)

Do not use `CHECK` outside of the initialization section of a method.
The statement behaves differently in different positions and may lead to unclear, unexpected effects.

For example, [`CHECK` in a `LOOP` ends the current iteration and proceeds with the next one](https://help.sap.com/doc/abapdocu_752_index_htm/7.52/en-US/abapcheck_loop.htm); people might accidentally expect it to end the method or exit the loop.

> Based on the [section _Exiting Procedures_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenexit_procedure_guidl.htm). Note that this contradicts the [keyword reference for `CHECK` in loops](https://help.sap.com/doc/abapdocu_752_index_htm/7.52/en-US/abapcheck_loop.htm).

## Error Handling

> [Clean ABAP](#clean-abap) > [Content](#content)

### Return Codes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling)

#### Prefer exceptions to return codes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Return Codes](#return-codes)

```ABAP
METHOD try_this_and_that.
  RAISE EXCEPTION NEW cx_failed( ).
ENDMETHOD.
```

instead of

```ABAP
" anti-pattern
METHOD try_this_and_that.
  error_occurred = abap_true.
ENDMETHOD.
```

Exceptions have multiple advantages over return codes:

- Exceptions keep your method signatures clean: you can return the result of the method as a `RETURNING` parameter and still throw exceptions alongside. Return codes pollute your signatures with additional parameters for error handling.

- The caller doesn't have to react to them immediately. He can simply write down the happy path of his code. The exception-handling `CATCH` can be at the very end of his method, or completely outside.

- Exceptions can provide details on the error in their attributes and through methods. Return codes require you to devise a different solution on your own, such as also returning a log.

- The environment reminds the caller with syntax errors to handle exceptions. Return codes can be accidentally ignored without anybody noticing.

#### Don't let failures slip through

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Return Codes](#return-codes)

If you do have to use return codes, for example because you call Functions and older code not under your control, make sure you don't let failures slip through.

```ABAP
DATA:
  current_date TYPE string,
  response     TYPE bapiret2.

CALL FUNCTION 'BAPI_GET_CURRENT_DATE'
  IMPORTING
    current_date = current_date
  CHANGING
    response     = response.

IF response-type = 'E'.
  RAISE EXCEPTION NEW /clean/some_error( );
ENDIF.
```

### Exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling)

#### Exceptions are for errors, not for regular cases

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Exceptions](#exceptions)

```ABAP
" anti-pattern
METHODS entry_exists_in_db
  IMPORTING
    key TYPE char10
  RAISING
    cx_not_found_exception.
```

If something is a regular, valid case, it should be handled with regular result parameters.

```ABAP
METHODS entry_exists_in_db
  IMPORTING
    key           TYPE char10
  RETURNING
    VALUE(result) TYPE abap_bool.
```

Exceptions should be reserved for cases that you don't expect and that reflect error situations.

```ABAP
METHODS assert_user_input_is_valid
  IMPORTING
    user_input TYPE string
  RAISING
    cx_bad_user_input.
```

Misusing exceptions misguides the reader into thinking something went wrong, when really everything is just fine.
Exceptions are also much slower than regular code because they need to be constructed and often gather lots of context information.

#### Use class-based exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Exceptions](#exceptions)

```ABAP
TRY.
    get_component_types( ).
  CATCH cx_has_deep_components_error.
ENDTRY.
```

The outdated non-class-based exceptions have the same features as return codes and shouldn't be used anymore.

```ABAP
" anti-pattern
get_component_types(
  EXCEPTIONS
    has_deep_components = 1
    OTHERS              = 2 ).
```

### Throwing

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling)

#### Use own super classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing)

```ABAP
CLASS cx_fra_static_check DEFINITION ABSTRACT INHERITING FROM cx_dynamic_check.
CLASS cx_fra_no_check DEFINITION ABSTRACT INHERITING FROM cx_no_check.
```

Consider creating abstract super classes for each exception type for your application, instead of sub-classing the foundation classes directly.
Allows you to `CATCH` all _your_ exceptions.
Enables you to add common functionality to all exceptions, such as special text handling.
`ABSTRACT` prevents people from accidentally using these non-descriptive errors directly.

#### Throw one type of exception

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing)

```ABAP
METHODS generate
  RAISING
    cx_generation_error.
```

In the vast majority of cases, throwing multiple types of exceptions has no use. The caller usually is neither interested nor able to distinguish the error situations. He will therefore typically handle them all in the same way - and if this is the case, why distinguish them in the first place?

    " anti-pattern
    METHODS generate
      RAISING
        cx_abap_generation
        cx_hdbr_access_error
        cx_model_read_error.

A better solution to recognize different error situations is using one exception type but adding sub-classes that allow - but don't require - reacting to individual error situations, as described in Use sub-classes to enable callers to distinguish error situations.

#### Use sub-classes to enable callers to distinguish error situations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing)

```ABAP
CLASS cx_bad_generation_variable DEFINITION INHERITING FROM cx_generation_error.
CLASS cx_bad_code_composer_template DEFINITION INHERITING FROM cx_generation_error.

TRY.
    generator->generate( ).
  CATCH cx_bad_generation_variable.
    log_failure( ).
  CATCH cx_bad_code_composer_template INTO DATA(bad_template_exception).
    show_error_to_user( bad_template_exception ).
  CATCH cx_generation_error INTO DATA(other_exception).
    RAISE EXCEPTION NEW cx_application_error( previous =  other_exception ).
ENDTRY.
```

If there are many different error situations, use error codes instead:

```ABAP
CLASS cx_generation_error DEFINITION ...
  PUBLIC SECTION.
    TYPES error_code_type TYPE i.
    CONSTANTS:
      BEGIN OF error_code_enum,
        bad_generation_variable    TYPE error_code_type VALUE 1,
        bad_code_composer_template TYPE error_code_type VALUE 2,
        ...
      END OF error_code_enum.
    DATA error_code TYPE error_code_type.

TRY.
    generator->generate( ).
  CATCH cx_generation_error INTO DATA(exception).
    CASE exception->error_code.
      WHEN cx_generation_error=>error_code_enum-bad_generation_variable.
      WHEN cx_generation_error=>error_code_enum-bad_code_composer_variable.
      ...
    ENDCASE.
ENDTRY.
```

#### Throw CX_STATIC_CHECK for manageable exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing)

If an exception can be expected to occur and be reasonably handled by the receiver, throw a checked exception inheriting from `CX_STATIC_CHECK`: failing user input validation, missing resource for which there are fallbacks, etc.

```ABAP
CLASS cx_file_not_found DEFINITION INHERITING FROM cx_static_check.

METHODS read_file
  IMPORTING
    file_name_enterd_by_user TYPE string
  RAISING
    cx_file_not_found.
```

This exception type _must_ be given in method signatures and _must_ be caught or forwarded to avoid syntax errors.
It is therefore plain to see for the consumer and ensures that (s)he won't be surprised by an unexpected exception and will take care of reacting to the error situation.

This guideline contradicts Robert C. Martin's _Clean Code_, which recommends to resort to unchecked exceptions in all cases to facilitate refactoring.
This is because ABAP's unchecked exceptions `CX_DYNAMIC_CHECK` and `CX_NO_CHECK` behave differently than Java's:
While Java allows declaring unchecked exceptions on methods, if you want to, ABAP forbids this.
As a consequence, Java is able to warn consumers of potential exceptions, while ABAP isn't.

#### Throw CX_NO_CHECK for usually unrecoverable situations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing)

If an exception is so severe that the receiver is unlikely to recover from it, use `CX_NO_CHECK`: failure to read a must-have resource, failure to resolve the requested dependency, etc.

```ABAP
CLASS cx_out_of_memory DEFINITION INHERITING FROM cx_no_check.

METHODS create_guid
  RETURNING
    VALUE(result) TYPE /bobf/conf_key.
```

`CX_NO_CHECK` _cannot_ be declared in method signatures, such that its occurrence will come as a bad surprise to the consumer.
In the case of unrecoverable situations, this is okay because the consumer will not be able to do anything useful about it anyway.

However, there _may_ be cases where the consumer actually wants to recognize and react to this kind of failure.
For example, a dependency manager could throw a `CX_NO_CHECK` if it's unable to provide an implementation for a requested interface because regular application code will not be able to continue.
However, there may be a test report that tries to instantiate all kinds of things just to see if it's working, and that will report failure simply as a red entry in a list - this service should be able to catch and ignore the exception instead of being forced to dump.

#### Consider CX_DYNAMIC_CHECK for avoidable exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing)

Use cases for `CX_DYNAMIC_CHECK` are rare and in general, we recommend to rather resort to the other exception types.
However, you may want to consider this kind of exception as a replacement for `CX_STATIC_CHECK` if the caller has full, conscious control over whether an exception can occur.

```ABAP
DATA value TYPE decfloat.
value = '7.13'.
cl_abap_math=>get_db_length_decs(
  EXPORTING
    in     = value
  IMPORTING
    length = DATA(length) ).
```

For example, consider the method `get_db_length_decs` of class `cl_abap_math`, that tells you the number of digits and decimal places of a decimal floating point number.
This method raises the dynamic exception `cx_parameter_invalid_type` if the input parameter does not reflect a decimal floating point number.
Usually, this method will be called for a fully and statically typed variable, such that the developer knows whether that exception can ever occur or not.
In this case, the dynamic exception would enable the caller to omit the unnecessary `CATCH` clause.

#### Dump for totally unrecoverable situations

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing)

If a situation is so severe that you are totally sure the receiver is unlikely to recover from it, or that clearly indicates a programming error, dump instead of throwing an exception: failure to acquire memory, failed index reads on a table that must be filled, etc.

```ABAP
MESSAGE x666(general).
```

This behavior will prevent any kind of consumer from doing anything useful afterwards.
Use this only if you are sure about that.

#### Prefer RAISE EXCEPTION NEW to RAISE EXCEPTION TYPE

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Throwing](#throwing)

```ABAP<f
RAISE EXCEPTION NEW cx_generation_error( previous = exception ).
```

in general is shorter than the needlessly longer

```ABAP
RAISE EXCEPTION TYPE cx_generation_error
  EXPORTING
    previous = exception.
```

However, if you make massive use of the addition `MESSAGE`, you may want to stick with the `TYPE` variant:

```ABAP
RAISE EXCEPTION TYPE cx_generation_error
  EXPORTING
    previous = exception
  MESSAGE e136(messages).
```

### Catching

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling)

#### Wrap foreign exceptions instead of letting them invade your code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Error Handling](#error-handling) > [Catching](#catching)

```ABAP
METHODS generate RAISING cx_generation_failure.

METHOD generate.
  TRY.
      generator->generate( ).
    CATCH cx_amdp_generation_failure INTO DATA(exception).
      RAISE EXCEPTION NEW cx_generation_failure( previous = exception ).
  ENDTRY.
ENDMETHOD.
```

The Law of Demeter recommends de-coupling things. Forwarding exceptions from other components violates this principle. Make yourself independent from the foreign code by catching those exceptions and wrapping them in an exception type of your own.

```ABAP
" anti-pattern
METHODS generate RAISING cx_sy_gateway_failure.

METHOD generate.
  generator->generate( ).
ENDMETHOD.
```

## Comments

> [Clean ABAP](#clean-abap) > [Content](#content)

### Express yourself in code, not in comments

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
METHOD correct_day_to_last_in_month.
  WHILE is_invalid( date ).
    reduce_day_by_one( CHANGING date = date ).
  ENDWHILE.
ENDMETHOD.

METHOD is_invalid.
  DATA(zero_if_invalid) = VALUE i( date ).
  result = xsdbool( zero_if_invalid = 0 ).
ENDMETHOD.

METHOD reduce_day_by_one.
  date+6(2) = date+6(2) - 1.
ENDMETHOD.
```

instead of

```ABAP
" correct e.g. 29.02. in non-leap years as well as result of a date calculation would be
" something like e.g. the 31.06. that example has to be corrected to 30.06.
METHOD fix_day_overflow.
  DO 3 TIMES.
    " 31 - 28 = 3 => this correction is required not more than 3 times
    lv_dummy = cv_date.
    " lv_dummy is 0 if the date value is a not existing date - ABAP specific implementation
    IF ( lv_dummy EQ 0 ).
      cv_date+6(2) = cv_date+6(2) - 1. " subtract 1 day from the given date
    ELSE.
      " date exists => no correction required
      EXIT.
    ENDIF.
  ENDDO.
ENDMETHOD.
```

Clean Code does _not_ forbid you to comment your code - it encourages you to exploit _better_ means, and resort to comments only if that fails.

### Comments are no excuse for bad names

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
DATA(input_has_entries) = has_entries( input ).
```

Improve your names instead of explaining what they really mean or why you chose bad ones.

```ABAP
" anti-pattern
" checks whether the table input contains entries
DATA(result) = check_table( input ).
```

### Use methods instead of comments to segment your code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
DATA(statement) = build_statement( ).
DATA(data) = execute_statement( statement ).
```

This not only makes the intent, structure, and dependencies of the code much clearer, it also avoids carry-over errors when temporary variables aren't properly cleared between the sections.

```ABAP
" anti-pattern
" -----------------
" Build statement
" -----------------
DATA statement TYPE string.
statement = |SELECT * FROM d_document_roots|.

" -----------------
" Execute statement
" -----------------
DATA(result_set) = adbc->execute_sql_query( statement ).
result_set->next_package( IMPORTING data = data ).
```

### Write comments to explain the why, not the what

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
" can't fail, existence of >= 1 row asserted above
DATA(first_line) = table[ 1 ].
```

Nobody needs repeating the code in natural language

```ABAP
" anti-pattern
" select alert root from database by key
SELECT * FROM d_alert_root WHERE key = key.
```

### Design goes into the design documents, not the code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
" anti-pattern
" This class serves a double purpose. First, it does one thing. Then, it does another thing.
" It does so by executing a lot of code that is distributed over the local helper classes.
" To understand what's going on, let us at first ponder the nature of the universe as such.
" Have a look at this and that to get the details.
```

Nobody reads that - seriously.
If people need to read a textbook to be able to use your code, this may be an indicator that your code has severe design issues that you should solve otherwise.
Some code _does_ need some explanation beyond a single line of comment; consider linking the design document in these cases.

### Comment with ", not with *

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

Quote comments indent along with the statements they comment

```ABAP
METHOD do_it.
  IF input IS NOT INITIAL.
    " delegate pattern
    output = calculate_result( input ).
  ENDIF.
ENDMETHOD.
```

Asterisked comments tend to indent to weird places

```ABAP
METHOD do_it.
  IF input IS NOT INITIAL.
* delegate pattern
    output = calculate_result( input ).
  ENDIF.
ENDMETHOD.
```

### Put comments before the statement they relate to

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
" delegate pattern
output = calculate_result( input ).
```

Clearer than

```ABAP
output = calculate_result( input ).
" delegate pattern
```

And less invasive than

```ABAP
output = calculate_result( input ).  " delegate pattern
```

### Delete code instead of commenting it

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
" anti-pattern
* output = calculate_result( input ).
```

When you find something like this, delete it.
The code is obviously not needed because your application works and all tests are green.
Deleted code can be reproduced from the version history later on.
If you need to preserve a piece of code permanently, copy it to a file or a $TMP or HOME object.

### Use FIXME, TODO, and XXX and add your ID

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
METHOD do_something.
  " XXX FH delete this method - it does nothing
ENDMETHOD.
```

- `FIXME` points to errors that are too small or too much in-the-making for internal incidents.
- `TODO`s are places where you want to complete something in the near(!) future.
- `XXX` marks code that works but could be better.

When you enter such a comment, add your nick, initials, or user to enable your co-developers to contact you and ask questions if the comment is unclear.

### Don't add method signature and end-of comments

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

Method signature comments don't help anybody.
Decades ago, where you couldn't see the method signature when inspecting its code, or working with printouts that had dozens of pages, these comments may have made sense.
But all modern IDEs (SE24, SE80, ADT) show the method signature easily* that these comments have become nothing but noise.

> (*) In the form-based editor of SE24/SE80, press button Signature. In ADT, mark the method name and press F2 or add the view *_ABAP Element Info_* in your perspective

```ABAP
" anti-pattern
* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method CALIBRATION_KPIS=>CALCULATE_KPI
* +-------------------------------------------------------------------------------------------------+
* | [--->] STRATEGY_ID                 TYPE        STRATEGY_ID
* | [--->] THRESHOLD                   TYPE        STRATEGY_THRESHOLD
* | [--->] DETECTION_OBJECT_SCORE      TYPE        T_HIT_RESULT
* | [<---] KPI                         TYPE        T_SIMULATED_KPI
* +-------------------------------------------------------------------------------------------------<
/SIGNATURE>
```

Similarly, end-of comments are superfluous.
These comments may have been helpful decades ago, when programs and functions and the nested IFs inside were hundreds of lines of code long.
But our modern coding style produces methods short enough to readily see what opening statement an `ENDIF` or `ENDMETHOD` belongs to:

```ABAP
" anti-pattern
METHOD get_kpi_calc.
  IF has_entries = abap_false.
    result = 42.
  ENDIF.  " IF has_entries = abap_false
ENDMETHOD.   " get_kpi_calc
```

### Don't duplicate message texts as comments

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

```ABAP
" anti-pattern
" alert category not filled
MESSAGE e003 INTO dummy.
```

Messages change independent from your code, and nobody will remember adjusting the comment, such that it will outdate and even become misleading quickly and without anybody noticing.

The modern IDEs give you easy ways to see the text behind a message, for example in the ABAP Development Tools, mark the message ID and press Shift+F2.

If you want it more explicit, consider extracting the message to a method of its own.

```ABAP
METHOD create_alert_not_found_message.
  MESSAGE e003 INTO dummy.
ENDMETHOD.
```

### ABAP Doc only for public APIs

> [Clean ABAP](#clean-abap) > [Content](#content) > [Comments](#comments)

Write ABAP Doc to document public APIs, meaning APIs that are intended for developers in other teams or applications.
Don't write ABAP Doc for internal stuff.

ABAP Doc suffers from the same weaknesses as all comments, i.e. outdates and becomes misleading quickly.
As a consequence, you should employ it only where it makes sense, not enforce writing ABAP Doc for each and everything.

> Read more in _Chapter 4: Good Comments: Javadocs in Public APIs_ and _Chapter 4: Bad Comments: Javadocs in Nonpublic Code_ of [Robert C. Martin's _Clean Code_].

## Formatting

> [Clean ABAP](#clean-abap) > [Content](#content)

Despite its maturity, ABAP unfortunately is one of the weaker languages when it comes to code formatting.
Below rules can be automated and should be done by an auto formatter, but ABAP's Pretty Printer doesn't take them into account.

### Optimize for reading, not for writing

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

Developers spend most time _reading_ code.
Actually _writing_ code takes up a way smaller portion of the day.

As a consequence, you should optimize your code formatting for reading, not for writing.

For example, you should prefer

```ABAP
DATA:
  a TYPE b,
  c TYPE d,
  e TYPE f.
```

to hacks such as

```ABAP
" anti-pattern
DATA:
  a TYPE b
  ,c TYPE d
  ,e TYPE f.
```

### Use the Pretty Printer before activating

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

Apply the pretty printer - Shift+F1 in SE80, SE24, and ADT - before activating an object.

> Read more in _Chapter 5: Formatting: Team Rules_ of [Robert C. Martin's _Clean Code_].

### Use your Pretty Printer team settings

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

Use our team settings. Go to Menu > Utilities > Settings ... > ABAP Editor > Pretty Printer.

Set "Indent" and "Convert Uppercase/Lowercase" > "Uppercase Keyword" as agreed in your team.

> Read more in _Chapter 5: Formatting: Team Rules_ of [Robert C. Martin's _Clean Code_].

#### Upper and lower case

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting) > [Use your Pretty Printer team settings](#use-your-pretty-printer-team-settings)

We don't have a clear guidance on whether keywords and identifiers should be uppercased or not.
Here is why:

General perception is that we should lowercase keywords and put identifiers in camel case.
This is the prevalent formatting in most today's programming languages and commonly perceived as concise and readable style.

However, ABAP is case-insensitive, such that camel-casing identifiers remains a mere optical improvement that tends to get lost in pretty-printing and database storages.
Also, decades ago, people decided to uppercase ABAP keywords to make them easier to identify in black-and-white printouts and monochrome displays.
Even though this main argument no longer holds, the keywords-uppercase-identifiers-lowercase format turned into a de facto standard.

The [section _Case_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenlower_upper_case_guidl.htm) follows this de facto standard.
The [section _Using Pretty Printer_ in the ABAP Programming Guidelines](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abenuse_pretty_printer_guidl.htm) contradicts and says the recommendation _should_ actually be lowercase keywords and uppercase identifiers.
We _do_ see foundation code that prefers lowercased keywords.

This point is currently discussed in [Issue #18](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode/issues/18)
Let's see whether we can come up with a more precise recommendation.
Until then we suggest a slight tendency towards following the ABAP Programming Guidelines, and keep the examples in sync with this.

### No more than one statement per line

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA do_this TYPE i.
do_this = input + 3.
```

Even if some occurrences may trick you into the misconception that this was readable:

```ABAP
" anti-pattern
DATA do_this TYPE i. do_this = input + 3.
```

### Stick to a reasonable line length

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

Adhere to a maximum line length of 120 characters.

The human eye reads text more comfortably if the lines are not too wide (ask a UI designer or eye movement researcher of your choice).
You will also appreciate the narrower code when debugging or comparing two sources next to each other.

The 80 characters limit (or even 72 in ABAP) originating in the old terminal devices is a little too restrictive.
While 100 characters are often recommended and a viable choice, 120 characters seem to work a little better for ABAP, maybe because of the general verbosity of the language.

> As a reminder you can configure in ADT the print margin to 120 characters, which then is visualized in the code view as a vertical line.

### Condense your code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA(result) = calculate( items ).
```

instead of adding unneeded blanks

```ABAP
" anti-pattern
DATA(result)        =      calculate(    items =   items )   .
```

### Add a single blank line to separate things, but not more

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA(result) = do_something( ).

DATA(else) = calculate_this( result ).
```

to highlight that the two statements do different things. But there is no reason for

```ABAP
" anti-pattern
DATA(result) = do_something( ).



DATA(else) = calculate_this( result ).
```

The urge to add separating blank lines may be an indicator that your method doesn't [do one thing](#do-one-thing-to-it-well-do-it-only).

### Don't obsess with separating blank lines

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
METHOD do_something.
  do_this( ).
  then_that( ).
ENDMETHOD.
```

No reason for the bad habit to tear your code apart with blank lines

```ABAP
" anti-pattern
METHOD do_something.

  do_this( ).

  then_that( ).

ENDMETHOD.
```

Blank lines actually only make sense if you have statements that span multiple lines

```ABAP
METHOD do_something.

  do_this( ).

  then_that(
    EXPORTING
      variable = 'A'
    IMPORTING
      result   = result ).

ENDMETHOD.
```

### Downside of alignment

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

Some of the following alignments will break when renaming refactoring produce names with a different length than before. You can either manually fix the broken alignments after renaming or decide on a formatting which is not sensitive to name length changes. Unfortunately this contradicts some of the advises given below.

### Align assignments to the same object, but not to different ones

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

To highlight that these things somehow belong together

```ABAP
structure-type = 'A'.
structure-id   = '4711'.
```

or even better

```ABAP
structure = VALUE #( type = 'A'
                        id   = '4711' ).
```

But leave things ragged that have nothing to do with each other:

```ABAP
customizing_reader = fra_cust_obj_model_reader=>s_get_instance( ).
hdb_access = fra_hdbr_access=>s_get_instance( ).
```

> Read more in _Chapter 5: Formatting: Horizontal Alignment_ of [Robert C. Martin's _Clean Code_].

### Close brackets at line end

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
modify->update( node           = if_fra_alert_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
modify->update( node           = if_fra_alert_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields
).
```

### Keep single parameter calls on one line

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA(unique_list) = remove_duplicates( list ).
remove_duplicates( CHANGING list =  list  ).
```

instead of the needlessly longer

```ABAP
" anti-pattern
DATA(unique_list) = remove_duplicates(
                           list  ).
DATA(unique_list) = remove_duplicates(
                         CHANGING
                           list =  list  ).
```

### Keep parameters behind the call

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA(sum) = add_two_numbers( value_1 = 5
                             value_2 = 6 ).
```

When this makes the lines very long, you can break the parameters into the next line:

```ABAP
DATA(sum) = add_two_numbers(
                   value_1 = ( round_up( input DIV 7 ) * 42 + round_down( 19 * step_size )
                   value_2 = VALUE #( ( `Calculation failed with a very weird result` ) ) ).
```

### If you break, indent parameters under the call

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA(sum) = add_two_numbers(
                   value_1 = 5
                   value_2 = 6 ).
```

Aligning the parameters elsewhere makes it hard to spot what they belong to:

```ABAP
" anti-pattern
DATA(sum) = add_two_numbers(
    value_1 = 5
    value_2 = 6 ).
```

> This is on the other side the best pattern if you want to avoid the formatting to be broken by a name length change.

### Line-break multiple parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA(sum) = add_two_numbers( value_1 = 5
                             value_2 = 6 ).
```

Yes, this wastes space.
However, otherwise, it's hard to spot where one parameter ends and the next starts:

```ABAP
" anti-pattern
DATA(sum) = add_two_numbers( value_1 = 5 value_2 = 6 ).
```

### Align parameters

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
modify->update( node           = if_fra_alert_c=>node-item
                key            = item->key
                data           = item
                changed_fields = changed_fields ).
```

Ragged margins make it hard to see where the parameter ends and its value begins:

```ABAP
" anti-pattern
modify->update( node = if_fra_alert_c=>node-item
                key = item->key
                data = item
                changed_fields = changed_fields ).
```

> This is on the other side the best pattern if you want to avoid the formatting to be broken by a name length change.

### Break the call to a new line if the line gets too long

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA(some_super_long_param_name) =
  if_some_annoying_interface~add_two_numbers_in_a_long_name(
      value_1 = 5
      value_2 = 6 ).
```

### Indent and snap to tab

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

Indent parameter keywords by 2 spaces and parameters by 4 spaces:

```ABAP
DATA(sum) = add_two_numbers(
              EXPORTING
                value_1 = 5
                value_2 = 6
              CHANGING
                errors  = errors ).
```

If you have no keywords, indent the parameters by 4 spaces.

```ABAP
DATA(sum) = add_two_numbers(
                value_1 = 5
                value_2 = 6  ).
```

Use the Tab key to indent. It's okay if this adds one more space than needed.
(This happens if the `DATA(sum) =` part at the left has an uneven number of characters.)

### Indent in-line declarations like method calls

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

Indent in-line declarations with VALUE or NEW as if they were method calls:

```ABAP
DATA(result) = merge_structures( a = VALUE #( field_1 = 'X'
                                              field_2 = 'A' )
                                 b = NEW /clean/structure_type( field_3 = 'C'
                                                                field_4 = 'D' ) ).
```

### Don't align type clauses

> [Clean ABAP](#clean-abap) > [Content](#content) > [Formatting](#formatting)

```ABAP
DATA name TYPE seoclsname.
DATA reader TYPE REF TO /clean/reader.
```

A variable and its type belong together and should therefore be visually grouped in close proximity.
Aligning the `TYPE` clauses draws attention away from that and suggests that the variables form one vertical group, and their types another one.
Alignment also produces needless editing overhead, requiring you to adjust all indentations when the length of the longest variable name changes.

```ABAP
" anti-pattern
DATA name   TYPE seoclsname.
DATA reader TYPE REF TO /clean/reader.
```

## Testing

> [Clean ABAP](#clean-abap) > [Content](#content)

### Principles

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing)

#### Write testable code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles)

Write all code in a way that allows you to test it in an automatic fashion.

If this requires refactoring your code, do it.
Do that first, before you start adding other features.

If you add to legacy code that is too badly structured to be tested, refactor it at least to the extent that you can test your additions.

#### Enable others to mock you

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles)

If you write code to be consumed by others, enable them to write unit tests for their own code, for example by adding interfaces in all outward-facing places, providing helpful test doubles that facilitate integration tests, or applying dependency inversion to enable them to substitute the productive configuration with a test config.

#### Readability rules

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles)

Make your test code even more readable than your productive code.
You can tackle bad productive code with good tests, but if you don't even get the tests, you're lost.

Keep your test code so simple and stupid that you will still understand it in a year from now.

Stick to standards and patterns, to enable your co-workers to quickly get into the code.

#### Don't make copies or write test reports

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles)

Don't start working on a backlog item by making a `$TMP` copy of a development object and playing around with it.
Others won't notice these objects and therefore won't know the status of your work.
You will probably waste a lot of time by making the working copy in the first place.
You will also forget to delete the copy afterwards, spamming our system and dependencies.
(Don't believe this? Go to your development system and check your `$TMP` right now.)

Also, don't start by writing a test report that calls something in a specific way, and repeat that to verify that things are still working when you're working on it.
This is poor man's testing: repeating a test report by hand and verifying by eye whether everything is still fine.
Take the next step and automate this report in a unit test, with an automatic assertion that tells you whether the code is still okay.
First, you will spare yourself the effort of having to write the unit tests afterwards.
Second, you will save a lot of time for the manual repetitions, plus avoid getting bored and tired over it.

#### Test publics, not private internals

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles)

Public parts of classes, especially the interfaces they implement, are rather stable and unlikely to change.
Let your unit tests validate only the publics to make them robust and minimize the effort you have to spend when you refactor the class.
Protected and private internals, in contrast, may change very quickly through refactoring, such that each refactoring would needlessly break your tests.

An urgent need to test private or protected methods may be an early warning sign for several kinds of design flaws.
Ask yourself:

- Did you accidentally bury a concept in your class that wants to come out into its own class, with its own dedicated suite of tests?

- Did you forget to separate the domain logic from the glue code? For example, implementing the domain logic directly in the class that is plugged into BOPF as an action, determination, or validation, or that was generated by SAP Gateway as a `*_DPC_EXT` data provider, may not the best idea.

- Are your interfaces too complicated and request too much data that is irrelevant or that cannot be mocked easily?

#### Don't obsess about coverage

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Principles](#principles)

Code coverage is there to help you find code you forgot to test, not to meet some random KPI:

Don't make up tests without or with dummy asserts just to reach the coverage. Better leave things untested to make transparent that you cannot safely refactor them.
You can have < 100% coverage and still have perfect tests.
There are cases - such as IFs in the constructor to insert test doubles - that may make it unpractical to reach 100%.
Good tests tend to cover the same statement multiple times, for different branches and conditions.
They will in fact have imaginary > 100% coverage.

### Test Classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing)

#### Call local test classes by their purpose

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Classes](#test-classes)

```ABAP
CLASS ltc_unit_tests DEFINITION FOR TESTING ... .
CLASS ltc_integration_tests DEFINITION FOR TESTING ... .
CLASS ltc_unit_tests_with_mocks DEFINITION FOR TESTING ... .
```

Good names reveal the level of the tests and what's common to their setup.

```ABAP
" anti-patterns
CLASS ltc_fra_online_detection_api DEFINITION FOR TESTING ... . " We know that's the class under test - why repeat it?
CLASS ltc_test DEFINITION FOR TESTING ....                     " Of course it's a test, what else should it be?
```

#### Put tests in local classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Classes](#test-classes)

Local classes allow running all unit tests associated with a class by pressing Ctrl+Shift+F10.

Hiding the tests in other, global classes confuses people because they can't see them and will forget to run these tests.

#### Where to put test classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Classes](#test-classes)

Unit tests are placed in the local test include of the class under test.
Component / integration / system tests implemented with the tool ABAP unit are placed in the local test include of  separate global classes. This global test class should be marked FOR TESTING and ABSTRACT.

#### How to execute test classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Classes](#test-classes)

Test classes can be executed within the ABAP development tools with Crtl-Shift-F10, with coverage measurement use Crtl-Shift-F11 and if you also want to include tests which are associated with the current code via test relations, you can use Crtl-Shift-F12.

> Note: On macOS exchange the Crtl key with the Cmd key

### Code Under Test

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing)

#### Name the code under test meaningfully, or default to CUT

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Code Under Test](#code-under-test)

Give the variable that represents the code under test a meaningful name:

```ABAP
DATA blog_post TYPE REF TO ...
```

Don't just repeat the class name with all its non-valuable namespaces and prefixes:

```ABAP
" anti-pattern
DATA clean_fra_blog_post TYPE REF TO ...
```

If you have different test setups, it can be helpful to describe the object's varying state:

```ABAP
DATA empty_blog_post TYPE REF TO ...
DATA simple_blog_post TYPE REF TO ...
DATA very_long_blog_post TYPE REF TO ...
```

If you have problems finding a meaningful name, resort to `cut` as a default.
The abbreviation stands for "code under test".

```ABAP
DATA cut TYPE REF TO ...
```

Especially in unclean and confusing tests, calling the variable `cut` can temporarily help the reader see what's actually tested.
However, tidying up the tests is the actual way to go for the long run.

#### Test interfaces, not classes

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Code Under Test](#code-under-test)

A practical consequence of the [_Test publics, not private internals_](#test-publics-not-private-internals), type your code under test with an _interface_

```ABAP
DATA code_under_test TYPE REF TO some_interface.
```

rather than a _class_

```ABAP
" anti-pattern
DATA code_under_test TYPE REF TO some_class.
```

#### Extract the call to the code under test to its own method

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Code Under Test](#code-under-test)

If the method to be tested requires a lot of parameters or prepared data, it can help to extract the call to it to a helper method of its own that defaults the uninteresting parameters:

```ABAP
METHODS map_xml_to_itab
  IMPORTING
    xml_string TYPE string
    config     TYPE /clean/xml2itab_config DEFAULT default_config
    format     TYPE /clean/xml2itab_format DEFAULT default_format.

METHOD map_xml_to_itab.
  result = cut->map_xml_to_itab( xml_string = xml_string
                                 config     = config
                                 format     = format ).
ENDMETHOD.

DATA(itab) = map_xml_to_itab( '<xml></xml>' ).
```

Calling the original method directly can swamp your test with a lot of meaningless details:

```ABAP
" anti-pattern
DATA(itab) = cut->map_xml_to_itab( xml_string = '<xml></xml>'
                                   config     = VALUE #( 'some meaningless stuff' )
                                   format     = VALUE #( 'more meaningless stuff' ) ).
```

### Injection

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing)

#### Use dependency inversion to inject test doubles

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

Dependency inversion means that you hand over all dependencies to the constructor:

```ABAP
METHODS constructor
  IMPORTING
    customizing_reader TYPE REF TO if_fra_cust_obj_model_reader.

METHOD constructor.
  me->customizing_reader = customizing_reader.
ENDMETHOD.
```

Don't use setter injection.
It enables using the productive code in ways that are not intended:

```ABAP
" anti-pattern
METHODS set_customizing_reader
  IMPORTING
    customizing_reader TYPE REF TO if_fra_cust_obj_model_reader.

METHOD do_something.
  object->set_customizing_reader( a ).
  object->set_customizing_reader( b ). " would you expect that somebody does this?
ENDMETHOD.
```

Don't use FRIENDS injection.
It will initialize productive dependencies before they are replaced, with probably unexpected consequences.
It will break as soon as you rename the internals.
It also circumvents initializations in the constructor.

```ABAP
" anti-pattern
METHOD setup.
  cut = NEW fra_my_class( ). " <- builds a productive customizing_reader first - what will it break with that?
  cut->customizing_reader ?= cl_abap_testdouble=>create( 'if_fra_cust_obj_model_reader' ).
ENDMETHOD.

METHOD constructor.
  customizing_reader = fra_cust_obj_model_reader=>s_get_instance( ).
  customizing_reader->fill_buffer( ). " <- won't be called on your test double, so no chance to test this
ENDMETHOD.
```

#### Use CL_ABAP_TESTDOUBLE

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

```ABAP
DATA(customizing_reader) = CAST /clean/customizing_reader( cl_abap_testdouble=>create( '/clean/default_custom_reader' ) ).
cl_abap_testdouble=>configure_call( customizing_reader)->returning( sub_claim_customizing ).
customizing_reader->read( 'SOME_ID' ).
```

Shorter and easier to understand than custom test doubles:

```ABAP
" anti-pattern
CLASS /dirty/default_custom_reader DEFINITION FOR TESTING CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES /dirty/customizing_reader.
    DATA customizing TYPE /dirty/customizing_table.
ENDCLASS.

CLASS /dirty/default_custom_reader IMPLEMENTATION.
  METHOD /dirty/customizing_reader~read.
    result = customizing.
  ENDMETHOD.
ENDCLASS.

METHOD test_something.
  DATA(customizing_reader) = NEW /dirty/customizing_reader( ).
  customizing_reader->customizing = sub_claim_customizing.
ENDMETHOD.
```

#### Exploit the test tools

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

In general, a clean programming style will let you do much of the work with standard ABAP unit tests and test doubles.
However, there are several tools that will allow you to tackle trickier cases in elegant ways:

- Use the `CL_OSQL_REPLACE` service to test complex OpenSQL statements by redirecting them to a test data bin that can be filled with test data without interfering with the rest of the system.

- Use the CDS test framework to test your CDS views.

- Use Avalon to test AMDPs and HANA-native database procedures.

#### Use test seams as temporary workaround

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

If all other techniques fail, or when in dangerous shallow waters of legacy code, refrain to [test seams](https://help.sap.com/doc/abapdocu_751_index_htm/7.51/en-US/index.htm?file=abendyn_access_data_obj_guidl.htm) to make things testable.

Although they look comfortable at first sight, test seams are invasive and tend to get entangled in private dependencies, such that they are hard to keep alive and stable in the long run.

We therefore recommend to refrain to test seams only as a temporary workaround to allow you refactoring the code into a more testable form.

#### Use LOCAL FRIENDS to access the dependency-inverting constructor

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

```ABAP
CLASS /clean/unit_tests DEFINITION.
  PRIVATE SECTION.
    DATA cut TYPE REF TO /clean/interface_under_test.
    METHODS setup.
ENDCLASS.

CLASS /clean/class_under_test DEFINITION LOCAL FRIENDS unit_tests.

CLASS unit_tests IMPLEMENTATION.
  METHOD setup.
    DATA(mock) = cl_abap_testdouble=>create( '/clean/some_mock' ).
    " /clean/class_under_test is CREATE PRIVATE
     " so this only works because of the LOCAL FRIENDS
    cut = NEW /clean/class_under_test( mock ).
  ENDMETHOD.
ENDCLASS.
```

#### Don't misuse LOCAL FRIENDS to invade the tested code

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

Unit tests that access private and protected members to insert mock data are fragile: they break when the internal structure of the tested code changes.

```ABAP
" anti-pattern
CLASS /dirty/class_under_test DEFINITION LOCAL FRIENDS unit_tests.
CLASS unit_tests IMPLEMENTATION.
  METHOD returns_right_result.
    cut->some_private_member = 'AUNIT_DUMMY'.
  ENDMETHOD.
ENDCLASS.
```

#### Don't change the productive code to make the code testable

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

```ABAP
" anti-pattern
IF me->in_test_mode = abap_true.
```

#### Don't sub-class to mock methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

Don't sub-class and overwrite methods to mock them in your unit tests.
Although this works, it is fragile because the tests break easily when refactoring the code.
It also enables real consumers to inherit your class, which [may hit you unprepared when not explicitly designing for it](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode#final-if-not-designed-for-inheritance).

```ABAP
" anti-pattern
CLASS unit_tests DEFINITION INHERITING FROM /dirty/real_class FOR TESTING [...].
  PROTECTED SECTION.
    METHODS needs_to_be_mocked REDEFINITION.
```

To get legacy code under test, [resort to test seams instead](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode#use-test-seams-as-temporary-workaround).
They are just as fragile but still the cleaner way because they at least don't change the class's productive behavior, as would happen when enabling inheritance by removing a previous `FINAL` flag or by changing method scope from `PRIVATE` to `PROTECTED`.

When writing new code, take this testability issue into account directly when designing the class, and find a different, better way.
Common best practices include [resorting to other test tools](https://github.wdf.sap.corp/CleanCode/ABAPCleanCode#exploit-the-test-tools) and extracting the problem method to a separate class with its own interface.

> A more specific variant of [Don't change the productive code to make the code testable](#dont-change-the-productive-code-to-make-the-code-testable).

#### Don't mock stuff that's not needed

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

```ABAP
cut = NEW /clean/class_under_test( db_reader = db_reader
                                   config    = VALUE #( )
                                   writer    = VALUE #( ) ).
```

Define your givens as precisely as possible: don't set data that your test doesn't need, and don't mock objects that are never called.
These things distract the reader from what's really going on.

```ABAP
" anti-pattern
cut = NEW /dirty/class_under_test( db_reader = db_reader
                                   config    = config
                                   writer    = writer ).
```

There are also cases where it's not necessary to mock something at all - this is usually the case with data structures and data containers.
For example, your unit tests may well work with the productive version of a `transient_log` because it only stores data without any side effects.

#### Don't build test frameworks

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Injection](#injection)

Unit tests should be data-in-data-out, with all test data being defined on the fly as needed.

```ABAP
cl_abap_testdouble=>configure_call( test_double )->returning( data ).
```

Don't start building frameworks that distinguish "test case IDs" to decide what data to provide.
The resulting code will be so long and tangled that you won't be able to keep these tests alive in the long term.

```ABAP
" anti-pattern

test_double->set_test_case( 1 ).

CASE me->test_case.
  WHEN 1.
  WHEN 2.
ENDCASE.
```

### Test Methods

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing)

#### Test method names: reflect what's given and expected

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Methods](#test-methods)

Good names reflect the given and then of the test:

```ABAP
METHOD reads_existing_entry.
METHOD throws_on_invalid_key.
METHOD detects_invalid_input.
```

Bad names reflect the when, repeat meaningless facts, or are cryptic:

```ABAP
" anti-patterns

" What's expected, success or failure?
METHOD get_conversion_exits.

" It's a test method, what else should it do but "test"?
METHOD test_loop.

" So it's parameterized, but what is its aim?
METHOD parameterized_test.

" What's "_wo_w" supposed to mean and will you still remember that in a year from now?
METHOD get_attributes_wo_w.
```

As ABAP allows only 30 characters in method names, it's fair to add an explanatory comment if the name is too short to convey enough meaning.
ABAP Doc or the first line in the test method may be an appropriate choice for the comment.

Having lots of test methods whose names are too long may be an indicator that you should split your single test class into several ones and express the differences in the givens in the class's names.

#### Use given-when-then

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Methods](#test-methods)

Organize your test code along the given-when-then paradigm:
First, initialize stuff in a given section ("given"), second call the actual tested thing ("when"), third validate the outcome ("then").

If the given or then sections get so long that you cannot visually separate the three sections anymore, extract sub-methods.
Blank lines or comments as separators may look good at first glance but don't really reduce the visual clutter. Still they are helpful for the reader to and the novice test writer to separate the section

#### "When" is exactly one call

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Methods](#test-methods)

Make sure that the "when" section of your test method contains exactly one call to the class under test:

```ABAP
METHOD rejects_invalid_input.
  " when
  DATA(is_valid) = cut->is_valid_input( 'SOME_RANDOM_ENTRY' ).
  " then
  cl_abap_unit_assert=>assert_true( is_valid ).
ENDMETHOD.
```

Calling multiple things indicates that the method has no clear focus and tests too much.
This makes it harder to find the cause when the test fails: was it the first, second, or third call that caused the failure?
It also confuses the reader because he is not sure what the exact feature under test is.

#### Don't add a TEARDOWN unless you really need it

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Methods](#test-methods)

`teardown` methods are usually only needed to clear up database entries or other external resources in integration tests.

Resetting members of the test class, esp. `cut` and the used test doubles, is superfluous; they are overwritten by the `setup` method before the next test method is started.

### Test Data

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing)

#### Make it easy to spot meaning

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Data](#test-data)

In unit tests, you want to be able to quickly tell which data and doubles are important, and which ones are only there to keep the code from crashing.
Support this by giving things that have no meaning obvious names and values, for example:

```ABAP
DATA(alert_id) = '42'.                   " well-known meaningless numbers
DATA(detection_object_type) = '?=/"&'.   " 'keyboard accidents'
CONSTANTS some_random_number = 782346.   " revealing variable names
```

Don't trick people into believing something connects to real objects or real customizing if it doesn't:

```ABAP
" anti-pattern
DATA(alert_id) = '00000001223678871'.           " this alert really exists
DATA(detection_object_type) = 'FRA_SCLAIM'      " this detection object type, too
CONSTANTS memory_limit = 4096.                  " this number looks carefully chosen
```

#### Make it easy to spot differences

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Data](#test-data)

```ABAP
exp_parameter_in = VALUE #( ( parameter_name = '45678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789END1' )
                               ( parameter_name = '45678901234567890123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789END2' ) ).
```

Don't force readers to compare long meaningless strings to spot tiny differences.

#### Use constants to describe purpose and importance of test data

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Test Data](#test-data)

```ABAP
CONSTANTS some_nonsense_key TYPE char8 VALUE 'ABCDEFGH'.

METHOD throws_on_invalid_entry.
  TRY.
      " when
      cut->read_entry( some_nonsense_key ).
      cl_abap_unit_assert=>fail( ).
    CATCH /clean/customizing_reader_error.
      " then
  ENDTRY.
ENDMETHOD.
```

### Assertions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing)

#### Few, focused assertions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions)

Assert only exactly what the test method is about, and this with a small number of assertions.

```ABAP
METHOD rejects_invalid_input.
  " when
  DATA(is_valid) = cut->is_valid_input( 'SOME_RANDOM_ENTRY' ).
  " then
  cl_abap_unit_assert=>assert_true( is_valid ).
ENDMETHOD.
```

Asserting too much is an indicator that the method has no clear focus.
This couples productive and test code in too many places: changing a feature will require rewriting a large number of tests although they are not really involved with the changed feature.
It also confuses the reader with a large variety of assertions, obscuring the one important, distinguishing assertion among them.

```ABAP
" anti-pattern
METHOD rejects_invalid_input.
  " when
  DATA(is_valid) = cut->is_valid_input( 'SOME_RANDOM_ENTRY' ).
  " then
  cl_abap_unit_assert=>assert_true( is_valid ).
  cl_abap_unit_assert=>assert_not_initial( log->get_messages( ) ).
  cl_abap_unit_assert=>assert_equals( act = sy-langu
                                      exp = 'E' ).
ENDMETHOD.
```

#### Use the right assert type

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions)

```ABAP
cl_abap_unit_assert=>assert_equals( act = table
                                    exp = test_data ).
```

Asserts often do more than meets the eye, for example assert_equals includes type matching and providing precise descriptions if values differ.
Using the wrong, too-common asserts will force you into the debugger immediately instead of allowing you to see what is wrong right from the error message.

```ABAP
" anti-pattern
cl_abap_unit_assert=>assert_true( xsdbool( act = exp ) ).
```

#### Assert content, not quantity

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions)

```ABAP
assert_contains_exactly( actual   = table
                         expected = VALUE string_table( ( `ABC` ) ( `DEF` ) ( `GHI` ) ) ).
```

Don't write magic-number-quantity assertions if you can express the actual content you expect.
Numbers may vary although the expectations are still met.
In reverse, the numbers may match although the content is something completely unexpected.

```ABAP
" anti-pattern
assert_equals( act = lines( log_messages )
               exp = 3 ).
```

#### Assert quality, not content

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions)

If you are interested in a meta quality of the result, but not in the actual content itself, express that with a suitable assert:

```ABAP
assert_all_lines_shorter_than( lines      = table
                               max_length = 80 ).
```

Asserting the precise content obscures what you actually want to test. It is also fragile because refactoring may produce a different but perfectly acceptable result although it breaks all your too-precise unit tests.

```ABAP
" anti-pattern
assert_equals( act = table
               exp = VALUE string_table( ( `ABC` ) ( `DEF` ) ( `GHI` ) ) ).
```

#### Use FAIL to check for expected exceptions

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions)

```ABAP
METHOD throws_on_empty_input.
  TRY.
      " when
      cut->do_something( '' ).
      cl_abap_unit_assert=>fail( ).
    CATCH /clean/some_exception.
      " then
  ENDTRY.
ENDMETHOD.
```

#### Forward unexpected exceptions instead of catching and failing

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions)

```ABAP
METHODS read_entry FOR TESTING RAISING /clean/some_exception.

METHOD read_entry.
  "when
  DATA(entry) = cut->read_something( ).
  "then
  cl_abap_unit_assert=>assert_not_initial( entry ).
ENDMETHOD.
```

Your test code remains focused on the happy path and is therefore much easier to read and understand, as compared to:

```ABAP
" anti-pattern
METHOD reads_entry.
  TRY.
      DATA(entry) = cut->read_something( ).
    CATCH /clean/some_exception INTO DATA(unexpected_exception).
      cl_abap_unit_assert=>fail( unexpected_exception->get_text( ) ).
  ENDTRY.
  cl_abap_unit_assert=>assert_not_initial( entry ).
ENDMETHOD.
```

#### Write custom asserts to shorten code and avoid duplication

> [Clean ABAP](#clean-abap) > [Content](#content) > [Testing](#testing) > [Assertions](#assertions)

```ABAP
METHOD assert_contains
  TRY.
      entries[ key = key ].
    CATCH cx_sy_itab_line_not_found.
      cl_abap_unit_assert=>fail( ).
  ENDTRY.
ENDMETHOD.
```

Instead of copy-pasting this over and over again.

[Robert C. Martin's _Clean Code_]: https://www.investigatii.md/uploads/resurse/Clean_Code.pdf
