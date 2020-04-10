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
Well, as I said in the obligatory introductory paragraph, everything related to Testing is extensive, very broad and combines an ungraspable conjunction of philosophical-theoretical elements with practical-technical issues...so I guess that's why I was so happy when in the context of the migration to Drupal 8|9 of [the contributed module humans.txt](https://www.drupal.org/project/humanstxt), [Pedro Cambra](https://www.drupal.org/u/pcambra) as its maintainer proposed [in an issue](https://www.drupal.org/project/humanstxt/issues/3123126) to provide the module with a certain type of test.   
It was a great opportunity to use it as a simple, didactic and intuitive approach.   

About the testing we are going to see in this article, it is important to situate it and give it context: we will make some types of browser tests, which are part of the test types that come from PHPUnit classes and resources. How are they related? Let's see this introduction made by [James G. Robertson](https://twitter.com/jamesgrobertson) in the [Atlantic BT](https://twitter.com/atlanticbt) website:

> *PHPUnit can handle different types of tests in Drupal core: unit tests, kernel tests, and functional tests. With these three tests, you can confirm the quality and reaction of code on edge cases in different layers. Unit tests test functions, methods, and classes without requiring a database connection to run. On the other hand, kernel tests test the integration of module APIs and require a database connection to be configured to run. Functional tests test the entire system and require more setup than the others. Within the functional tests, there are both Browser and JavaScript tests. In addition to these PHP-based tests, you may also run core JavaScript tests using the Nightwatch framework.*

