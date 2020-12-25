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

**Table of Contents**  
<!-- TOC -->  
[1-Introduction](#introduction)  
* [Acknowledgements and Reading Materials](#acknowledgements--reading-materials)
* [The Heuristic Approach](#the-heuristic-approach)
* [Scenario](#scenario)  
  
[Step 1 - Select your Tooling](#step-1-select-your-tooling)  
* [Prompt](#prompt)  
* [Browser Extensions](#browser-extensions)  
* [Drupal Modules](#drupal-modules)  

[Step 2 - Take an updated photo](#step-2-take-an-update-photo)  
* [Make your own plan](#make-your-own-plan)  
* [Get a snapshot](#get-a-snapshot)  
* [Goals and Benchmarking ](#goals-and-benchmarking)  

[Step 3 - Check out PHP](#step-3-check-out-php)  
* [Understanging PHP](#understanding-php)
* [OPCache](#opcache)  
* [Some More Optimizations](#some-more-optimizations)  
  
[Step 4 - Review your Nginx webserver](#step-4-review-your-nginx-webserver)  
* [FPM: Processes, Workers and Threads](#fpm-processes-workers-and-threads)  
* [Enabling some key params](#enabling-some-key-params)  
* [Compression and functions](#compression-and-functions)  
  
[Step 5 - Have a little checklist for your stack](#step-5---have-a-little-checklist-for-your-stack)
* [Review your Drupal installation](#review-your-drupal-installation)  
* [Tuning MySQL (MariaDB)](#tuning-mysql-mariadb)  
* [How Docker Works](#how-docker-works)  

[Step 6 - Read more and more, and more](#step-6---read-more-and-more-and-more)  
* [About Drupal Performance](#about-drupal-performance)  
* [About Nginx Performance](#about-nginx-performance)  
* [About PHP Performance](#about-php-performance)  
* [About MySQL Performance](#about-mysql-performance)  
* [About Docker Performance](#about-docker-performance)  

[:wq!](#wq)  
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
  
* **
