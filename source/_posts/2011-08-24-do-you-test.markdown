---
layout: post
title: "Do you test?"
date: 2011-08-24 15:23
comments: true
categories: 
- ruby
- rails
- testing
- code
---

Note: this was in response to a question asked on the [Dallas.rb](http://dallasrb.org/ "Dallas.rb homepage") group regarding if, how, and why you write tests (or specs, or whatever you want to call them).

Definitely test.
It will feel weird / slow at first, but it helps you write much cleaner, more modular code.
I initially hated it when I was forcing myself to adopt it, now I feel naked without it.
For me, I find testing important for a few reasons:

* It helps me think through what I want the app to do should do from a stakeholders standpoint.

* It helps me and anyone else working on my app from a dev standpoint know how my app and its components should behave - specifications should do just that:
  specify what certain things should do! I try to always ask myself "What should you do, Mr. Object" and then write a spec around that.

  For example, take a look at this:

  {% gist 1168937 %}

  You can see what I'd expect if you were to write *User#full_name*. It says: *"Show me the full name when available, or its parts, or default to the email address, which is ensured to be present, if all else fails."*
  Perhaps we drew up this specification when determining how we should handle displaying a "Hi, #{current_user.full_name}" in our UI for the different cases our app might encounter.
  Or perhaps #full_name was the failing part of our acceptance test when we were writing the code that we wish he had.
  (See [The RSpec Book](http://pragprog.com/book/achbd/the-rspec-book) for this reference and a very informative read.)

* It's an insurance policy and time saver.
  Anytime you change your app you can be confident that other areas still behave as expected.
  My workflow before TDD / BDD was:

    1. implement some new feature
    2. think through all other parts that could be effected
    3. pop open my dev env and manually test each to make sure things were working.

  This was a waste of time and exposed me to my own human error: perhaps I forgot to do a certain thing or whatever.
  The computer is your friend (and worker), make it do the checking for you.
  My workflow is now:

    1. implement some new feature
    2. initiate tests
    3. go make a sandwich, return and see what passed what needs fixing.

    I found writing expressive and clean unit tests much easier than acceptance tests.
    If you are using Cucumber, check this out: [Imperative vs Declarative scenarios in user stories](http://benmabey.com/2008/05/19/imperative-vs-declarative-scenarios-in-user-stories.html) and try to avoid using the pre-packaged web-steps when possible.
