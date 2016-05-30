# Chapter 1 - TDD Basics

## Introduction

In this workshop, we are going to make a simple command line calculator. Our calculator will have just `add` and `product` functionality. Requirements are pretty simple but main focus is on initial steps of `Test Driven Development`.

During this workshop we will do/learn following:

 - Create a new Git project and commit it on github.
 - Initialize composer project and understand `composer.json` file.
 - Understand composer auto-loading.
 - Use composer to install PHP Unit.
 - While coding, we will also get an introduction of basic refactoring.
 - Code. While coding, we will follow TDD approach, which include
   - Write Test - which will obviously fail.
   - Code - To make test succeed.
   - Test - To ensure we achieved our requirements.
   - Refactor - To make code better.
   - Test - To ensure refactoring did not brake anything.
   - Done - Move to next task/requirement.

## Project

To code, we need some tasks or requirements. Project section describes what are the requirements of the project we are going to make in this chapter.

In this chapter, **we are going to make a simple command line calculator.**

However, as in real projects, we will make it in parts. As in real projects, we have some initial set of requirements, we achieve them and make project live. Once project is live, we ger next set of requirements and start development for next version of application. Here, we will divide our application is tasks. Please assume every task is an application, supposed to go live after development. This means, every task must be complete in itself.

Once we complete a task, we need to start development of next task, which represent new feature of our application. This means, doing new task should not break previous task and this is where, `test driven development` comes into picture. We will write test of previous tasks, say task 1 and 2, and while working on task 3, PHP Unit will ensure Task 1 and 2 are still working.

This workshop is divided into 8 tasks. I'm not going to tell about next task. Obviously task represent future requirements and no one know what future will be? So we will take one task at a time.

## Task 1

We need to make a command line utility to sum zero, one or two numbers. Command should be

```bash
php calculator.php sum
php calculator.php sum 1
php calculator.php sum 2,3
```

and its output must 0, 1, and 5 respectively.

> Please do not consider the scenarios not mentioned in the task statements. Lets not complicate the situations. We will just do what task ask us to do.

## Task 1 solution

Lets code to achieve task 1 requirements. Before we jump to the solution, we have following prerequisite for this task.

 - `PHP 5.5+`: We assume, PHP 5.5 or higher is already installed. If not, you may check `Appendix C: XAMPP` to setup quick PHP development environment. Although a bit time taking but `Appendix D: Vagrant` or `Appendix E: Docker` is recommended way of setting local PHP development environment.
 - `Git and GitHub account`: We need Git installed locally and you must have github account (free). If you are not aware about git and github, `Appendix A: Git and Github basics` will help you.
 - `Composer`: We will also need composer. If you are not aware of composer, `Appendix B: Composer` will help you.

### Think well

I remember a story from a old Hindi movie (forgot movie name). There was a factory, where one machine was not working. Factory owner was tensed as if machine do not work for single day, he will have loss of several thousand Rupees. Fortunately an Engineer was visiting factory and was asked to check if he can fix the machine. He asked a hammer and hit machine on one point, and machine started. Owner was happy and asked him service charges.  Engineer asked Rs. 1000, which was prety high at that time. Owner was shocked, Rs. 1000 just for one hammers hit? Engineer replied, "No, just Rs. 1 for hammer hit; Rs. 999 for where should I hit."

Similarly, if a developer works 8 hours a day, it generally mean he think 6-7 hours and type 1-2 hours. This is obvious, we are developers, not typist. We do not get payment for typing, but what should we type and this needs time to think.

Before we start writing first line of code, we must think what we need to do? Do we know that? Well yes, task 1 requirements are pretty clear but wait, may be no, we do not know how will we achieve that. So lets list down few requirements.

 - To run command `php calculator.php add`, we must have `calculator.php` at the project root.
   - However this is also going to be our front controller.
   - It is not a good idea to write our business logic in `calculator.php`.
   - Also `calculator.php` needs to handle command line arguments means it will be functional code, not a class.
   - This all means, we need separate `Calculator.php` having `Calculator` class.
 - Our `Calculator` class will contain main business logic.
   - We writes PHP Unit tests for business logic, means Calculator class must have tests.
 - In command `php calculator.php add`, we have a command line argument `add`, which represent operation. Thus our `Calculator` class should have `add` method to handle `add` operation.
 - Our `add` method will have business logic to add numbers so it must be tested. Which tests?
   - Add should return an integer.
   - Add without parameter should return `0`.
   - Add with single parameter should return same number.
   - Add with two parameters should return their sum.
   - There are few failure conditions as well
     - Add with non string parameters should fail (throw exception).
     - Add with non-number within parameter should fail (throw exception).

