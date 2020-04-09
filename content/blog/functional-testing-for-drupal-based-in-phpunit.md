---
title: "Functional testing for Drupal 8|9 based in PHPUnit"
date: 2020-03-17
draft: true

# post thumb
image: "images/post/davidjguru_drupal_thinking_about_drupal_migrations_examples_main.jpg"

# meta description
description: "Thinking about Drupal Migrations (II) - Examples"

# taxonomies
categories: 
  - "Migrations"  
tags:
  - "Drupal Migrations"
  - "Migrate API"
  - "Contrib Modules"
  - "Drupal Plugins"
  - "ETL"

# post type
type: "post"
---


Functional Testing in Drupal 8


https://www.drupal.org/node/2783189


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


