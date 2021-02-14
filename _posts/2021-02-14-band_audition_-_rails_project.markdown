---
layout: post
title:      "Band Audition - Rails Project"
date:       2021-02-14 15:47:11 -0500
permalink:  band_audition_-_rails_project
---

I have just wrapped up my third project through Flatiron School, the Rails Project. Check it out [here.](https://github.com/nlewis84/Band-Audition)

## Band Audition
I decided to build an app for my wife to use. She is currently a middle school band director. Part of her responsibilities are to audition students for which instrument they are best suited for. This app would replace the Google Sheet she currently uses.

## Features
* User Accounts
* Users can create many Auditions
* Users can join auditions they did not create through the use of "Join" codes
* Auditions have many Instruments
* Can create new students and assign them to an Instrument
* Display page for seeing all students assigned to instruments so far

That last feature is important because it will allow my wife to display the "results" on a projector.

## Planning

If you are working on your own Rails project, I would say that there are two main things that you plan extensively before you start writing any code:

* Model Associations
* Nested Routes/Forms

![Model Diagram](https://i.ibb.co/B67y186/chrome-VKi-U0-Ng3-VQ.png)

I created this file using app.diagrams.net. You will potentially go through several different iterations of your associations, so don't let that discourage you. For instance, I started without the Audition_instruments join table. I was getting some strange things happening and models not saving properly….with the help of one of the Flatiron leads, I was able to realize the need for an additional join table. Also, missing from this picture is the user assignable attribute that exists on Audition_instruments.

![Routes.rb](https://ibb.co/g44w0Md)

This is what I ended up with for my routes. As you can see, I had to do some work to get my nested routes working the way I wanted. For your project, you may find it beneficial to think through and write out the experience you want your user to have when creating instances of models in your project. This will help you decide how to best setup your nested routes/forms.

## Coding
Once I felt good about my plan and had diagrammed the entire User experience, I moved on to actually writing the code. I have found that I work best by building parts in this order:

* Models
* Controller/Routes (only build methods that send to appropriate pages)
* Views (very basic...just a header)
* Controller/Views (build the logic for individual methods, then the view for that method)
* Login/Logout/Signup (can do earlier, but I like doing this at the end)
* Add CSS and Formatting (can do earlier)
* Move all logic to the Model
* DRY up all code

# Declaring Global CSS Variables

I learned how to declare global CSS variables, thanks to this [MDN page.](https://developer.mozilla.org/en-US/docs/Web/CSS/:root)

I used this as I was trying to find a color palette that I really liked.  It was super tedious to go through and change every individual color hex code….so I thought, let's DRY this up. I ended up also using this for my fonts across my CSS file.

![Global CSS Variables](https://ibb.co/hcrG7FJ)


I additionally used it to create the gradient. This is a very cool feature, and I am so excited that I figured this out! Colors and font sizes were sourced from the two websites below.
# Resources
These are some of the amazing resources that I keep coming back to:

* https://app.diagram.net -- Free Online Diagram Software...great for planning!
* https://coolors.co/palettes/trending -- Trending color palettes!
* https://type-scale.com/ -- An adjustable guide for building consistent font sizes on your website

