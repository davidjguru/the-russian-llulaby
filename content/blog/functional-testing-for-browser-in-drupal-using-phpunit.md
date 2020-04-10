---
title: "Functional testing for Browser in Drupal 8|9 using PHPUnit"
date: 2020-04-10
draft: true

# post thumb
image: "images/post/davidjguru_functional_testing_for_drupal_based_in_phpunit.jpg"

# meta description
description: "How to build functional testing for your Drupal 8 or 9"

# taxonomies
categories: 
  - "Development"  
tags:
  - "Drupal Development"
  - "PHP"
  - "Drupal Testing"
  - "Contrib Modules"
  - "Backend"

# post type
type: "post"
---
Everything around Test-driven development (TDD) is a very interesting and very motivating world. Besides, these are issues with a certain antiquity, so it is very easy to find related contents. But the relationship between this area and Drupal is even more interesting: there are multiple options to implement testing of different types and different orientation (Unit Test, Kernel, JavaScript, functional ...) so it can be complex to introduce in this world. Today I want to take advantage of the experience of porting to Drupal 8 a small contributed module to share examples about browser-focused Functional Testing in Drupal 8 (or Drupal 9).  We will learn together with simple use cases. It may be a small slice (Functional Testing of a very small features) of a big cake (Testing in Drupal), but it will be a nice entry point. Follow me. 

--------------------------------------------------------------------------------------
  
