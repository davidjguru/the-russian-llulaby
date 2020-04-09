---
title: "PHPUnit Browser testing for Drupal 8|9"
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
[2- The BrowserTestBase class](#2--the-browsertestbase-class)  
[3- Basic Scaffolding](#3--basic-scaffolding)  
[4- Your tests](#4--your-tests)  
[5- Running the test](#5--running-the-test)
[6- Read More](#6--read-more)  
[7- :wq!](#7--wq)    
<!-- /TOC -->

-------------------------------------------------------------------------------

# 1- Introduction 
Well, as I said in the obligatory introductory paragraph everything related to Testing is extensive, very broad and combines an ungraspable conjunction of philosophical-theoretical elements with practical-technical issues...so I guess that's why I was so happy when in the context of the migration to Drupal 8|9 of [the contributed module humans.txt](https://www.drupal.org/project/humanstxt), [Pedro Cambra](https://www.drupal.org/u/pcambra) as its maintainer proposed [in an issue](https://www.drupal.org/project/humanstxt/issues/3123126) to provide the module with a certain type of test.   
It was a great opportunity to use it as a simple, didactic and intuitive approach. 




# 2- The BrowserTestBase class



# 3- Basic scaffolding 




# 4- Your tests



# 5- Running the test



# 6- Read More

* [PHPUnit Browser Test tutorial in Drupal.org](https://www.drupal.org/node/2783189)  
* [Automated Test using PHPUnit and Nightwatch.js in Drupal.org](https://api.drupal.org/api/drupal/core%21core.api.php/group/testing/8.8.x)  
* [Running PHPUnit tests in Drupal, from Drupal.org documentation](https://www.drupal.org/node/2116263)  



# 7- :wq! 



##### Recommended song

{{< youtube bGISz52QGEE >}}





It's terribly important to realize that each test runs in a completely new Drupal instance, which is created from scratch for the test. In other words, none of your configuration and none of your users exists! None of your modules are enabled beyond the default Drupal core modules. If your test sequence requires a privileged user, you'll have to create one (just as you would if you were setting up a manual testing environment from scratch). If modules have to be enabled, you will need to specify them. If something has to be configured, you'll have to write code in the test to do it, because none of the configuration on your current site is in the magically created Drupal instance that we're testing. None of the files in your files directory are there, none of the optional modules are installed, none of the users are created.



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


