---
title: "Five Steps to Improve the Performance of Drupal 9"
date: 2020-11-25
draft: true

# post thumb
image: "images/post/davidjguru_drupal_9_improve_performance_main.jpg"

# meta description
description: "Improve the performance of your Drupal Installation"

# taxonomies
categories: 
  - "Development"  
tags:
  - "Backend"
  - "Contrib Modules"
  - "Environments"
  - "PHP"

# post type
type: "post"
---


It is said that a small change or the sum of many of them can end up generating great transformations. This is valid as a legend to address improvements or to analyze the meaning of chaos, but now I just want to use it to frame the meaning of today's article. Well, I was speaking about perfomance with some of my colleagues and then I thought of some cases from diverse projects I had worked on and decided to put together some notes about perfomance always in a PHP / Drupal. I have gathered various experiences, conclusions and notes and tried to articulate them in an orderly way. You might be interested if you are concerned about the slowness of your Drupal installation. One part to find out how some issues work at the performance level, other parts to improve it, so yes...today I'll write about this topic. There we go!  

--------------------------------------------------------------------------------------
**Picture from Unsplash, user [Benjamin @Child](https://unsplash.com/@bchild311)**

  
---------------------------------------------------------------------------------

**Table of Contents**  
<!-- TOC -->  
[Introduction](#introduction)  
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
* []()  
* []()  
* []()  
  
[Step 5 - Have a little checklist for Drupal modules](#step-5-have-a-little-checklist-for-drupal-modules)
* []()  
* []()  
* []()  

[Step 6 - Read more and more, and more](#step-6---read-more-and-more-and-more)  
* [](#a)
* []()
* []()

[:wq!](#wq)  
<!-- /TOC -->

-------------------------------------------------------------------------------

## Introduction


## Step 1 - Select your Tooling

### Prompt 

#### Apache Benchmarking 

#### Parallel

#### GNU-Plot

#### Others

**TMUX** 

**Atop**

**Siege**

### Browser Extensions

#### Diver

#### PLT

#### RTM


### Drupal Modules

#### Performance Monitor


#### Monitoring
 

#### Remote Dashboard 


## Step 2 - Take an updated photo

### Make your own plan

1. Measure and record the current site performance.  
2. Define goals and requirements for the site.  
3. Actually perform your review.  
4. Define a list of potential improvements based on the site goals and requirements,
using the information gathered in the performance baseline and your review. 

### Get a snapshot 


### Goals and benchmarking



## Step 3 - Check out PHP 



### Understanding PHP 

### OPCache 

**Memory**

**Times**

**Files**

### Some more optimizations



## Step 4 - Review your Nginx webserver

### FPM: processes, workers and threads


### Enabling some key params

#### Gzip compression

#### Events Functions





## Step 5 - Have a little checklist for Drupal modules

### Review your custom code

### Disabling modules for UIs

### 

## Step 6 - Read more and more, and more

### About Drupal performance

https://www.kelltontech.com/kellton-tech-blog/how-optimize-drupal-website-performance
https://www.siteground.com/kb/how_to_optimize_drupal_for_better_performance/
https://www.monitis.com/blog/7-best-practices-for-improving-your-drupal-performance/
https://www.keycdn.com/blog/speed-up-drupal
https://www.valuebound.com/resources/blog/a-beginners-guide-to-performance-optimization-drupal-8
https://2bits.com/articles/drupal-performance-tuning-and-optimization-for-large-web-sites.html





### About Nginx Performance 

https://docs.bluehosting.cl/tutoriales/servidores/como-configurar-nginx-para-obtener-un-rendimiento-optimo.html
https://dzone.com/articles/maximizing-drupal-8-performance-with-nginx
https://dzone.com/articles/maximizing-drupal-8-performance-with-nginx-part-ii
https://www.nginx.com/blog/maximizing-drupal-8-performance-nginx-part-ii-caching-load-balancing/
https://www.ashnik.com/nginx-drupal-help-boost-site-performance/




### About PHP Performance

https://www.cloudways.com/blog/php-performance/
https://stackify.com/php-performance-optimization-guide/
https://stackify.com/php-performance-tuning/
https://codeburst.io/php-performance-optimization-992acaa78817
https://tideways.com/profiler/blog/5-ways-to-increase-php-performance

https://www.yourhowto.net/best-practices-to-optimize-code-performance-in-php/

### About MySQL Performance

https://codeburst.io/database-performance-optimization-8d8407808b5b
https://www.infoworld.com/article/3210905/10-essential-performance-tips-for-mysql.html
https://www.linode.com/docs/databases/mysql/how-to-optimize-mysql-performance-using-mysqltuner/
https://severalnines.com/database-blog/mysql-performance-cheat-sheet
https://www.percona.com/blog/2014/01/28/10-mysql-performance-tuning-settings-after-installation/


### About Docker Performance

https://rancher.com/5-tips-making-containers-faster/
https://containerjournal.com/features/make-docker-containers-faster-improve-performance/
https://hackernoon.com/another-reason-why-your-docker-containers-may-be-slow-d37207dec27f
https://jobandtalent.engineering/optimizing-docker-images-for-a-faster-development-workflow-591dc3ac4de0
https://crate.io/a/analyzing-docker-container-performance-native-tools/
https://medium.com/@beld_pro/how-does-docker-play-with-cgroups-when-i-create-an-instance-6ce24daccfb2

https://shekhargulati.com/2019/01/03/how-docker-uses-cgroups-to-set-resource-limits/


## :wq! 

##### Recommended song

{{< youtube e5Z56ZXvAy4 >}}