* Source: [Atlantic BT, A full guide to phpunit in Drupal 8](https://www.atlanticbt.com/insights/a-full-guide-to-phpunit-in-drupal-8/).

Right, so what we're going to do in this case has to do with testing actions to be performed in the web interface of our Drupal installation but running through code using classes that already provide methods for replicating *"manual actions"*. Intuitively... What can we think that we will need? Let's advance a possible outline of our possible actions: 
 
1. Maybe, loading a URL.  
2. Perhaps, pressing buttons on a form.  
3. We may need to load values into this former form.  
4. Or also check users, roles and permissions.

So we'll need classes and resources that allow us to reproduce these actions through code (so that they can be automated). This is just a sketch of our possible needs, as we must first be clear about which features we want to test. So the first guideline shared in this introduction will be: **You must know well your needs to test**.


# 2- Arrangements
Well, in this section I've included the fun little story about how I discovered that I didn't have the necessary resources installed in the test environment I chose. Pay attention.  

It all started when I switched environments and realized that I didn't have phpunit installed...I always use DDEV ([Get more info about DDEV](https://www.therussianlullaby.com/tags/ddev/)) as a tool for creating local development environments and in this case I switched to one that *didn't have* a Drupal installation with the --dev resources. So if this is your case, take advantage and review the following steps.

**Environment**

* First, stop your local apache, if exists: ```sudo /etc/init.d/apache2 stop```  
* Second, up with your DDEV project: ```ddev start```. Make sure your DDEV project is up, running and functioning normally. You're going to execute PHPUnit from within the DDEV main web container and you'll use the db container too.   
* Third, go to the web container and install with composer the next resources: 
   ```toml
  ddev ssh
  composer require phpunit/phpunit
  ```
**Note:** In my first iteration, I launched one request like this (opening the phpunit's version to the latest available) and it caused me a lot of problems when trying to use PHPUnit:

 ```text
 Could not use "\Drupal\Tests\Listeners\HtmlOutputPrinter" as printer: class does not exist
 PHPUnit 9.1.1 
```
 **What's the problem?** 
Drupal 8.x is not compatible with PHPUnit in that version (In fact there'are some issues proposing an update to PHPUnit 8 for Drupal 9, [like this](https://www.drupal.org/project/drupal/issues/3063887)), so it's better to uninstall it and request a slightly more limited version, which is not the last one, something around PHPUnit 7 for example. 

For the rest of the dependencies, I recognize that I was playing to solve them just following the successive error messages trying to launch several tests again, like ```Class "Symfony\Bridge\PhpUnit\SymfonyTestsListener" does not exist``` and many others. In summary I have installed all the following resources in the approximate versions indicated: 

```toml
composer remove phpunit/phpunit
composer require phpunit/phpunit:^7 # In my case, this installed phpunit 7.5.20
composer require symfony/phpunit-bridge:^3.4.3
composer require behat/behat:^3.4
composer require behat/mink:^1.8
composer require behat/mink-goutte-driver:^1.2
```

**PHPUnit in Drupal Core**

The next step was adjust the use of PHPUnit from the file provided by Drupal, present in ```/project/web/core/phpunit.xml.dist```. It's necessary to make a copy of the file and rename it as ```phpunit.xml``` simply, leaving this copy in the same location ```/core```. 

Now we need modify two environment variables within the copied file. Beware: 

```text
    <!-- Example SIMPLETEST_BASE_URL value: http://localhost -->
    <env name="SIMPLETEST_BASE_URL" value="http://localhost"/>
    <!-- Example SIMPLETEST_DB value: mysql://username:password@localhost/databasename#table_prefix -->
    <env name="SIMPLETEST_DB" value="mysql://db:db@db/db"/>
```
As you can see, now we need an internal URL and all the connection data to your database. Ok, in my case as I'm using DDEV I can extract the information from my prompt, just launching ```ddev describe```. Remember that we're looking for a string formatted as:  mysql://username:password@localhost/databasename. And in the DDEV context, by default, all these data are the same: ```db```. Let's see:  

![Showing values from a DDEV installation for Drupal](../../images/post/davidjguru_functional_testing_for_drupal_based_in_phpunit_db.jpg)

Like an extra, I've configured a folder to save results from tests in a HTML format using the variables: 

```text
 <env name="BROWSERTEST_OUTPUT_DIRECTORY" value="/var/www/html/web/sites/default/simpletest/browser_output/"/>
 <env name="BROWSERTEST_OUTPUT_BASE_URL" value="http://migrations.ddev.site"/>
```
Where ```migrations.ddev.site``` is just the name of the DDEV project that I'm using. This will write all the output from the executed test in the directory using HTML format, and so you can see the pages that the browser visited during the test. Theses pages are saved as files and linked under the URL.  

You can see my whole configuration for PHPUnit using DDEV here in the next gist: 

  {{< gist davidjguru 2d59eed50818f74710ae3b0f87fb947d >}}

So the second guideline shared in this section will be: **tune up your environment well**. 

When you have available all the libraries and dependencies along with the configuration of the phpunit.xml file, you can try to run the tests that bring many modules through a console execution instruction.  
The format of the instruction we will use will be related to the position for ourselves within the path /project/web/ and launch the call to phpunit in a format like this: 

```text
../vendor/bin/phpunit -c core modules/contrib/config_inspector 
``` 
Where the param ```-c``` points to the localization of the phpunit.xm file and it will run all the test located in the marked direction (in this example will be executed tests from the [Config Inspector Contrib Module](https://www.drupal.org/project/config_inspector)). 

![Executing test from the Config Inspector Contrib Module](../../images/post/davidjguru_functional_testing_for_drupal_based_in_phpunit_launch_test.png)


# 3- The BrowserTestBase class
Now we are going to deal with a fundamental PHP class for this test case we are going to make, [the BrowserTestBase.php class](https://api.drupal.org/api/drupal/core%21tests%21Drupal%21Tests%21BrowserTestBase.php/class/BrowserTestBase/8.8.x) of the Drupal core test context, path: ```core/tests/Drupal/Tests/BrowserTestBase.php``` (Don't confuse it with [an already deprecated version from the context of the simpletest core module called so BrowserTestBase](https://api.drupal.org/api/drupal/core%21modules%21simpletest%21src%21BrowserTestBase.php/class/BrowserTestBase/8.8.x)). 

This abstract class extends the TestCase class from PHPUnit (one of the main reasons why we need the use of PHPUnit here) and uses a lot of PHP traits providing a lot of methods from different origins, finally grouped by this base class that we'll have to extend for our goals. In general terms, we know that abstract classes are classes that are not instantiated and can only be inherited, thus transferring an obligatory functioning to the daughter classes. In this example, using BrowserTestBase we'll have nearly forty methods available within the class (there are dozens of possible assertions) to perform specific checks on actions to be performed through the browser. 

**The info from the class is very explicit:** You must use the class for functional Drupal test (ok), You have to put your new classes extending BrowserTestBase as a base class in path: ```/modules/custom/your_custom_module/test/src/Functional``` and avoid using t() function for translate unless you're testing translation functionality. 


´´´text
/**
 * Provides a test case for functional Drupal tests.
 *
 * Tests extending BrowserTestBase must exist in the
 * Drupal\Tests\yourmodule\Functional namespace and live in the
 * modules/yourmodule/tests/src/Functional directory.
 *
 * Tests extending this base class should only translate text when testing
 * translation functionality. For example, avoid wrapping test text with t()
 * or TranslatableMarkup().
 *
 * @ingroup testing
 */
´´´



# 4- Basic scaffolding 




# 5- Your tests



# 6- Running the test



# 7- Read More

* [PHPUnit Browser Test tutorial in Drupal.org](https://www.drupal.org/node/2783189)  
* [Automated Test using PHPUnit and Nightwatch.js in Drupal.org](https://api.drupal.org/api/drupal/core%21core.api.php/group/testing/8.8.x)  
* [Running PHPUnit tests in Drupal, from Drupal.org documentation](https://www.drupal.org/node/2116263)  
* [Running Drupal's PHPUnit test suites on DDEV, by Matt Glaman](https://glamanate.com/blog/running-drupals-phpunit-test-suites-ddev)  

# 8- :wq! 



##### Recommended song

{{< youtube bGISz52QGEE >}}




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


