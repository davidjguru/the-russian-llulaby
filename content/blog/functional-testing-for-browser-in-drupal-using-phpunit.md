---
title: "Functional testing for Browser in Drupal 8|9 using PHPUnit"
date: 2020-04-10
draft: false

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
Everything around Test-driven development (TDD) is a very interesting and very motivating world and besides, these are topics with a certain antiquity. So it is very easy to find related contents. But the relationship between this area and Drupal is even more interesting: there are multiple options to implement testing of different types and different orientation (Unit Test, Kernel, JavaScript, functional ...) so it can be complex to introduce in this world. Today I want to take advantage of the experience of porting to Drupal 8 a small contributed module to share examples about browser-focused Functional Testing in Drupal 8 (or Drupal 9).  We will learn together with simple use cases. It may be a small slice (Functional Testing of a very small features) of a big cake (Testing in Drupal), but it will be a nice entry point. Follow me. 

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

Ok, and what is the features we'll have to test? the Humans.txt contrib module was created years ago to offers a way for building the humans.txt file from within Drupal. For several months we have been working on its portability to Drupal 8 and its features are summarized in two main tasks:   

1. Generating an object as a humans.txt file with values loaded from a configuration form.
1. Offering to create a link to the object/file in the ```<head>``` section of pages. 

Mainly, these will be the features that we will have to test. 


# 2- Arrangements
Well, in this section I've included the fun little story about how I discovered that I didn't have the necessary resources installed in the test environment I chose. Pay attention.  

It all started when I switched environments and realized that I didn't have phpunit installed...I always use DDEV ([- Get more info about DDEV - ](https://www.therussianlullaby.com/tags/ddev/)) as a tool for creating local development environments and in this case I switched to one that *didn't have* a Drupal installation with the --dev resources. So if this is your case, take advantage and review the following steps.

**Environment**

* First, stop your local apache, if exists: ```sudo /etc/init.d/apache2 stop```  
* Second, up with your DDEV project: ```ddev start```. Make sure your DDEV project is up, running and functioning normally. You're going to execute PHPUnit from within the DDEV main web container and you'll use the db container too.   
* Third, go to the web container and install with composer the next resources:   

   ```toml
  ddev ssh
  composer require phpunit/phpunit
  ```
