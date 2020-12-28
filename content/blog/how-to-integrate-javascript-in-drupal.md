---
title: "JavaScript & Drupal: Love at first behavior (How to)"
date: 2020-11-25
draft: true

# post thumb
image: "images/post/davidjguru_drupal_9_javascript_integration_main.jpg"

# meta description
description: "How to integrate JavaScript and Drupal"

# taxonomies
categories: 
  - "Development"  
tags:
  - "Drupal Behaviors"
  - "Drupal Development"
  - "Backend"
  - "PHP"

# post type
type: "post"
---
Imagine that you have to integrate JavaScript code into your Drupal project... Where do you start? How do you do it? You're looking for information but you don't find anything "holistic", something that goes from 0 to 100 and that puts in context how the relationships between Drupal and JavaScript are structured. Well, this article was made for you (Or for other people in your team that you want to introduce to this topic).   

In this guide you will learn basic concepts of JavaScript, the terminology used in Drupal, functions, methods and common mechanics to enrich your projects by make them run with executable code on the client side. And all through a combination of theory and practice. It includes some exercises that I have integrated.  

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Magnus Engø](https://unsplash.com/@magnusengo).**

  
---------------------------------------------------------------------------------

**Index of sections**  
<!-- TOC -->  
[1-Introduction](#1--introduction)   
[2- JavaScript and Drupal: basic concepts](#2--javascript-and-drupal-basic-concepts)  
[3- How to include JavaScript code in Drupal](#3--how-to-include-javascript-code-in-drupal)  
  * [3.1- Setting up the scenario: creating a custom module](#31--setting-up-the-scenario-creating-a-custom-module)  
  * [3.2- The Library concept](#32--the-library-concept)  
    * [3.2.1- Secuence for creating libraries](#321--secuence-for-creating-libraries)  
    * [3.2.2- Loading libraries in head](#322--loading-libraries-in-head)  
    * [3.2.3- Libraries as external resources](#323--libraries-as-external-resources)  
    * [3.2.4- Libraries and dependencies](#324--libraries-and-dependencies)  
  * [3.3- The JavaScript file](#33--the-javascript-file)  
  * [3.4- Adding JavaScript Libraries](#34--adding-javascript-libraries)  
    * [3.4.1- Using the attached property in Render Arrays](#341--using-the-attached-property-in-render-arrays)  
    * [3.4.2- Libraries in a TWIG template](#342--libraries-in-a-twig-template)  
    * [3.4.3- Global libraries for a Theme](#343--global-libraries-for-a-theme)  
    * [3.4.4- Adding libraries from hooks](#344--adding-libraries-from-hooks)  
[4- Just a little bit more of JavaScript in Drupal](#4--just-a-little-bit-more-of-javascript-in-drupal)  
  * [4.1- Structure and Guidelines for IIFE](#41--structure-and-guidelines-for-iife)
  * [4.2- Passing parameters in IIFE](#42--passing-parameters-in-iife)  
  * [4.3- Passing values from PHP to JavaScript using drupalSettings](#43--passing-values-from-php-to-javascript-drupalsettings)  
  * [4.4- Changes in rendered HTML](#44--changes-in-rendered-html)  
    * [4.4.1- Counting visits using web storage](#441--counting-visits-using-web-storage)  
[5- Drupal and the old jQuery](#5--drupal-and-the-old-jquery)  
  * [5.1- Fast Review of the jQuery keys](#51--fast-review-of-the-jquery-keys)  
  * [5.2- Using jQuery in our Drupal installation](#52--using-jquery-in-our-drupal-installation)  
  * [5.3- Using a different version of jQuery](#53--using-a-different-version-of-jquery)  
[6- Drupal Behaviors](#6--drupal-behaviors)
  * [6.1- Anatomy of a Behavior](#61--anatomy-of-a-behavior)  
  * [6.2- The global object: Drupal](#62--the-global-object-drupal)  
  * [6.3- Behaviors in Drupal](#63--behaviors-in-drupal)  
[7- JavaScript without JavaScript: #ajax, #states](#7--javascript-without-javascript-ajax-states)  
  * [7.1- Brief Introduction to AJAX in Drupal](#71--brief-introduction-to-ajax-in-drupal)  
  * [7.2- Rendering elements with #states propery](#72--rendering-elements-with-states-property)  
[8- Problems and Solutions](#8--problems-and-solutions)  
  * [8.1- Slow execution due to wrong use of context](#81--slow-execution-due-to-wrong-use-of-context)  
  * [8.2- Loading JavaScript out of context](#82--loading-javascript-out-of-context)  
[9- Links and reading resources](#9--links-and-reading-resources)  
  * [9.1- JavaScript fundamentals](#91--javascript-fundamentals)  
  * [9.2- Functions in JavaScript and the IIFE format](#92--functions-in-javascript-and-the-iife-format)  
  * [9.3- JavaScript and Drupal](#93--javascript-and-drupal)  
  * [9.4- jQuery](#94--jquery)  
  * [9.5- Snippets](#95--snippets)  
[10- :wq!](#wq)  

**Index of Exercises**  
<!-- TOC -->  
[1-Introduction](#exercise-1-creating-a-basic-custom-module-for-testing)  
* [Acknowledgements and Reading Materials](#acknowledgements--reading-materials)
* [The Heuristic Approach](#the-heuristic-approach)
* [Scenario](#scenario)  


<!-- /TOC -->

-------------------------------------------------------------------------------

## 1- Introduction  

Some time ago (around December 2019, but it seems a century has passed ) I started writing what I thought would be a simple guide to integration between JavaScript and Drupal. A couple of months later, in February 2020, I had a tutorial of more than eleven thousand words written in Castillian (Spanish from Spain) that I published in my Medium profile.  

What was initially going to be brief has become a kind of reference guide on JavaScript and Drupal and (as far as I know) is now part of the training resources shared in many companies in Spain and other Latin American countries. Here you can reach the original publication in Medium, the so called: [JavaScript & Drupal 101 TUTORIAL HANDBOOK TOTAL MAX POWER 2000](https://medium.com/@davidjguru/javascript-drupal-101-tutorial-handbook-total-max-power-2000-a137326fea6a) (I can swear I had a lot of fun thinking about the title).  

Well, the fact is that since the publication, I received three basic types of feedback:  

  * "Hey, this is wrong, you have to check it"
  * "We have people in the company from other countries, do you have it translated into English?"
  * "Thank you for not putting it behind the Medium payment wall"

So although my first intention was to move all this content to an open book format like git-book or something like that, I've actually grouped the first two together and I'm going to publish a review of the original post translated into English. As always, I hope it can be useful to someone. 

In a complementary way, you can download all the code from the exercises grouped as a single Drupal custom module, available here: [gitlab.com/davidjguru/javascript_custom_module](https://gitlab.com/davidjguru/javascript_custom_module). This works in Drupal 8 and Drupal 9.  

**There we go!**  

## 2- JavaScript and Drupal: basic concepts  

If this is your first approach to the intersection between Drupal and JavaScript and it may even be your first approach to Drupal and its world, it's convenient that you review this section beforehand, in which we are going to share some terms and names that we will use throughout the tutorial.  

By this way you will know what we are talking about at any time in the manual and you will be able to follow the cases, examples and exercises more easily.  

* **Drupal**: Our technological platform of reference in this context. Something halfway between the framework and the CMS, free software downloadable and installable from here: [https://www.drupal.org](https://www.drupal.org). In this tutorial we'll travel over the shoulder of a Drupal, so it is good to know it.  
  
* **Render Array**: It's a key piece of Drupal to "paint" on screen. They are multidimensional arrays that must meet certain rules using different properties to model the elements to be rendered. The elements we usually draw are described here: [drupal.org/api/drupal/elements/9.2.x](https://api.drupal.org/api/drupal/elements/9.2.x). Most of the connections between Drupal and JavaScript will be done from Drupal's render arrays, so is highly recommended to know them and learn its declarative format.  
  
* **JavaScript**: A programming language very diversified so much as to be the basis of many frameworks, libraries and tools in fashion. Today it's executable both in client and server. In this context we will use the so called "Vanilla JavaScript", that is, the own handcrafted code outside JS platforms. See a guide from Mozilla: [mozilla.org/JavaScript/Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Introduction). In this tutorial, although it is not an advanced JavaScript manual, we will use this language in several sections, so is great that you know it a little bit.  

* **Immediately-invoked Function Expressions(IIFE)**: Also called "self-executing" function, it's a specific format to declare JavaScript functions so they are executed as they are declared, as soon as they are defined. See: [flaviocopes.com/javascript-iife](https://flaviocopes.com/javascript-iife/) to understand better this important concept. In this article we tried to integrate JavaScript into Drupal through this format, so it would be optimal if you at least understand the concept.  
  
* **AJAX**: This stands for Asynchronous JavaScript + XML, a combination of technologies for use partial requests (lighter than complete requests) from the client to the server, which results in speed and performance improvements. See more: [developer.mozilla.org/Guide/AJAX](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX). Although it is a complex and extensive topic, we will focused in the possibilities of implementing AJAX in Drupal.  
  
* **DOM**: The Document Object Model is the tree structure that represents all the HTML code used in the representation of the web we are visiting. See: [developer.mozilla.org/Glossary/DOM](https://developer.mozilla.org/en-US/docs/Glossary/DOM). In this guide we are going to make modifications and operations on HTML elements, so we will learn how to make changes on the DOM from Drupal.  
  
* **jQuery**: It's a mythical library based on JavaScript to facilitate (theoretically) manipulations of the DOM. In Drupal it (still, by now) maintains a very extensive presence, so we better get along with it. See: [developer.mozilla.org/Glossary/jQuery](https://developer.mozilla.org/en-US/docs/Glossary/jQuery). We're going to execute jQuery code in the Drupal context.  

## 3- How to include JavaScript code in Drupal

We will practice with the inclusion of JavaScript code in our project. To do this, we will create a new custom module and iterate on it providing you with JavaScript based functionality while we discuss the most important concepts in the following sections. In order to doing this, I recommend quickly creating a containerised test environment, using DDEV to deploy a Drupal installation on the fly. If you don't know DDEV, you can follow my own guide published in Digital Ocean: [How To Develop a Drupal 9 Website on Your Local Machine Using Docker and DDEV](https://www.digitalocean.com/community/tutorials/how-to-develop-a-drupal-9-website-on-your-local-machine-using-docker-and-ddev).  

You can also deploy a lightweight version of a Drupal installation just using your PHP local config, with a light server. Follow the steps in the next snippet:  

* [Drupal 8 || 9 : Ultra-lightweight deploy of Drupal setup (without Apache or MySQL)](https://gitlab.com/-/snippets/2021961)


### 3.1- Setting up the scenario: creating a custom module

To begin with, let's define the new custom module we will work with. I don't know what context you have with respect to Drupal, so I'll write down here a sequence of links that you can update with. You will need a Drupal deploy, maybe XAMP+ environment with web server, database and a Drupal deployed and ready to use, or if you're using DDEV (as I recommended in the previous section).  

Explaining how to create a custom module for Drupal is beyond the scope of this guide, but here are some links to read:  

* [Drupal.org guide: Creating Custom Module](https://www.drupal.org/docs/creating-custom-modules)  

**Snippets**
* [Drupal 9 in six steps using DDEV: Quick Deploy](https://gitlab.com/-/snippets/2012512)  
* [Drupal 8 || 9: Deploying a new Drupal Site with Composer / Drush on the fly](https://gitlab.com/-/snippets/1897782)  
* [Drupal 8 || 9: Creating modules and forms using Drupal Console](https://gitlab.com/-/snippets/1898128)  

### Exercise 1: Creating a basic custom module for testing

In case you already have a Drupal site available for testing (including use of Drupal Console), just type this from the console while being inside your project and Drupal Console will take care of creating the new module:

```txt
// Using Drupal Console with params.
drupal generate:module \
--module="Custom Module for JavaScript" \
--machine-name="javascript_custom_module" \
--module-path="modules/custom" \
--description="This is a custom generated module for JavaScript." \
--package="Custom" \
--module-file \
--no-interaction
```

If Drupal Console is not your option, you can use Drush, launching the command: 

```txt
$ drush generate
$ ddev drush generate
```
And you'll get a list of options, including:  

```txt
 module:                                                                                    
   module-configuration-entity                       Generates configuration entity module  
   module-content-entity (content-entity)            Generates content entity module        
   module-file                                       Generates a module file                
   module-standard (module)                          Generates standard Drupal 8 module     
```
And ask for a custom module creation with params, avoiding all parameters setting through dialogue:  

```txt
$ drush gen module-standard --directory modules/custom --answers '{"name": "Custom Module for JavaScript", "machine_name": "javascript_custom_module", "description": "Custom Generated Module for JavaScript.", "package": "Custom", "dependencies": "", "install_file": "no", "libraries": "no", "permissions": "no", "event_subscriber": "no", "block_plugin": "no", "controller": "no", "settings_form": "no"}'
```

See an example here: [Drupal 8 || 9 : Creating custom module using Drush generate](https://gitlab.com/-/snippets/2054593).

You can also download this basic custom module created for examples from my gitlab repository: [gitlab.com/davidjguru/basic_custom_module](https://gitlab.com/davidjguru/drupal-custom-modules-examples/tree/master/basic_custom_module), or doing git clone from the whole repository for custom modules: [gitlab.com/davidjguru/drupal-custom-modules-examples](https://gitlab.com/davidjguru/drupal-custom-modules-examples).  

This module is quite simple and basic, only for first setps in Drupal: when enabled, only creates a new path /basic/custom with a Controller that gives you a response creating a render array in Drupal, with a very simple markup message for HTML. With this, we can start to test.  

![Basic Custom First Route in Drupal](../../images/post/davidjguru_drupal_javascript_guide_1.png)  

We will now generate some content automatically for our exercises / test scenario. We can rename the custom module if we want, to particularize it a bit more (I'll use the naming javascript_custom_module to avoid confusion with other test modules. We will install, activate and generate a random comments set within our platform.  
To do this we'll use the [Drupal Devel Module](https://www.drupal.org/project/devel) and its [Devel Generate sub-module](https://www.drupal.org/docs/8/modules/devel/installation-whats-in-the-box) to create test content, adding [new commands and sub-commands to Drush](https://www.drupal.org/docs/8/modules/devel/new-drush-commands). We'll use Composer and Drush from inside the console project folder, just by typing:

```txt
$ composer require drupal/devel
$ drush en devel devel_generate
$ drush genc 10 5 --types=article
```
With these instructions above we asked to devel-generate to create ten items, using the type nodes (default in Drupal) with a comments set in each node, between 0 and 5 per node. We now have ten initial nodes to build our initial exercise scenario:  

![Creating a new set of nodes with type article](../../images/post/davidjguru_drupal_javascript_guide_2.png)  

Next, we will reorder what this example Controller originally returned. Until now it was simply a text message, but now we are going to add a table with comments associated with the current user. To do this we are going to perform a database query using the database service, extract the returned values and process them by launching them into the table rendering system. For the query filtered by the current user data through the current_user service .  

Let's see, now the controller class would look like this:  

{{< gist davidjguru 7896402 cd2e0bacbe4fb39ca511610ff58d0930 "CommentsListcontroller.php" >}}

What once enabled the test module (using Drush or Drupal Console -if it works in your Drupal installation-):

```txt
$ drush en -y javascript_custom_module
$ drupal moi javascript_custom_module 
```
This will generate the /javascript/custom path through the Controller and it will render on screen the following table:  

![Showing list of Comments in a table](../../images/post/davidjguru_drupal_javascript_guide_3.png)  

With this step, we have already prepared the initial scenario and can move on to perform exercises directly with JavaScript.  

**Next!**  


### 3.2- The "library" concept

Working with both CSS and JS from Drupal 8 onwards has become standardised. In previous versions of Drupal you had to use specific functions to add CSS or JS resources.
As I explained in this snippet: [Drupal 8 || 9 : Altering HTML in headers from hooks](https://gitlab.com/-/snippets/1927862), you had to use things like [drupal_add_html_head()](https://api.drupal.org/api/drupal/includes%21common.inc/function/drupal_add_html_head/7.x) to add new HTML tags, [drupal_add_js()](https://api.drupal.org/api/drupal/includes%21common.inc/function/drupal_add_js/7.x) to incorporate JavaScript or the [drupal_add_css()](https://api.drupal.org/api/drupal/includes%21common.inc/function/drupal_add_css/7.x) function to add more style sheets.  


#### 3.2.1- Secuence for creating libraries  

From Drupal 8, the sequence of inserting libraries has been standardised, and consists of fulfilling these three steps:  

* Create the CSS/JS files.  
* Define a library that includes these files.  
* Add this library to a typical Drupal render array.  

But in this case, we are going to reverse steps 1 and 2: first we will see how to create the library and then we will talk about the JavaScript file itself, which could be a little more complex.  

### Exercise 2: Defining our new custom library

Let's see...in our custom module, we'll include un nuevo fichero module_name.libraries.yml in order to describe the new dependencies, so in our case study, we'll create a new file called javascript_custom_module.libraries.yml filled with the next lines:  

```txt
// Case 1: Basic library file with only JavaScript dependencies.
module_name.library_name:
  js:
    js/hello_world.js: {}

// Example
custom_hello_world:
  js:
    js/hello_world.js: {}

```

All the libraries will be declared, as a rule of style, in the same .libraries.yml file, where we will describe all the libraries we need in our project, grouped by function or use.  

Here you can see several examples of definition of libraries for Drupal with some example models:  

{{< gist davidjguru 7896402 23b85a0dfea3ebf311245110a42316aa "basic_custom_module.libraries.yml" >}}

As we can see in the examples listed in the previous gist, there are different ways to declare libraries and even to add them externally. About the declaration of libraries, we can add a couple of curiosities that are nice to know:  


#### 3.2.2- Loading libraries in head 

By default, all libraries will tend to be loaded into the footer: In order to avoid operations over elements in DOM (Document Object Model) that have not yet been loaded, JS files will be included at the end of the DOM. If for some reason you need to load it at the beginning, then you can declare it explicitly using the pair parameter/value "header: true":

```txt 
js_library_for_header:
  header: true
  js:
    header.js: {}
    
js_library_for_footer:
  js:
    footer.js: {}
```

#### 3.2.3- Libraries as external resources  

#### 3.2.4- Libraries and dependencies 

### 3.3- The JavaScript file  

### 3.4- Adding JavaScript libraries 

#### 3.4.1- Using the #attached property in Render Arrays 

#### 3.4.2- Libraries in a TWIG template

#### 3.4.3- Global libraries for a Theme 

#### 3.4.4- Adding libraries from Hooks

## 4- Just a little bit more of JavaScript in Drupal  

### 4.1- Structure and guidelines for IIFE

### 4.2- Passing parameters in IIFE

### 4.3- Passing values from PHP to JavaScript: drupalSettings  

### 4.4- Changes in rendered HTML 

#### 4.4.1- Counting visits using Web Storage

## 5- Drupal and the old jQuery 

### 5.1- Fast review of the jQuery keys

### 5.2- Using jQuery in our Drupal installation  

### 5.3- Using a different version of jQuery  

## 6- Drupal Behaviors  

### 6.1- Anatomy of a Behavior  

### 6.2- The global object "Drupal" 

### 6.3- Behaviors in Drupal 

## 7- JavaScript without JavaScript: #ajax, #states  

### 7.1- (Brief) Introduction to AJAX in Drupal  

### 7.2- Rendering elements with #states property 

## 8- Problems and solutions 

### 8.1- Slow execution due to wrong use of 'context' 


### 8.2- Loading JavaScript out of context 


## 9- Links and Reading resources 

### 9.1- JavaScript fundamentals 

* [http://ryanmorr.com/understanding-scope-and-context-in-javascript/](http://ryanmorr.com/understanding-scope-and-context-in-javascript/)  
  
### 9.2- Functions in JavaScript and the IIFE format 

* [https://developer.mozilla.org/en-US/docs/Glossary/IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)  
* [https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript#Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/A_re-introduction_to_JavaScript#Functions)  
* [https://medium.com/@vvkchandra/essential-javascript-mastering-immediately-invoked-function-expressions-67791338ddc6](https://medium.com/@vvkchandra/essential-javascript-mastering-immediately-invoked-function-expressions-67791338ddc6)  

### 9.3- JavaScript and Drupal  

* [Unofficial JavaScript API for Drupal, by Théodore Biadala, @nod_](http://read.theodoreb.net/drupal-jsapi/)  
* [https://stackoverflow.com/questions/34025396/how-to-open-a-modal-in-drupal-8-without-using-a-link](https://stackoverflow.com/questions/34025396/how-to-open-a-modal-in-drupal-8-without-using-a-link)  
* [An example about Drupal.dialog](https://gist.github.com/devudit/dcdb76502975a13dd7c623cecc04f509)  
* [https://stackoverflow.com/questions/3941426/drupal-behaviors](https://stackoverflow.com/questions/3941426/drupal-behaviors)  
* [https://sqndr.github.io/d8-theming-guide/javascript/behaviors.html](https://sqndr.github.io/d8-theming-guide/javascript/behaviors.html)  
* [http://www.jaypan.com/tutorial/high-performance-javascript-using-drupal-7s-javascript-api](http://www.jaypan.com/tutorial/high-performance-javascript-using-drupal-7s-javascript-api)  
* [Drupal.org guide: Creating Custom Module](https://www.drupal.org/docs/creating-custom-modules)  
* [How To Develop a Drupal 9 Website on Your Local Machine Using Docker and DDEV](https://www.digitalocean.com/community/tutorials/how-to-develop-a-drupal-9-website-on-your-local-machine-using-docker-and-ddev)

### 9.4- jQuery 

  * [https://github.com/robloach/jquery-once](https://github.com/robloach/jquery-once)  
  * [https://github.com/RobLoach/jquery-once/blob/master/API.md#readme](https://github.com/RobLoach/jquery-once/blob/master/API.md#readme)  

### 9.5- Snippets

* [Drupal 8 || 9: Creating custom module using Drush generate](https://gitlab.com/-/snippets/2054593)  
* [Drupal 8 || 9: Altering HTML in headers from hooks](https://gitlab.com/-/snippets/1927862)  
* [Drupal 8 || 9: Ultra-lightweight deploy of Drupal setup (without Apache or MySQL)](https://gitlab.com/-/snippets/2021961)
* [Drupal 8 || 9: Altering HTML in headers from hooks](https://gitlab.com/-/snippets/1927862)
* [Drupal 8 || 9: Deploying a new Drupal Site with Composer / Drush on the fly](https://gitlab.com/-/snippets/1897782)  
* [Drupal 8 || 9: Creating modules and forms using Drupal Console](https://gitlab.com/-/snippets/1898128)
* [Drupal 9 in six steps using DDEV: Quick Deploy](https://gitlab.com/-/snippets/2012512)    


## 10- :wq! 

##### Recommended song

{{< youtube e5Z56ZXvAy4 >}}