With above few bullet points, we have a rough road-map of we need to do. Remaining chapter is all about how we do it.

### Development folder

Although not part of task, I want to recommend how to organize your projects. There must be a single folder say `dev` which must contains all of your projects. Within `dev` folder, you may keep all of your project. If you work on too many projects, may create further sub-folder to arrange different type of projects.

`dev` folder may be kept on location like `/home/<yourname>/dev` on Linux/Mac or `D:\dev` on windows.

> This also means you should never keep your projects in `/var/www` folder. That is a bad practice. Create project in dev folder and use virtual host if needed. I specially mentioned this as I saw many developer putting all of their projects in `/var/www` folder.

### Creating project

Go to the path where you want to make project like `/home/<yourname>/dev/learning/phpreboot` and run following commands:

```bash
mkdir tddworkshop
cd tddworkshop
git init
```

> Going forward, in this chapter, I will call this folder (tddworkshop) as project root. If I say run command in project root, it will mean running command in folder we just created (tddworkshop).

Above command will initialize empty git repository. Create a README.md file and make your first commit. Now create a github project, add remote and push.

> This is optional step but I recommend this as if you stuck somewhere, you must have your code online to get help from me or someone else.

If you are not familiar with git or github, please check `Appendix A: Git and Github basics`.

**.gitignore**

Initializing git project will create a folder `.git`. I do not want to commit few files to my git, for example, I'm using PHP Storm, which create a folder `.idea`. Similarly, Netbeans create a folder `nbproject`. These folder are necessary to manage project in IDE but not the part of project and it does not make sense to commit them on git.

To ignore these files, create a new file named `.gitignore` and add file/folder names in that file. For example, my `.gitignore` file looks like:

```
.idea
```

### Initializing composer project

Composer is a tool to manage dependencies of your PHP project. We too have few dependencies on third party libraries; right now PHP unit.

If you are not familiar with composer, you will find `Appendix B: Composer` useful to install composer and understand composer basics.

Simply run command `composer init` at project root. It will ask simple questions (details in `Appendix B`) and at the end generates `composer.json` file as follow:

```json
{
    "name": "kapilsharma/tddworkshop",
    "description": "Sample code for Test Driven Development (TDD) workshop.",
    "license": "MIT",
    "authors": [
        {
            "name": "kapilsharma",
            "email": "kapil@kapilsharma.info"
        }
    ],
    "require": {}
}
```

This ensure composer is initialized. Lets now install PHP unit.

### Installing PHP Unit

For Test Driven Development, we need some tool for Unit testing. PHP Unit is one of the most popular unit testing tool in PHP and we are going to use it.

Right now, PHP Unit have two active versions:

- PHP Unit 4.8 supports PHP version 5.3, 5.4, 5.5, 5.6 and 7.0
- PHP Unit 5.3 supports PHP version 5.6 and 7.0

Latest is best and PHP 5.5 is also near end of security support. So our obvious choice must be PHP Unit 5.3. However in this course, I'm going through PHP unit 4.8 as I believe few of you might not have PHP 5.6. (If you don't, its high time to update). Still purpose of this workshop is to learn TDD, not force everyone to migrate to latest PHP version.

Thus I'm going with PHP Unit 4.8 but I highly recommend others to go with PHP Unit 5.3.

To install PHP Unit 4.8 through composer, run command `composer require --dev phpunit/phpunit:4.8`. Please note `--dev` flag. This tells composer that we need PHP Unit only on development system, not on production. Running this command will install PHP Unit and following lines in composer.json

```json
    "require-dev": {
        "phpunit/phpunit": "4.8"
    }
```

It will also create a folder `vendor`. All packages installed by composer goes in vendor directory. Also add `/vendor/` in .gitignore file on separate line, if it was not already done with `composer init` command. My `.gitignore` file now looks as follow:

```
.idea
/vendor/
```

We have PHP Unit installed. We will soon use it, be patient for few more minutes.

### Auto loading.

I hope you are aware with namespaces, auto loading your PHP classes through composer auto loader. If yes, you can skip this section and move to next section.

