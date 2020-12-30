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
[Exercise 1: Creating a basic custom module](#exercise-1-creating-a-basic-custom-module-for-testing)  
[Exercise 2: Defining our new custom library](#exercise-2-defining-our-new-custom-library)  
[Exercise 3: Defining our initial JavaScript file](#exercise-3-defining-our-initial-javascript-file)  
[Exercise 4: Adding libraries to our Drupal custom module](#exercise-4-adding-libraries-to-our-drupal-custom-module)  



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

**DISCLAIMER:** This guide is actually a manual for the integration of JavaScript code in Drupal-based projects, but only in the context of implementing Drupal modules. This is basically a backend issue. This guide does not contain information related to JavaScript frameworks (React, Angular, Vue) or about the use of Drupal headless as decoupled. Neither does it deal with Drupal Theming issues and its approach to them is only tangential. This tutorial is only for people related to the Drupal backend.  


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

```
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

```
$ drush generate
$ ddev drush generate
```
And you'll get a list of options, including:  

```
 module:                                                                                    
   module-configuration-entity                       Generates configuration entity module  
   module-content-entity (content-entity)            Generates content entity module        
   module-file                                       Generates a module file                
   module-standard (module)                          Generates standard Drupal 8 module     
```
And ask for a custom module creation with params, avoiding all parameters setting through dialogue:  

```
$ drush gen module-standard --directory modules/custom --answers '{"name": "Custom Module for JavaScript", "machine_name": "javascript_custom_module", "description": "Custom Generated Module for JavaScript.", "package": "Custom", "dependencies": "", "install_file": "no", "libraries": "no", "permissions": "no", "event_subscriber": "no", "block_plugin": "no", "controller": "no", "settings_form": "no"}'
```

See an example here: [Drupal 8 || 9 : Creating custom module using Drush generate](https://gitlab.com/-/snippets/2054593).

You can also download this basic custom module created for examples from my gitlab repository: [gitlab.com/davidjguru/basic_custom_module](https://gitlab.com/davidjguru/drupal-custom-modules-examples/tree/master/basic_custom_module), or doing git clone from the whole repository for custom modules: [gitlab.com/davidjguru/drupal-custom-modules-examples](https://gitlab.com/davidjguru/drupal-custom-modules-examples).  

This module is quite simple and basic, only for first setps in Drupal: when enabled, only creates a new path /basic/custom with a Controller that gives you a response creating a render array in Drupal, with a very simple markup message for HTML. With this, we can start to test.  

![Basic Custom First Route in Drupal](../../images/post/davidjguru_drupal_javascript_guide_1.png)  

We will now generate some content automatically for our exercises / test scenario. We can rename the custom module if we want, to particularize it a bit more (I'll use the naming javascript_custom_module to avoid confusion with other test modules. We will install, activate and generate a random comments set within our platform.  
To do this we'll use the [Drupal Devel Module](https://www.drupal.org/project/devel) and its [Devel Generate sub-module](https://www.drupal.org/docs/8/modules/devel/installation-whats-in-the-box) to create test content, adding [new commands and sub-commands to Drush](https://www.drupal.org/docs/8/modules/devel/new-drush-commands). We'll use Composer and Drush from inside the console project folder, just by typing:

```
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

```
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

Let's see...in our custom module, we'll include a new file module_name.libraries.yml in order to describe the new dependencies, so in our case study, we'll create a new file called javascript_custom_module.libraries.yml filled with the next lines:  

```
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

``` 
js_library_for_header:
  header: true
  js:
    header.js: {}
    
js_library_for_footer:
  js:
    footer.js: {}
```

#### 3.2.3- Libraries as external resources  

We are looking at examples of creating our own custom libraries, but it's also possible to declare in the .libraries.yml file of our custom module the use of an external library that is available via CDN or by an external repository.  

It is possible to request to Drupal the use of an external library to incorporate it to our project, as we can see in the example of the use of backbone.js in the Drupal core, created by third parties, incorporated to Drupal and declared coherently with their external data:  

![Libraries as external resources](../../images/post/davidjguru_drupal_javascript_guide_4.png)  

By the way, in the same file core.libraries.yml you'll can see all the JavaScript resources declared from the core of Drupal. Some of these resources will be used here in this guide.  ;-)   

In this former example about backbone.js in Core, we're seeing that finally, the library is used from a local environtment, right? so...It is possible loading a library directly from remote? we'll see the official documentation from Drupal saying something like this:  

> *“You might want to use JavaScript that is externally on a CDN (Content Delivery Network) to improve page loading speed. This can be done by declaring the library to be “external”. It is also a good idea to include some information about the external library in the definition.”.*  

* [Source: Drupal.org/docs Adding js to a Drupal Module](https://www.drupal.org/docs/creating-custom-modules/adding-stylesheets-css-and-javascript-js-to-a-drupal-module#external)  
  
So we can do something like this: 

```
angular.angularjs:
  remote: https://github.com/angular/angular.js
  version: 1.4.4
  license:
    name: MIT
    url: https://github.com/angular/angular.js/blob/master/LICENSE
    gpl-compatible: true
  js:
    https://ajax.googleapis.com/ajax/libs/angularjs/1.4.4/angular.min.js: { type: external, minified: true }
```

Quite interesting, right?  



#### 3.2.4- Libraries and dependencies  

It is possible that within our JavaScript code, in your own .js file, we may need to use another third-party library for our functionality. Well, in that case, we can declare libraries with dependencies following a basic vendor/resource or vendor/library scheme.

Let's see an example in which we intend to use a hide/show effect. As such animations are available in the jQuery library and it's integrated in Drupal (we will see it later), then instead of creating those functions we'll declare the dependency and we will be able to use them:  


```
js_library_hide_show:
  js:
    js/my_custom_javascript_library.js: {}
  dependencies:
    - core/jquery
```

In addition, there is a set of options that you can use as attributes to customize the use of your new CSS / JavaScript libraries. See: [Drupal org Docs: Libraries options and details](https://www.drupal.org/docs/8/theming/adding-stylesheets-css-and-javascript-js-to-a-drupal-8-theme#libraries-options-details).  

### 3.3- The JavaScript file  

The next step will be to define that JavaScript file that we have declared as a resource within the new previous library.  

### Exercise 3: Defining our initial JavaScript file 

For that, we'll create a /js folder and will put inside our new file hello-world.js wich contains our new library with a little action, just say hello by Console:  

```
(function () {
  'use strict';

  // Put here your custom JavaScript code.
  console.log ("Hello World");
})();
```

So the internal structure of our custom module for testing should look like this:  

```
/javascript_custom_module
    /js
        javascript_file_name.js
    /src
        /Controller
            YourCustomExampleController.php
    javascript_custom_module.info.yml
    javascript_custom_module.routing.yml
    javascript_custom_module.libraries.yml
```


### 3.4- Adding JavaScript libraries 

Now our goal is linking the new library with its JavaScript .js file associated with the context in which it should work, right? Well, for that we are going to make a base case and then we are going to add more probable cases, given that in Drupal it is possible to attach JavaScript libraries in various ways, depending on how we need to use them in our code.  

But let's see first the base case for our case: #attached.

#### 3.4.1- Using the #attached property in Render Arrays 

On one hand, we have the eternal Drupal Render Arrays, that is, the arrays loaded with properties, values, parameters and others that we use to send to the Drupal rendering system so it transforms everything and ends up painting HTML renderable in a browser.  

On the other hand, we have a property called "#attached" that offers us a set of already defined sub-properties that allow us to attach resources of different nature to any render array we are using (a controller response, a form build, etc):  

* Library -> $render_array['#attached']['library']  
* drupalSettings (from PHP to JavaScript) -> $render_array['#attached']['drupalSettings']  
* Http_Header -> $render_array['#attached']['http_header']  
* HTML Link in Head -> $render_array['#attached']['html_head_link']  
* HTML Head -> $render_array['#attached']['html_head']  
* Feed -> $render_array['#attached']['feed']  
* Placeholders -> $render_array['#attached']['placeholders']  
* HTML Response Placeholders -> $render_array['#attached']['html_response_attachment_placeholders']  

We will come back to some of these cases in following sections, But for more info about the processing of attached resources, You can visit the official documentation in Drupal.org: [public function HtmlResponseAttachmentsProcessor](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21HtmlResponseAttachmentsProcessor.php/function/HtmlResponseAttachmentsProcessor%3A%3AprocessAttachments/8.7.x).  

See some examples at: 
* [davidjguru.github.io/the-magic-of-attached](https://davidjguru.github.io/blog/drupal-fast-tips-the-magic-of-attached).  
* [dev.to/davidjguru/erasing-traces-of-generator-in-drupal-projects](https://dev.to/davidjguru/erasing-traces-of-generator-in-drupal-projects-1cdh)  


### Exercise 4: Adding libraries to our Drupal custom module  

By now, we just need to go to the PHP class file (The Controller) and modify the render array that is returned at the end, including the #attached property with our new library:

```
// Path: javascript_custom_module/src/Controller/
// File: CommentsListController.php
// Function: gettingList()

// Before (line 42):
$final_array['welcome_message'] = [
  '#type' => 'item',
  '#markup' => $this->t('Hello World, I am just a text.'),
];

// Now (line 42): 
$final_array['welcome_message'] = [
  '#type' => 'item',
  '#markup' => $this->t('Hello World, I am just a text.'),
  '#attached' => [
    'library' => [
      'javascript_custom_module/js_hello_world_console',
    ],
  ],
];

// Form : 
$attachments['#attached']['library'][] = 'module/library';
```
Just after changed it, We will reinstall our custom module, clearing cache:

```
$ drush pmu javascript_custom_module
$ drush en -y javascript_custom_module
$ drush cr

// Drupal Console (include clearing cache)
$ drupal mou javascript_custom_module
$ drupal moi javascript_custom_module
```

 We can see now from the Console of your browser the result of the execution of our first JavaScript code, just going to the declared route:  

 ![Loading JavaScript file in the custom module](../../images/post/davidjguru_drupal_javascript_guide_5.png)  

**We've made our first interaction with JavaScript in Drupal!**  
Well, now we are going to continue adding new JS cases, and then we will come back to this same initial case to continue iterating and looking at more and more available functionality.  

Following this simple initial exercise, we can check the operation of basic JavaScript methods such as an alert window or a confirmation window through the integration of libraries using the #attached property:  

 ![Adding basic JavaScript functions to our custom code](../../images/post/davidjguru_drupal_javascript_guide_6.gif)  



#### 3.4.2- Libraries in a TWIG template

To add libraries to a Twig template within our project, either for a custom template within our own module or in a specific Twig template of the Theme we are using, we will load it through the Twig `attach_library()` function that allows us to add directly to the template:  

```
{% block salute %}
  {% if salute_list is not empty %}
    {{ attach_library('custom_module_name/library_name') }}
    <div class="salute__wrapper layout-container">
       {{ parent() }}
    </div>
   {% endif %}
{% endblock salute %}
```
But the truth is that it can cause problems in the rendering (that it does not arrive in time to load in the rendering cycle of the Render system that is put in motion when "painting" a page) if it is added to the global template html.html.twig . This is a debate that has been going on for some time: [https://www.drupal.org/node/2398331#comment-9745117](https://www.drupal.org/node/2398331#comment-9745117) and is also a subject for discussion with a view to changing the way libraries are loaded in the near future of Drupal: [https://www.drupal.org/project/drupal/issues/3050386](https://www.drupal.org/project/drupal/issues/3050386). So beware of the template you use it on that might not work and pay attention to changes that might come in new versions of Drupal.  


#### 3.4.3- Global libraries for a Theme 

To declare your library as a global dependency for your Theme or your custom module, just include it in the declarative file of the *.info.yml resource using the libraries property:  

```
# resource.info.yml

libraries:
  - module/library
```

In any case and as in the previous section, there are discussions about the evolution of this and some measures that are supposed to be taken for future versions: [https://www.drupal.org/node/1542344](https://www.drupal.org/node/1542344). The advice remains the same: **Pay attention to possible changes.**


#### 3.4.4- Adding libraries from Hooks

It is also possible to add new custom libraries in our Drupal context, specifically before the time of rendering existing pages, through pre-processing hooks, such as [hook_page_attachments()](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21theme.api.php/function/hook_page_attachments/9.0.x), which still maintains the already seen way of adding resources:  

```
// Form: 
$attachments['#attached']['library'][] = 'module/library';
```

Using a basic scheme for use:  

```
/**
 * Implements hook_page_attachments().
 */
 
 function custom_page_attachments(array &$attachments) {
   
    $attachments['#attached']['library'][] = 'module/library';

 }
```

Another option in hooks is the [hook_preprocess_HOOK()](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21theme.api.php/function/hook_preprocess_HOOK/9.0.x) function that according to its documentation, makes it easier for modules to preprocess theming variables for various elements. Let's see a couple of examples:  

```
/**
 * Implements hook_preprocess_HOOK() for menu.
 */
function theme_name_preprocess_menu(&$variables) {

  $variables[‘#attached’][‘library’][] = ‘theme/library’;
}
```

The execution of this previous hook will make Drupal go to `menu.html.twig` and perform the addition of the differentiated library. Furthermore, this resource can be used in a generic way (for example, for all pages):  

```
/**
 * Implements hook_preprocess_HOOK() for page.
 */
function custom_theming_preprocess_page(&$variables) {
  
  $variables['#attached']['library'][] = 'module/library';
}
```

In this case it is recommended to specify metadata to facilitate the caching of the new change, specifically if the aggregation operation of the new library depends on conditions, for example:  

```
/**
 * Implements hook_preprocess_HOOK() for page with conditions.
 */
function custom_theming_preprocess_page(&$variables) {

  $variables['page']['#cache']['contexts'][] = 'route';
  $route = "entity.node.preview";  
  
  if (\Drupal::routeMatch()->getRouteName() === $route) {
    $variables['#attached']['library'][] = 'module/library';
  }
}
```

And for more specific resources:  

```
/**
 * Implements hook_preprocess_HOOK() for maintenance_page.
 */
function seven_preprocess_maintenance_page(&$variables) {

  $variables[‘#attached’][‘library’][] = ‘theme/library’;
}
``` 


## 4- Just a little bit more of JavaScript in Drupal  

Let's take a closer look at the rules of use and integration of JavaScript code in a Drupal project.  

### 4.1- Structure and guidelines for IIFE

The first thing that should call our attention is the fact that the structure of the .js extension file that we have introduced in our project through the /js folder has the following structure:  

```
(function () {
  'use strict';

  // Put here your custom JavaScript code.
  console.log ("Hello World");
})();
```

In Drupal, all our JavaScript code will be integrated within a closure function, as a wrapper of the code based on the IIFE pattern, that is, the "Immediately Invoked Function Expression (IIFE)" model, used as a useful structure for three key issues:  

1. First, it allows immediate execution (or self-execution).  
2. Second, it limits the scope of internal variables: does not alter other JavaScript codes present in the project.  
3. Third, The context execution of the IIFE is created and ends up destroying it automatically: it frees up memory space, and releases it quickly.  

How is this achieved? Well I think we can understand the IIFE model in an intuitive way in four steps. Let's see:  

1. We can create a function in JavaScript as normal:  
```
function myFunction() {

  // Here your JavaScript code.
}
```
2. This function may or may not have a name (being an anonymous function) but in this case must be assigned to a variable:  

```
// Function with name:
function myFunction(){ 

  // Here your JavaScript code. 

} -> Right

// Anonymous function assigned to a variable:
var myFunction = function() { 
  
  // Here your JavaScript code. 

} -> Right

// Anonymous function being not assigned to a variable:  
function() { 
  
  // Here your JavaScript code. 
  
} -> JavaScript error
```

So JavaScript does not allow us to execute the function, because after the keyword "function" it waits for a name that it cannot find.  

3. This can be avoided by introducing the anonymous function in parentheses (well actually just by putting a sign in front of it would already serve but we adopt this consensus of the parentheses as a style guideline). This makes the JavaScript engine consider it an expression, or Function Expression (instead of Function Statement, with a name):  
   
```
(function() {
  
  // Here your JavaScript code.

})
```

4. The function remains in memory but nobody is using it. How do we execute it? Well, we can use the final parenthesis to call its execution:  


```
(function() { 

  // Here your JavaScript code. 

})()   -> It's only a guideline, since this algo serves:   

(function() { 
  
  // Here your JavaScript code. 

}())  -> We've passed the invocative parentheses into the expression.
```

In fact, if we enter parameters in the execution brackets, the function will treat them with absolute normality. We will see an example later on through a small exercise (Ex. 5: Passing values to the IIFE format).

Besides, as it is an anonymous function, it can be used as an "arrow function":  

```
(() => {   // Here your JavaScript code. //   })()
```

The latter are the forms that our JavaScript code can take in Drupal. Remember that whatever the style guideline we choose, we always need to comply with two fundamental guidelines:  

1. They are built in a compartmentalized way, without "contaminating" any global object, that is, the global execution space (that the variables only live inside their function, like a private code block).  
2. They are executed immediately, destroyed and cannot be executed again (if a page is reloaded, they are requested again).  


### 4.2- Passing parameters in IIFE

We are going to makechanges on the rendered HTML of our Drupal through our custom module, for which we must first assign a custom selector to the element we want to modify.  

### Exercise 5: Passing values to the IIFE format  

We start by going back to the controller class file and adding two new Drupal element rendering system properties: #prefix and #suffix which allow an HTML element to be framed within other HTML tags. In this case we want to add our own id to the element.

```
// Line 42.
$final_array['welcome_message'] = [
  '#type' => 'item',
  '#markup' => $this->t('Hello World, I am just a text.'),
  '#prefix' => '<div id="salute">',
  '#suffix' => '</div>',
  '#attached' => [
    'library' => [
      'javascript_custom_module/js_hello_world_console',
     ],
   ],
 ];

```

Next we create a new .js file ('iife_salute_example.js')with a function in IIFE format. To this function we will pass a text string as a greeting for our users ('Dear User'), and we will declare the input parameter in its definition ('parameter').  

```
(function (parameter) {
  'use strict';

  // Get the HTML element by it ID.
  let element = document.getElementById("salute");
  console.log(element);

  // Add to the HTML the new string using the parameter.
  element.innerHTML += "Salute, " + parameter;

  // Creating and adding a line for the HTML element.
  var hr = document.createElement("hr");
  console.log(hr);
  element.prepend(hr);

})('Dear User');
```

We'll introduce some changes with pure JavaScript, like adding a text to the message of the HTML element, taking the value of the text string passed by parameter. Then we also put a dividing line over the element, as a separator.  

We added the new file to the library resources that we had already defined previously:  

```
js_hello_world_console:
  js:
    js/hello_world_console.js: {}
    js/iife_salute_example.js: {}
```

And so, if we clean the drush cr cache and reload the /javascript/custom path in the browser, we will be able to see the new changes made using JavaScript:  

![Rendering custom changes in HTML using JavaScript](../../images/post/davidjguru_drupal_javascript_guide_7.png) 

### 4.3- Passing values from PHP to JavaScript: drupalSettings  

We have seen in the previous section how to pass values to that IIFE within the revision of the structure and operation of this JavaScript code format and now we are going to stop at a very particular construction that is available for us to make connections between our server executable code (PHP) and our client executable code (JavaScript) within Drupal: let's talk about drupalSettings.  

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
* [Drupal org Docs: Libraries, options and details](https://www.drupal.org/docs/8/theming/adding-stylesheets-css-and-javascript-js-to-a-drupal-8-theme#libraries-options-details)  
* [public function HtmlResponseAttachmentsProcessor](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Render%21HtmlResponseAttachmentsProcessor.php/function/HtmlResponseAttachmentsProcessor%3A%3AprocessAttachments/8.7.x).  
* [Drupal Fast Tips: The magic of 'attached'](https://davidjguru.github.io/blog/drupal-fast-tips-the-magic-of-attached)  
* [Erasing traces of generator in Drupal projects](https://dev.to/davidjguru/erasing-traces-of-generator-in-drupal-projects-1cdh)  


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
