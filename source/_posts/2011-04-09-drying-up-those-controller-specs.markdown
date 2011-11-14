---
layout: post
title: "DRYing up those controller specs"
date: 2011-04-09 13:39
comments: true
categories: 
- ruby
- rails
- testing
- code
---

I recently had a bunch of duplicate code in my controller specs and wanted to share a solution I arrived at that DRYed things up significantly.
Feel free to use it and abuse it and if you have recommendations on how to improve it, let me know. 

First, the quick requirements and background: *all* controller methods must explicitly demonstrate that guests do not have access to certain resources and are told, upon any attempt to access them, that they must login in order gain access. 

The first pass was to write the tests, get all to pass, then look to consolidate and improve the suites.
Here's the first pass: 

{% gist 1134284 %}

These specs duplicate an awfully large amount of code.
This is an issue because more lines of code translate into more maintenance, more potential areas to make mistakes, and more places that need to be updated when things change.
(For example, what if we want to flash an alert of *"OH NOZ, SIGN IN FIRST PLZ"*?
I could find and replace, but that should be handled in one place.) 

In order to remedy these issues, I added a controller macro that includes *#it_should_block_access_to(action, options = {})*, which is passed an action (:index, :create, :update), maps each to a default method (:get, :post, :put) if none is specified, and generates the appropriate routing (member vs collections).
It also handles format specification, such as :json, or setting any other arbitrary params via the options hash. 

Here's the refactored code: 

{% gist 1134094 %}

The controller specs are cleaner, a big chunk of code duplication has been removed, and there is a main handler for these tests that does not (in my opinion) add too much complexity into the testing suite.