For others, you must be aware of statements like `include`, `include_once`, `require`, `require_once`. Before PHP 5.3, they were commonly used to include one php file in another php file. With namespaces, PSR-0/PSR-4, and composer, auto-loading becomes very easy. PSR-0 and PSR-4 are auto loading standards, defined by PHP-FIG(http://www.php-fig.org). Composer also create a class named `vendor/autoload.php`. We just need to include that class in our front controller (initial script) and rest classes installed through composer can be automatically loaded.

However there is a problem, classes downloaded through composer can be automatically loaded but what about classes we created? Well composer takes care of them as well. We just need to tell composer where our classes are.

In this chapter, we will be following structure

```bash
tddworkshop (project root)
 |
 |- src
 |   |
 |   `- phpreboot (namespace phpreboot)
 |       |
 |       `- tddworkshop (namespace phpreboot\tddworkshop)
 `- tests
     |
     `- phpreboot (namespace phpreboot)
         |
         `- tddworkshop (namespace phpreboot\tddworkshop)
```

We need to tell composer that whenever we say namespace `phpreboot`, it should look folder `src/phpreboot` or `tests/phpreboot`. However `tests` folder contains only test so it will be needed only while development, not on production. To let composer know about namespaces and able to auto-load our classes, we need to add following lines in our `composer.json` file.

```json
  "autoload": {
    "psr-4": {"Phpreboot\\": "src/Phpreboot"}
  },
  "autoload-dev": {
    "psr-4": {"Phpreboot\\": "tests/Phpreboot"}
  },
```

I guess above changes are pretty clear to read. We are using PSR-4 auto-loading. Appendix B is good starting point for now but I recommend reading more about composer. With above changes, my final `composer.json` file looks like following.

```json
{
    "name": "kapilsharma/tddworkshop",
    "description": "Sample code for Test Driven Development (TDD) workshop.",
    "license": "MIT",
    "authors": [
        {
            "name": "kapilsharma",
            "email": "kapil@kapilsharma.info"
        }
    ],
    "autoload": {
      "psr-4": {"Phpreboot\\": "src/Phpreboot"}
    },
    "autoload-dev": {
      "psr-4": {"Phpreboot\\": "tests/Phpreboot"}
    },
    "require": {},
    "require-dev": {
        "phpunit/phpunit": "4.8"
    }
}
```

As final step, run `composer install` on terminal for changes to take effect.

### Tests

Our project is now setup and we are ready to start what every developer like; code. However we are going to follow TDD. This simply means first write test; obviously that test will fail but no problem. Let our test fail and then write code to make that test pass. Lets write our first test.

### Test 1: Calculator class exists.

As per our structure, lets create folders to hold our test class. Run following commands from project root.

```
mkdir tests
cd tests
mkdir phpreboot
cd phpreboot
mkdir tddworkshop
cd tddworkshop
touch CalculatorTest.php
```

Now open `CalculatorTest.php` in your preferred editor and write following code in it.

```php
<?php
namespace phpreboot\tddworkshop;

use phpreboot\tddworkshop\Calculator;

class CalculatorTest extends \PHPUnit_Framework_TestCase
{
    private $calculator;

    public function setUp()
    {
        $this->calculator = new Calculator();
    }

    public function tearDown()
    {
        $this->calculator = null;
    }

    public function testAddReturnsAnInteger()
    {
        $result = $this->calculator->add();

        $this->assertInternalType('integer', $result, 'Result of `add` is not an integer.');
    }
}
```

Let me explain this code a bit. Since we are using namespace, first line of code must define namespace. Although namespace in tests are not mandatory but it might be handy in few conditions so its always good idea to define namespace.

Next line suggest we want to use class `Calculator`. Obviously that class still do not exist but in TDD, we do not care about actual status but expected status and we expect that class Calculator must be existing.

Then we have Test class. Name of all PHP Unit test class should end with `Test`. Thus we generally name it as `<classname>Test`. Here we are supposed to test `Calculator` class so obvious name is `CalculatorTest`. Also all test classes must extend `\PHPUnit_Framework_TestCase` so that we can get benefit of methods defined by PHP Unit.

Next we defined a property $calculator and two methods `setUp` and `tearDown`. For testing Calculator class, we must create its instance and we will save that instance in class variable `$calculator`.

At broader level, we can consider `setup` and `tearDoan` as constructor and destructor respectively but not exactly. In PHP Unit, these two methods are called before and after every test. For each test, we need new instance of Calculator so we do it in `setUp` method. Once test is finished, we no longer need Calculator instance so we make it null. Another reason of making it null is, I don't want one test impact another test in any way. Making calculator instance null ensure it.

And finally we have a test `testAddReturnsAnInteger`. All test methods must have a `test` prefix (And class have `Test` suffix). Our Calculator class must have an `add` function (first command line argument) which should return an integer. So in our first test, we are testing `add` method must return an integer. You may be little surprised with name of function but we will soon come to that. For now, just believe it is a good name.

First line of test `testAddReturnsAnInteger` is pretty clear, we are calling `add` method without any parameters. Second line starts with `$this->assertInternalType`. PHP Unit provide several `assert***` methods to test different conditions. `assertInternalType`, checks if we have particular primitive type of a variable. Like most `assert***` methods, it take three parameters; expected value, actual value and optional message to display in case test fails. We should write test message carefully as once you have several thousand tests, you should be able to identify exact test which failed. We will discuss more about it shortly.

### PHP Unit configuration

This section is about `phpunit.xml` file. If you are aware about PHP Unit configuration, please skip this section.

Now we have PHP Unit installed and a Test written, its time to run tests. But wait, we need some settings. We need to define `phpunit.xml` file. In `phpunit.xml` file, we define some common configuration for PHP Unit like which classes should be tested, how do we want result to be saved/displayed or some initialization tasks. So lets create a file `phpunit.xml` at project root and add following code

```xml
<?xml version="1.0" encoding="UTF-8"?>

<phpunit bootstrap="vendor/autoload.php" colors="true">
    <testsuites>
        <testsuite name="TDDWorkshop">
            <directory>tests/phpreboot/</directory>
        </testsuite>
    </testsuites>

    <filter>
        <whitelist>
            <directory suffix=".php">src/Phpreboot/</directory>
        </whitelist>
    </filter>

    <logging>
        <log type="coverage-html" target="./log/codeCoverage" charset="UTF-8"
             yui="true" highlight="true"
             lowUpperBound="50" highLowerBound="80"/>
        <log type="testdox-html" target="./log/testdox.html" />
    </logging>
</phpunit>
```

First line is simple xml declaration. Second line define xml root tag and have two attributes. First attribute `bootstrap` tells PHP Unit how to initialize our application for testing purpose. It essentially is a script to initialize all classes and settings needed so that test can execute successfully. In our case, we need to autoload classes. If you remember, we did not included `Calculator` class in our test but using it. Since composer provide us a good autoloader, we are using it to bootstrap our PHP Unit. Second attribute just tell we need colourful output in our terminal, if supported.

Next block `testsuites` define our test-suite. Test-suite is a collection of few test cases. We want to execute all tests under `tests/phpreboot` folder so we simply added that directory. We also defined name of the testsuite. Here we are having only one test suite but if we have multiple test suites, with name, we can execute only one test suite at a time. This is specially important when we have huge product with several thousands of tests; we obviously do not like to run them all every time.

Next block is `filters`. PHP Unit can also create code completion reports. It need xdebug to be installed on the system. If you have xdebug installed, you can actually see code completion reports once you executed tests. Code completion report just tell how much part of your code is covered by PHP Unit tests, which is very handy at times. This block contains `whitelist`. In whitelist, we define classes/folders for which we want to generate code completion report. Lets generate report for our whole src folder, but only for php files.

Next section `logging` defines how do we want to save test reports. IT contains two log tags, one for code coverage report and other testdox. We will check testdox shortly in details.

So we have `phpunit.xml` file. However it is not a good idea to commit `phpunit.xml` on git. First reason, we do not run tests on production so we will not need it there and second, every developer may have different need from PHP unit. If you have huge product with many developers working on several modules and have several thousand test cases, a developer might want to edit phpunit.xml locally to run tests of only his module. Lets follow best practices and make a copy of `phpunit.xml`. Run command `cp phpunit.xml phpunit.xml.dist`. This will create a copy of phpunit.xml with name `phpunit.xml.dist` and we will commit dist file. Also do not forget to add phpunit.xml in `.gitignore` file.

### Running first test.

Now we are all set to run our first test. Open terminal, go to project root and run following command

```bash
vendor/bin/phpunit
```

As expected, running this test will give following fatal error

```bash
PHP Fatal error:  Class 'phpreboot\tddworkshop\Calculator' not found in /home/kapil/dev/github/phpreboot/tddworkshop/tests/phpreboot/tddworkshop/CalculatorTest.php on line 20
```

We do not have `Calculator` class and we are trying to create its instance in `setUp` method. Lets fix this issue and make Calculator class. Run following commands from project root.

```bash
mkdir src
cd src
mkdir phpreboot
cd phpreboot
mkdir tddworkshop
cd tddworkshop
touch Calculator.php
```

Now open `Calculator.php` and add following code:

```php
<?php
namespace phpreboot\tddworkshop;

class Calculator
{

}
```

We created the class, lets run our test again. We still get fatal error but this time, its different error.

```bash
PHP Fatal error:  Call to undefined method phpreboot\tddworkshop\Calculator::add() in /home/kapil/dev/github/phpreboot/tddworkshop/tests/phpreboot/tddworkshop/CalculatorTest.php on line 30
```

Obvious, we are trying to call `add` method on Calculator, which do not exist. Lets make it.

```php
    public function add()
    {
    }
```

and run test again. This time we do not have any error but our test failed with message

```bash
There was 1 failure:

1) phpreboot\tddworkshop\CalculatorTest::testAddReturnsAnInteger
Result of `add` is not an integer.
Failed asserting that null is of type "integer".