**Picture from Unsplash, user [National Cancer Institute, @nci](https://unsplash.com/@nci)**

 
---------------------------------------------------------------------------------

**Table of Contents**  
<!-- TOC -->  
[1- Introduction](#1--introduction)  
[2- Arrangements](#2--arrangements)  
[3- The BrowserTestBase class](#3--the-browsertestbase-class)  
[4- Basic Scaffolding](#4--basic-scaffolding)  
[5- Your tests](#5--your-tests)  
[6- Running the test](#6--running-the-test)
[7- Read More](#7--read-more)  
[8- :wq!](#8--wq)    
<!-- /TOC -->

-------------------------------------------------------------------------------

# 1- Introduction 
Well, as I said in the obligatory introductory paragraph everything related to Testing is extensive, very broad and combines an ungraspable conjunction of philosophical-theoretical elements with practical-technical issues...so I guess that's why I was so happy when in the context of the migration to Drupal 8|9 of [the contributed module humans.txt](https://www.drupal.org/project/humanstxt), [Pedro Cambra](https://www.drupal.org/u/pcambra) as its maintainer proposed [in an issue](https://www.drupal.org/project/humanstxt/issues/3123126) to provide the module with a certain type of test.   
It was a great opportunity to use it as a simple, didactic and intuitive approach. 

[CONVERT]
PHPUnit can handle different types of tests in Drupal core: unit tests, kernel tests, and functional tests. With these three tests, you can confirm the quality and reaction of code on edge cases in different layers.

Unit tests test functions, methods, and classes without requiring a database connection to run. On the other hand, kernel tests test the integration of module APIs and require a database connection to be configured to run. Functional tests test the entire system and require more setup than the others. Within the functional tests, there are both browser and JavaScript tests. In addition to these PHP-based tests, you may also run core JavaScript tests using the Nightwatch framework.



PHPUnit browser functional tests are run against a fresh Drupal installation so each test requires set up like installing modules, creating users, etc. This can be done in the BrowserTestBase::setUp() method (which is run before each test method in the class) or by writing an abstract base class (that extends BrowserTestBase) to be extended by additional test classes. You could also simply run a previous test at the beginning of the test you are writing. The results of the browser tests are saved to a directory, which can be configured in the phpunit.xml file. See PHPUnit Browser test tutorial in the Drupal documentation.

It's terribly important to realize that each test runs in a completely new Drupal instance, which is created from scratch for the test. In other words, none of your configuration and none of your users exists! None of your modules are enabled beyond the default Drupal core modules. If your test sequence requires a privileged user, you'll have to create one (just as you would if you were setting up a manual testing environment from scratch). If modules have to be enabled, you will need to specify them. If something has to be configured, you'll have to write code in the test to do it, because none of the configuration on your current site is in the magically created Drupal instance that we're testing. None of the files in your files directory are there, none of the optional modules are installed, none of the users are created.



Never! Nope, not in assertion messages, not for button labels, not for the text you assert on the page. You always want to test the literal string on the page, you don't want to test the Drupal translation system.

[/CONVERT]

# 2- Arrangements

First, make sure your DDEV project is up and running. You’ll be executing PHPUnit from within the web container and accessing the database container for Drupal’s Kernel, Functional, and FunctionalJavascript tests.

 Thumbnail

We care about modifying two environment variables

    SIMPLETEST_BASE_URL, this defaults to http://localhost and is used as the URL for the browser emulators in Functional and FunctionalJavascript tests
    SIMPLETEST_DB, this is the database connection string and is required to run any of Drupal’s test suites beyond unit

Edit the entry for SIMPLETEST_BASE_URL and set it to http://localhost. For SIMPLETEST_DB we need to pass a connection string formatted as mysql://username:password@localhost/databasename.


https://glamanate.com/blog/running-drupals-phpunit-test-suites-ddev


 ddev describe
 ddev ssh
 composer require phpunit/phpunit
 
 Could not use "\Drupal\Tests\Listeners\HtmlOutputPrinter" as printer: class does not exist
 PHPUnit 9.1.1 
 
 https://www.drupal.org/project/drupal/issues/3063887
 
 composer require phpunit/phpunit:^7
 phpunit 7.5.20

 
 Class "Symfony\Bridge\PhpUnit\SymfonyTestsListener" does not exist

composer require symfony/phpunit-bridge:^3.4.3
composer require behat/behat:^3.4
composer require behat/mink:^1.8
composer require behat/mink-goutte-driver:^1.2


# 3- The BrowserTestBase class



# 4- Basic scaffolding 




# 5- Your tests



# 6- Running the test



# 7- Read More

* [PHPUnit Browser Test tutorial in Drupal.org](https://www.drupal.org/node/2783189)  
* [Automated Test using PHPUnit and Nightwatch.js in Drupal.org](https://api.drupal.org/api/drupal/core%21core.api.php/group/testing/8.8.x)  
* [Running PHPUnit tests in Drupal, from Drupal.org documentation](https://www.drupal.org/node/2116263)  

# 8- :wq! 



##### Recommended song

{{< youtube bGISz52QGEE >}}



1- The BrowserTestBase


2- Boilerplate code: 
namespace Drupal\Tests\rules\Functional;

use Drupal\Tests\BrowserTestBase;

/**
 * Tests that the Rules UI pages are reachable.
 *
 * @group rules_ui
 */
class UiPageTest extends BrowserTestBase {

}

3- List of modules required for the test
/**
   * Modules to enable.
   *
   * @var array
   */
  protected static $modules = ['node', 'rules'];


4- setUp()
Note, that if you implement setUp()-method, start with executing the parent::setUp()-method like in the example.


5- Create specific test

Now we need to create specific tests to exercise the module. We just create methods of our test class, each of which exercises a particular test. All methods should start with 'test' in lower-case. Any method, with public visibility, that starts this way will automatically be recognized by PHPUnit and run when requested.

Important:

Each test function will have a completely new Drupal instance to execute tests. This means that whatever you have created in a previous test function will not be available anymore in the next.



6- Secuence

Most tests will follow this pattern:
Do a $this->drupalGet('some/path') to go to a page
Use $this->clickLink(..) to navigate by links on the page
Use $this->getSession()->getPage()->fillField(...); to fill out form fields
Submit forms with $this->getSession()->getPage()->pressButton(...); or use $this->submitForm(...); (or use the deprecated drupalPostForm() method)
Do one or more assertions to check that what we see on the page is what we should see.


7 - Assertions

There are dozens of possible assertions.

8- Running the test


