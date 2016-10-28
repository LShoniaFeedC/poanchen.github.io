---
layout: post
title: "How to login to a WordPress site using CasperJS?"
author: Poan (Baron) Chen
author_url: https://github.com/poanchen
date: 2016-10-28
---
Today, I am going to show you guys how to login to a site using CasperJS. CasperJS, is a navigation scripting and testing utility for [PhantomJS](http://phantomjs.org/), written in JavaScript. It allows us to test websites a lot like [PHPUnit](https://github.com/sebastianbergmann/phpunit/). Imagine that using CasperJS to automate the testing on your site. How it can help you to automate the repetitive tasks. How it can save your time and help you focus on implmenting your business logics. In this tutorial, we are going to impersonate a typical user who wants to login to a WordPress site, with correct username and password, they will be granted to signin. To prove that we are logged in to the site, we will be taking a screenshot when we passed the signin part. If you haven't already, install [CasperJS](http://casperjs.org/) or [PhantomJS](http://phantomjs.org/) before we continue on. Now, let's dive right in.

## CasperJS code to start with

<pre>
  <code class="javascript">
    var page = require('webpage').create();
    var casper = require('casper').create();

    var urlBeforeLoggedIn = "https://www.jenrenalcare.com/wp-login.php";
    var urlAfterLoggedIn = "https://www.jenrenalcare.com/wp-admin/";
  </code>
</pre>
The above code is a simple set up to get the CasperJS working. Those two variables namely urlBeforeLoggedIn and urlAfterLoggedIn. We will be using those url to sign in and for after sign in. Then, we need to get the CasperJS to open a site. It can be as simple as something like this.

## CasperJS code to open a site

<pre>
  <code class="javascript">
    casper.start(urlBeforeLoggedIn);
  </code>
</pre>
This simply means that CasperJS start by going to this specific site. Then, now we need to login to the site. With CasperJS, we can simply achieve it by doing,

## CasperJS code to submit a POST form

<pre>
  <code class="javascript">
    casper.waitForSelector('form[method="post"]', function() {
      casper.fillSelectors('form[method="post"]', {
        'input[name="log"]': 'put_your_username_or_email_address_here',
        'input[name="pwd"]': 'put_your_password_here'
      }, true);
    });
  </code>
</pre>
By the way, I used the web browser page inspector to get the name of the username field and password field which is log and pwd. At the end, I also set true, so that the form will be submitted automatically. Be sured to put your real username and password there. Then, all we need to do is wait for the site to transfer to the logged in area.

## CasperJS code to wait for the url to redirect to logged in area

<pre>
  <code class="javascript">
    casper.waitForUrl(urlAfterLoggedIn, function() {
      this.echo("Now, with correct username and password, you should be signed in to the site when you see this.");
    });
  </code>
</pre>
When you run this CasperJS script, and see the message printed. That means that you have successfully signed in with CasperJS. To prove that we are signed in, I will be taking screenshot using the built-in function in CasperJS.

## CasperJS code to screenshot where we are in CasperJS script

<pre>
  <code class="javascript">
    casper.waitForUrl(urlAfterLoggedIn, function() {
      this.viewport(3000, 1080);
      this.capture('./screenshot.png', {top: 0,left: 0,width: 3000, height: 1080});
    });
  </code>
</pre>
When this script is completed, you should see an image saved in where you put your CasperJS script.

## Complete code for CasperJS script that signin to a WordPress site, then take a screenshot when it is in
<pre>
  <code class="javascript">
    var page = require('webpage').create();
    var casper = require('casper').create();

    var urlBeforeLoggedIn = "https://www.jenrenalcare.com/wp-login.php";
    var urlAfterLoggedIn = "https://www.jenrenalcare.com/wp-admin/";

    casper.start(urlBeforeLoggedIn);

    casper.waitForSelector('form[method="post"]', function() {
      casper.fillSelectors('form[method="post"]', {
        'input[name="log"]': 'put_your_username_or_email_address_here',
        'input[name="pwd"]': 'put_your_password_here'
      }, true);
    });

    casper.waitForUrl(urlAfterLoggedIn, function() {
      this.viewport(3000, 1080);
      this.capture('./screenshot.png', {top: 0,left: 0,width: 3000, height: 1080});
    });
  </code>
</pre>

## Wrapping Up

Hopefully this guide has given you the confidence to use CasperJS to simulate user activity on a web page. For example, login. I hope that this tutorial has helped you and thank you for reading!

## Resources

I'll try to keep this list current and up to date. If you know of a great resource you'd like to share or notice a broken link, please [get in touch](https://github.com/poanchen).

### Getting started

* [CasperJS documentation](http://docs.casperjs.org/en/latest/) by [CasperJS](http://casperjs.org/).
* [Image downloader using CasperJS](https://github.com/poanchen/image-downloader) by [poanchen](https://github.com/poanchen).