**Note:** In my first iteration, I launched one request like this (opening the phpunit's version to the latest available) and it caused me a lot of problems when trying to use PHPUnit:  

 ```toml
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

```
    <!-- Example SIMPLETEST_BASE_URL value: http://localhost -->
    <env name="SIMPLETEST_BASE_URL" value="http://localhost"/>
    <!-- Example SIMPLETEST_DB value: mysql://username:password@localhost/databasename#table_prefix -->
    <env name="SIMPLETEST_DB" value="mysql://db:db@db/db"/>
```
As you can see, now we need an internal URL and all the connection data to your database. Ok, in my case as I'm using DDEV I can extract the information from my prompt, just launching ```ddev describe```. Remember that we're looking for a string formatted as:  mysql://username:password@localhost/databasename. And in the DDEV context, by default, all these data are the same: ```db```. Let's see:  

![Showing values from a DDEV installation for Drupal](../../images/post/davidjguru_functional_testing_for_drupal_based_in_phpunit_db.jpg)

Like an extra, I've configured a folder to save results from tests in a HTML format using the variables: 

```
 <env name="BROWSERTEST_OUTPUT_DIRECTORY" value="/var/www/html/web/sites/default/simpletest/browser_output/"/>
 <env name="BROWSERTEST_OUTPUT_BASE_URL" value="http://migrations.ddev.site"/>
```
Where **BROWSERTEST_OUTPUT_DIRECTORY** is used as the directory where the output data will be saved by PHPUnit and needs to be an absolute local path, in my case the result HTML from test goes to  
```/var/www/html/web/sites/default/simpletest/browser_output/``` and while **BROWSERTEST_OUTPUT_BASE_URL** help to register the future links to the saved output, in my case is: ```migrations.ddev.site``` and is just the name of the DDEV project that I'm using. This will write all the output from the executed test in the directory using HTML format, and so you can see the pages that the emulated browser visited during the test. Theses HTML pages are saved as files and linked under the URL.  

When you launch the test, Drupal 8 will use a simulated browser to execute actions and check assertions, is like an emulator offered by Mink, just a pure headless browser from the installed dependency [behat/mink](https://packagist.org/packages/behat/mink).   
Now remember that you must ensure the write permissions in the destiny folder for the user that will launch the test. You can see my whole configuration for PHPUnit using DDEV here in the next gist: 

  {{< gist davidjguru 2d59eed50818f74710ae3b0f87fb947d >}}

So the second guideline shared in this section will be: **tune up your environment well**. 

When you have available all the libraries and dependencies along with the configuration of the phpunit.xml file, you can try to run the tests that bring many modules through a console execution instruction.  
The format of the instruction we will use will be related to the position for ourselves within the path /project/web/ and launch the call to phpunit in a format like this:  

```
../vendor/bin/phpunit -c core modules/contrib/config_inspector 
``` 

Where the param ```-c``` points to the localization of the phpunit.xm file and it will run all the test located in the marked direction (in this example will be executed tests from the [Config Inspector Contrib Module](https://www.drupal.org/project/config_inspector)). 

![Executing test from the Config Inspector Contrib Module](../../images/post/davidjguru_functional_testing_for_drupal_based_in_phpunit_launch_test.png)


# 3- The BrowserTestBase class
Now we are going to deal with a fundamental PHP class for this test case we are going to make, [the BrowserTestBase.php class](https://api.drupal.org/api/drupal/core%21tests%21Drupal%21Tests%21BrowserTestBase.php/class/BrowserTestBase/8.8.x) of the Drupal core test context, path: ```/core/tests/Drupal/Tests/BrowserTestBase.php``` (Don't confuse it with [an already deprecated version from the context of the simpletest core module called so BrowserTestBase](https://api.drupal.org/api/drupal/core%21modules%21simpletest%21src%21BrowserTestBase.php/class/BrowserTestBase/8.8.x)). 

This abstract class extends the TestCase class from PHPUnit (one of the main reasons why we need the use of PHPUnit here) and uses a lot of PHP traits providing many methods from different origins, finally grouped by this base class that we'll have to extend for our goals. In general terms, we know that abstract classes are classes that are not instantiated and can only be inherited, thus transferring an obligatory functioning to the daughter classes. In this example, using BrowserTestBase we'll have nearly forty methods available within the class (there are dozens of possible assertions) to perform specific checks on actions to be performed through the browser. 

**The info from the class is very explicit:** You must use the class for functional Drupal test (ok), You have to put your new classes extending BrowserTestBase as a base class in path: ```/modules/custom/your_custom_module/test/src/Functional``` and avoid using t() function for translate unless you're testing translation functionality. 


```
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
```

Remember what we said before about knowing well what kind of actions you want to test? Think that this class will provide us with functions that will align possible actions with concrete methods to reproduce mechanics through code.  
For example:  

* Your test needs to create new users? use the [drupalCreateUser() method](https://api.drupal.org/api/drupal/core%21modules%21user%21tests%21src%21Traits%21UserCreationTrait.php/function/UserCreationTrait%3A%3AcreateUser/8.8.x).
* Your test needs to login to the Drupal installation? use the [drupalLogin() method](https://api.drupal.org/api/drupal/core%21tests%21Drupal%21Tests%21UiHelperTrait.php/function/UiHelperTrait%3A%3AdrupalLogin/8.8.x).
* Your test needs to check if a form field is being rendered? you can use: [assertSession()->fieldExists('field_machine_name')](https://api.drupal.org/api/drupal/vendor%21behat%21mink%21src%21WebAssert.php/function/WebAssert%3A%3AfieldExists/8.8.x). 

As you see, **the possibilities are many and only require that we have some knowledge of the capabilities that the BrowserTestBase class offers to automate our tests**.


# 4- Basic scaffolding 
I will now share some actions that I need to do before I can start testing functions. As my goal is to prepare test for the contrib module "Humans.txt" the first thing I will do is download the module, install it and make sure I'm in the last commit of the branch I'm interested in testing (the one in version 8.x).  

As I'm in a DDEV based context the first thing I'll do is to access my web container and from there perform the rest of initial commands:  

```toml
ddev ssh
composer require drupal/humanstxt
drupal moi humanstxt
cd modules/contrib/humanstxt
git checkout 8.x-1.x
```

Now I'm in the initial point for my job. Another aspect that I must evaluate is if it is convenient (or not) to create a submodule inside Humanstxt as a "test", with its own installation and testing paths that respond to the same controllers as the main module, or if on the contrary it's excessive (for now) for the testing of this module. At this moment I think I will create the tests directly, testing against the resources of the main module. 


# 5- Your tests
In our current context, using a new class HumansTxtBasicText which extends BrowserTestBase, every method inside our class will be a unique test by itself, but there will a lot of assertions more to check, cause of in one of our used functions, we're calling some internal assertions. For example, if we're using the function ```drupalCreateUser(['permission name'])``` from the [UserCreationTrait](https://api.drupal.org/api/drupal/core%21modules%21user%21tests%21src%21Traits%21UserCreationTrait.php/trait/UserCreationTrait/8.8.x) and originally named as ```createUser``` , with our direct assertions, we're going to check other internals just like:  

```toml
$valid_user = $account->id() !== NULL;
$this->assertTrue($valid_user, new FormattableMarkup('User created with name %name and pass %pass', ['%name' => $edit['name'], '%pass' => $edit['pass']]), 'User login');
```

Due to this, you'll see more assertions than yours (or the explicit yours), like this feedback when I'm only using four explicit assertions:

```
Testing modules/contrib/humanstxt
..                                     2 / 2 (100%)

Time: 1.12 minutes, Memory: 4.00 MB

OK (2 tests, 16 assertions)
```
Ok, don't fear. Let's move on.

The next important point to know is that **all your methods should start with the word 'test' in lowercase**. So any method using this as prefix with public visibility will be automatically detected by PHPUnit and will be launched too. 

And there's one more important thing: **any stuff you make within a method/test, won't live outside it**. So, for example if in a test you're creating a new user, then in the next test you'll have to create it again. Ok? That's because every time you run a method/test, each test function will have a full new Drupal instance to execute tests.

Thinking about how to do it in an organized way, I thought about doing it in three functional blocks: 

1. First, check the access control to the configuration form and its fields, that is, check the behavior for different roles (administrators, users with basic and anonymous permissions).

2. Then, check complementary questions with the information collected in the headers of the answers to requests or the cache tags stored. 

3. Finally, test if the file is created, kept accessible and its link is located in the right way. Everything for different user profiles: administrators, basic and anonymous permissions. 

So, I'll create an new folder within the module with path: ```humanstxt/test/src/Functional/```, using a new class called ```HumansTxtBasicTest.php``` with namespace: ```Drupal\Tests\humanstxt\Functional```.

## Phase one: Checking access control
We're going to test if three different users, one with admin permissions, other as a basic user without specific permissions and a last anonymous user can reach the configuration page for the Humans.txt module. Theoretically, only the admin user can access and the basic user or the anonymous cannot reach the config page. Let's see.

First I'm gonna test the access for admins:

```
  /**
   * Checks if an admin user can access to the configuration page.
   */
  public function testHumansTxtAdminAccess() {
    // Build initial paths.
    $humanstxt_config = Url::fromRoute('humanstxt.admin_settings_form', [], ['absolute' => FALSE])->toString();

    // Create user for testing.
    $this->adminUser = $this->drupalCreateUser(['administer humans.txt']);

    // Login for the former admin user.
    $this->drupalLogin($this->adminUser);

    // Access to the path of humanstxt config page.
    $this->drupalGet($humanstxt_config);

    // Check the response returned by Drupal.
    $this->assertResponse(200);
  }
```

I'm using the [Url class](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Url.php/class/Url/8.8.x) cause I don't like work with explicit paths in testing. So, if a path changes in a routing.yml file, the test continues being valid and it will be run normally.

And then, I'll check the access for the pair of users no-admin, using the same test: 

```
  /**
   * Checks if a non-administrative user cannot access to the config page.
   */
  public function testHumansTxtUserNoAccess() {
    // Build initial path.
    $humanstxt_config = Url::fromRoute('humanstxt.admin_settings_form', [], ['absolute' => FALSE])->toString();

    // Create user for testing.
    $this->normalUser = $this->drupalCreateUser(['access content']);

    // Login for the former basic user.
    $this->drupalLogin($this->normalUser);

    // Try access to the path of humanstxt config page.
    $this->drupalGet($humanstxt_config);

    // Check the response returned by Drupal.
    $this->assertResponse(403);

    // Logout as normal user and repeat the former cycle as anonymous user.
    $this->drupalLogout();
    $this->drupalGet($humanstxt_config);
    $this->assertResponse(403);
  }
```

Now It's time to check the access to the fields of the configuration form.

```
  /**
   * Checks if an administrator can see the fields.
   */
  public function testHumansTxtAdminFields() {
    // Build initial path.
    $humanstxt_config = Url::fromRoute('humanstxt.admin_settings_form', [], ['absolute' => FALSE])->toString();

    // Create user for testing.
    $this->adminUser = $this->drupalCreateUser(['administer humans.txt']);

    // Login for the the former admin user.
    $this->drupalLogin($this->adminUser);

    // Access to the path of humanstxt config page.
    $this->drupalGet($humanstxt_config);

    // The textarea for configuring humans.txt is shown.
    $this->assertSession()->fieldExists('humanstxt_content');

    // The checkbox for configuring the creation of the humanstxt link is shown.
    $this->assertSession()->fieldExists('humanstxt_display_link');
  }

  /**
   * Checks if a non-administrative user cannot use the configuration page.
   */
  public function testHumansTxtUserFields() {
    // Build initial path.
    $humanstxt_config = Url::fromRoute('humanstxt.admin_settings_form', [], ['absolute' => FALSE])->toString();

    // Create user for testing.
    $this->normalUser = $this->drupalCreateUser(['access content']);

    // Login for the former basic user.
    $this->drupalLogin($this->normalUser);

    // Access to the path of humanstxt config page.
    $this->drupalGet($humanstxt_config);

    // The textarea is not shown for basic users.
    $this->assertNoFieldById('edit-humanstxt-content', NULL);

    // The checkbox is not shown for basic users.
    $this->assertNoFieldById('edit-humanstxt-display-link', NULL);
  }
```

## Phase Two: Checking Complementary Items
In this block I would like to check stuff like the headers of the returned object/file or the cache tags.

```
  /**
   * Checks if the header is right.
   */
  public function testHumansTxtHeader() {
    // Build initial path.
    $humanstxt_path = Url::fromRoute('humanstxt.content', [], ['absolute' => FALSE])->toString();

    // Access to the path of humans.txt object/file.
    $this->drupalGet($humanstxt_path);

    // Check the returned response.
    $this->assertResponse(200);

    // Check if the file was served as text/plain with charset=UTF-8.
    $this->assertHeader('Content-Type', 'text/plain; charset=UTF-8');
  }

  /**
   * Checks if cache tags exists.
   */
  public function testHumansTxtCacheTags() {
    // Build initial path.
    $humanstxt_path = Url::fromRoute('humanstxt.content', [], ['absolute' => FALSE])->toString();

    // Access to the path of humans.txt object/file.
    $this->drupalGet($humanstxt_path);

    // Check the returned response.
    $this->assertResponse(200);

    // Check the related cache tag.
    $this->assertCacheTag('humanstxt');
  }
```
## Phase Three: Checking the delivered content

Ok, in order to check the content file or the link write in the <head> section, I thought to make a central and unified test that would group everything for the different user profiles to be tested. What kind of things am I interested in checking? Well I would like to test if the access to the configuration form is possible for administrators (although this was already tested in a previous test) and then creating a new configuration of the Humans.txt in order to test if the saved content corresponds with the one visible inside the object/file (testing also the access to the file).   
Finally I would like to test if the link associated to the meta tag in the <head> section of the pages is being loaded, choosing one at random. 

```
  /**
   * Checks if humans.txt file is delivered for Different Users as was configured.
   */
  public function testHumansTxtConfigureHumansTxtDifferentUsers() {
    // Build initial paths.
    $humanstxt_config = Url::fromRoute('humanstxt.admin_settings_form', [], ['absolute' => FALSE])->toString();
    $humanstxt_path = Url::fromRoute('humanstxt.content', [], ['absolute' => TRUE])->toString();
    $humanstxt_link = '<link rel="author" type="text/plain" hreflang="x-default" href="' . $humanstxt_path . '">';

    // Create users for testing.
    $this->adminUser = $this->drupalCreateUser(['administer humans.txt']);
    $this->normalUser = $this->drupalCreateUser(['access content']);

    // Login as admin and get the config form page.
    $this->drupalLogin($this->adminUser);
    $this->drupalGet($humanstxt_config);

    // Load a new configuration for Humans.txt file and submit config Form.
    $test_string = "# Testing Humans.txt {$this->randomMachineName()}";
    $this->submitForm(['humanstxt_content' => $test_string, 'humanstxt_display_link' => TRUE], t('Save configuration'));

    // Check the object/file created.
    $this->drupalGet($humanstxt_path);
    $this->assertResponse(200);

    // Test header.
    $this->assertHeader('Content-Type', 'text/plain; charset=UTF-8');

    // Get page content.
    $content = $this->getSession()->getPage()->getContent();

    // Test the assert- if exists the test_string in the content.
    $this->assertTrue($content == $test_string, sprintf('Test string [%s] is shown in the configured humans.txt file [%s].', $test_string, $content));

    // Test if the link to the object/file is in HTML <head> section for Admins.
    $this->drupalGet($humanstxt_config);
    $this->assertResponse(200);
    $tags = $this->getSession()->getPage()->getHtml();
    $this->assertStringContainsString($humanstxt_link, $tags, sprintf('Test link [%s] is shown in the HTML -head- section from [%s].', $humanstxt_link, $tags));

    // Logout as admin and login as normal user.
    $this->drupalLogout();
    $this->drupalLogin($this->normalUser);

    // Repeat the previous cycle now as normal user.
    $this->drupalGet($humanstxt_path);
    $this->assertResponse(200);
    $this->assertHeader('Content-Type', 'text/plain; charset=UTF-8');
    $content = $this->getSession()->getPage()->getContent();
    $this->assertTrue($content == $test_string, sprintf('Test string [%s] is shown in the configured humans.txt file [%s].', $test_string, $content));
    $this->drupalGet($humanstxt_config);
    $this->assertResponse(403);
    $tags = $this->getSession()->getPage()->getHtml();
    $this->assertStringContainsString($humanstxt_link, $tags, sprintf('Test link [%s] is shown in the HTML -head- section from [%s].', $humanstxt_link, $tags));

    // Logout as normal user.
    $this->drupalLogout();

    // Now a third iteration as anonymous user.
    $this->drupalGet($humanstxt_path);
    $this->assertResponse(200);
    $this->assertHeader('Content-Type', 'text/plain; charset=UTF-8');
    $content = $this->getSession()->getPage()->getContent();
    $this->assertTrue($content == $test_string, sprintf('Test string [%s] is shown in the configured humans.txt file [%s].', $test_string, $content));
    $this->drupalGet($humanstxt_config);
    $this->assertResponse(403);
    $tags = $this->getSession()->getPage()->getHtml();
    $this->assertStringContainsString($humanstxt_link, $tags, sprintf('Test link [%s] is shown in the HTML -head- section from [%s].', $humanstxt_link, $tags));
  }
```
To check the insertion of the <link> tag in <head> I've used a double version of the former codeblock, playing with the values of the element from the Configuration Form: ```'humanstxt_display_link' => FALSE``` and changing it for the next codeblock. This checkbox when false doesn't insert the link to the humans.txt object/file in <head>, and set to TRUE it will put the tag. The occurrence of the mentioned tag in <head> is managed using a special kind of assertion method from PHPUnit called [assertStringContainsString](https://phpunit.readthedocs.io/en/7.5/assertions.html#assertstringcontainsstring), available in my installed version of the testing framework (7.5) and its inverse for negative versions: assertStringNotContainsString().  

So my idea is using the [getSession() method from BrowserTestBase](https://api.drupal.org/api/drupal/core%21tests%21Drupal%21Tests%21BrowserTestBase.php/function/BrowserTestBase%3A%3AgetSession/8.8.x) class which returns a [Mink Session Object](https://api.drupal.org/api/drupal/vendor%21behat%21mink%21src%21Session.php/class/Session/8.8.x). This session object from Mink offers a method called [getPage()](https://api.drupal.org/api/drupal/vendor%21behat%21mink%21src%21Session.php/function/Session%3A%3AgetPage/8.8.x) that can return an object [DocumentElement](https://api.drupal.org/api/drupal/vendor%21behat%21mink%21src%21Element%21DocumentElement.php/class/DocumentElement/8.8.x) and use its method [getHtml()](https://api.drupal.org/api/drupal/vendor%21behat%21mink%21src%21Element%21Element.php/function/Element%3A%3AgetHtml/8.8.x) that returns al the HTML code formatted as string. All of this is in the line:  
```$tags = $this->getSession()->getPage()->getHtml();``` and in the variable $tags I'll save all the HTML response ready to search the link, using the variable as haystack. 
In any case, all the HTML code from a page is too much for processing, and we don't need to get all the HTML code, so we'll get a substring cutting the output from the getHtml() method, extracting up to two thousand characters, enough to evaluate the whole <head> section.

```
// All the HTML code is too much we just need to inspect the <head> section.
$tags = substr($this->getSession()->getPage()->getHtml(), 0, 2020);
$this->assertStringContainsString($humanstxt_link, $tags, sprintf('Test link: [%s] is NOT shown in the head section from [%s] and this shouldn\'t happen.', $humanstxt_link, $tags));
```

**Finally you can see this first version of the TestClass as Gist in Github**. After uploading the patch to the issue there will surely be revisions and changes to the patch, so I promise to link the final version of the Test class that will be committed to the 8.x-1.x branch. 

  {{< gist davidjguru 589ab794e974a15699ed6fea683783f1 >}}

# 6- Running the test

Well, and now with our test stored and the phpunit configuration initially resolved, it's time to run our test and observe the results. To do this, in my case I'm located in ```/project/web/``` and with phpunit.xml placed in ```/project/web/core/```, I launch the instruction:  

```
../vendor/bin/phpunit -c core modules/contrib/humanstxt
```

Which throws all the test located inside the humanstxt contrib module...

![Executing test from the Humans.txt Contrib Module](../../images/post/davidjguru_functional_testing_for_drupal_based_in_phpunit_results.png)

As we can see, we're executing eight tests with hundred and six assertions. Due to our phpunit.xml configuration, these test are writing over sixty four HTML files based in results using the browser emulator from Mink, thanks to which you will be able to reproduce certain actions by opening the files in your web browser.

In the next caption we can see the HTML output nº64 generated from one of last assertions in the code, just when an anonymous user try to get the Humanstxt configuration form and receive and error message with code HTTP 403 'Forbidden':

![Executing test from the Humans.txt Contrib Module](../../images/post/davidjguru_functional_testing_for_drupal_based_in_phpunit_html_view.png)

As you can see, you can move through the files using the Previous | Next Links in header, connecting all the secuence of HTML results from tests.



# 7- Read More

* [PHPUnit Browser Test tutorial in Drupal.org](https://www.drupal.org/node/2783189).  
* [Automated Test using PHPUnit and Nightwatch.js in Drupal.org](https://api.drupal.org/api/drupal/core%21core.api.php/group/testing/8.8.x).  
* [Running PHPUnit tests in Drupal, from Drupal.org documentation](https://www.drupal.org/node/2116263).  
* [Running Drupal's PHPUnit test suites on DDEV, by Matt Glaman](https://glamanate.com/blog/running-drupals-phpunit-test-suites-ddev).  
* [Mink at a glance, from Mink documentation](http://mink.behat.org/en/latest/at-a-glance.html).  
* [Developing Web Applications with Behat and Mink](https://docs.behat.org/en/v2.5/cookbook/behat_and_mink.html)  


# 8- :wq! 

##### Recommended song

{{< youtube bGISz52QGEE >}}
