---
title:   2adays.com
company: 2aDays, LLC
period:  Dec 2016
date:    2016-12-01
tags:
  - API
  - Automation
  - Billing
  - Consulting
  - Rails
  - Recurly
  - Ruby
  - Sidekiq
project_type: work
---

I developed the Rails application for 2adays.com, a website that helps student
athletes through the college recruiting process. My work involved replacing an
existing coach and school search directory with a fully custom one. The Rails
application is responsible for pulling information from a third party API for
coaches and schools, and formatting the date for use in student searches and
ratings. Recently, I implemented a new recurring subscription service.

## Background

> 2adays.com is a website that helps student athletes through the
> college recruiting process. Current college-level athletes are able to rate
> their coaches through a survey-based system. These surveys are tabulated and
> ratings are generated for each coach. High school athletes and their parents
> are able to search these coaches and surveys so that they can make better
> choices as they enter the college athletic world.

2aDays approached me in late 2016 to build a replacement for their directory
service using Rails. This application would be handed off to another developer
for long term maintenance. [Best practices][12factor] were a deliverable.

This application best represents the work I do. My contact at 2aDays gave me
loose guidelines and expectations and left me to do my best work. A little of
everything I do with Rails went into this one:

- Front end design with Bootstrap, SCSS, a splash of CoffeeScript, and good
  old `.html.erb`.
- A healthy dose of ActiveRecord.
- I18n. Totally worth it, even for just one language.
- Mirroring data from an (often quirky) "REST" API.
- Thorough testing. But not too much, you know?
- Server setup with Vagrant.
- Documentation. I made sure the next developer would know how to get around.
- I was able to ship the app in 6 weeks working nights and weekends. The
  2aDays team was very happy with the final result, and the developer that
  took over maintenance has done so with ease.

[2aDays]: https://www.2adays.com
[12factor]: https://12factor.net/