/home/kapil/dev/github/phpreboot/tddworkshop/tests/phpreboot/tddworkshop/CalculatorTest.php:32
```

We are expecting `add` method should return an integer but it returned nothing (null) and thus our test failed. Lets return `0` for now. Add `return 0;` in the method and run test again.

And finally, we got our test passing.

```bash
.

Time: 69 ms, Memory: 4.00MB

OK (1 test, 1 assertion)
```

Before we commit our files, please note, there is a new `log` folder. If you remember, in `phpunit.xml` we asked two reports in `logging` section. However we do not want to commit reports so lets put `log` in `.gitignore` and commit.

### Second test

If you remember `think well` section, our second test should be to confirm `add` without parameter should return 0. This condition is already matching right now as we are returning `0` (regardless of condition) but this is temporary condition. When we will write code for other conditions, add method might not return `0`. To keep us future secure, lets write a test for that. Add following method in our `CalculatorTest` class.

```php
    public function testAddWithoutParameterReturnsZero()
    {
        $result = $this->calculator->add();
        $this->assertSame(0, $result, 'Empty string on add do not returns 0');
    }
```

In this test, first line of test is pretty clear, we are just calling `add` method without any parameter and saving result in `$result` variable.

In second line, which is actually a test, we used PHP Unit's `assertSame` method. I recommend to go through PHP Unit documentations to know about all `assert***` methods. In short, `assertSame` and `assertEquals` check if expected and actual result are same? Difference, assertEquals check `expected == actual` and assertSame check `expected === actual`.

As prerequisite of this course, I expect you know difference between `==` and `===`. If no, you must first learn a bit more about PHP. In short, assertSame check value as well as type so ideally two tests for me. Now lets run the test. If you forgot, command is `vendor/bin/phpunit` in project root. Output of test is `OK (2 tests, 2 assertions)`, everything passing as expected. Still this test important as it will test this condition every time we run PHP Unit means keeping us future secure for this condition.

### Reviewing testdox.html

 Before we go ahead, lets first check generated docs. Please open `log/testdoc.html` in a browser (double click it in explorer).

 Its output should be like

 > ### phpreboot\tddworkshop\Calculator
 >
 >   - Add returns an integer
 >   - Add without parameter returns zero

 As you can see our HTML report is readable even by non-technical person. If you check closely, message is actually the name of our test function, without `test` prefix. Hope now it is clear why we selected strange names for our test function. Name should be human readable in English but at the same time, it must not be a whole sentence. While naming tests, always think a short and simple line to exactly define your test. If we `think well` about solution before starting tasks, test names will be obvious as while thinking. If you are still not able to think suitable names, don't worry, keep writing tests and keep reviewing generated HTML report. Remember the mantra `Practice makes man perfect`. With some experience, naming a test method will become obvious for you.

 > Practice makes man perfect.
 >
 > For developers, 'Code makes developer perfect'. Code a lot. Don't miss any opportunity to code, contribute to Open source.

 And remember that:

 [open-source-is-open-course]