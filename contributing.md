# Contributing

First of all: *thank you!* Any kind of support is greatly appreciated and each
contribution is increasing our motivation. This document outlines how to
contribute to make our lives easier - both you, contributors, and core developers.
It will be divided into sections describing most common ways to contribute to an open
source project.

### Reporting bugs
All bugs in Codice should be reported using GitHub issues. It makes them very easy
to track and organize but also to give you feedback regarding your report. If you think you
just found a bug, go ahead and fill a new issue, but please remember a couple of simple rules.

1. Make sure it's not duplicated
2. Shortly describe what the problem is and where it occurs.
3. Tell us how to reproduce a bug - what actions resulted in incorrect behaviour.
4. Don't forget to mention *what* is that incorrect behaviour (explanation why is
   it wrong, any kind of error messages etc) and how do you expect it to work correctly.

We will try to respond as soon as it's possible but please keep an eye on your
issue - there might be questions for you and if you answer them it will drastically
reduce time needed to fix the bug. GitHub is nice and sends email notifications so
it's not that hard.

#### Security bugs
If you have encountered a bug related to the security of Codice, we would kindly
ask you to email `security@codice.eu`.

### Suggesting new features
GitHub issues are also a good place for suggesting and discussing any new features,
ideas etc. Make sure it hasn't been already submitted and fill in an issue. We will
categorize it and try to give you feedback quickly.

### Pull requests
Propositions in form of working code should be submitted as GitHub's Pull Requests.
Give a detailed description, try to include some tests and hopefully your change
will get merged into Codice core.

#### Coding standards
We try to adhere some widely known standards for source code and kindly remind you
that any contribution regarding the code should follow them as well.

* [PSR 4 Autoloading Standard](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-4-autoloader.md)
* [PSR 2 Coding Style Guide](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)
* [PSR 1 Coding Style Guide](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md)

Keep in mind that (for server-side changes) running test suite using `phpunit` command is a must.

### Contributing documentation
#### Determining version to edit
Rules are very similiar to contributing to Codice core - any change meant for all actively
supported versions should go into `master` branch and fixes for a specific branch should
target this version's branch (e.g. `v.0.4`).

What does it mean in terms of documentation? Example of a change for a specific version is
missing information about the break in some version. If you, on the other hand, encounter
a typo in the page header, it probably happens regardless of documentation version - then
`master` is the right choice.
