---
layout: post
permalink: /blog/2012/02/faster-data-input-using-form-navigation/
title: Faster Data Input using Form Navigation
date: 2012-02-25 20:30:32+00:00
excerpt: "In order to help, we worked on a jQuery snippet that allows the use of keys on the keyboard to navigate from one field to the next..."

author:
  name: Tim Akinbo
  twitter: takinbo
  gplus: takinbo 
  bio: Founder
  image: takinbo.jpeg
  wordpress_id: 201
  comments: true
categories:
- Software Development
---


[![Project SwiftCount Checklist Entry Form](http://www.timbaobjects.com/wp-content/uploads/2012/02/psc_checklist_form-680x389.png)](http://www.timbaobjects.com/wp-content/uploads/2012/02/psc_checklist_form.png)

Data entry is at the heart of the operations at [Project SwiftCount](http://www.pscnigeria.org/). Our data clerks have to fill forms like the one in the screenshot above over and over again. In studying how they accomplish this task, we noticed a certain activity that takes a lot of time - leaving the keyboard to use the mouse.

Every time a data clerk wants to navigate from one input field to the next, they have to reach out to the mouse and click on the next field. The really smart ones had figured out the tab key and use that to move from one field to the next. You'll agree with me that if you have to fill in fifty of those input fields, that's a lot of work.

In order to help, we worked on a jQuery snippet that allows the use of keys on the keyboard to navigate from one field to the next. So all the data clerk needs to do is either press the "n" key on the keyboard to navigate to the next input field or "p" to navigate to the previous. This works for us since numerals are the only valid inputs in each of these text boxes so trapping the "n" and "p" keys to perform the navigation suffices.

We've created [this snippet](https://gist.github.com/1910209) demonstrating how it was done.
