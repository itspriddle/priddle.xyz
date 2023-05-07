---
title:   Helix
company: World Wide Web Hosting, LLC
period:  Sep 2011
date:    2011-09-01
tags:
  - AWS
  - Coffeescript
  - Javascript
  - jQuery
  - Linux
  - PostgreSQL
  - Rails Engines
  - Rails
  - Ruby
  - RubyGems
  - SCSS
project_type: work
---

Helix is a custom built billing system that is used by staff and customers to
manage web hosting services. I have been a lead developer on this project
since it's inception. I helped create and deploy the initial implementation,
including the UI, while at World Wide Web Hosting, and currently maintain it
with the software engineering team at Pressed. The application was designed
using Ruby on Rails, PostgreSQL, jQuery, Coffeescript, and SCSS on top of AWS.
Helix also includes several supplemental Rails applications and Ruby gems
which I helped implement and deploy.

## Background

Helix began in late 2011 at World Wide Web Hosting (WWWH) as a full rewrite of
their existing client billing system. WWWH was a traditional shared web host,
and Helix was originally designed with this in mind. WWWH supported multiple
brands

Helix was originally created to work with _shared_ web hosting for multiple
brands. In 2015, WWWH was acquired by another hosting provider.

A small group of employees from WWWH split off to form Pressed. Helix has
since been adapted to work with our white-labeled Managed WordPress hosting
product.

## Overview

The Helix application suite is composed of 3 Rails applications and one
supporting Rails engine:

- Helix: This application is used by staff to manage products, review orders,
  provision services, generate revenue reports, etc.
- Build: This application is used by _customers_ to order and manage their
  hosting services and work with our support team (a _customer_ is an end user
  purchasing hosting).
- PartnerPanel: This application is used by _partners_ to manage their hosting
  products, clients, and revenue reports (a _partner_ is a company selling
  hosting to an end user).
- HelixCore: This Rails engine is used by each of the above applications to
  provide shared configuration, models, helpers, libraries, locales, etc. Any
  gem dependencies common to all applications are declared and initialized
  here.

> Note: Originally, there was a separate order form application, EventHorizon.
> This application was merged in to Build in 2016 in an effort to merge the
> suite into a single monolithic application.

On the backend:

- Cloudburst Provisioning: Various APIs, scripts and supporting code used
  manage the life cycle (provisioning, suspension, termination, etc.) of
  WordPress sites.

The Rails application stack:

- Ruby 2.2
- Rails 4.2
- PostgreSQL 9.4
- Redis 3.2
- Sidekiq Pro
- AWS
- ActiveMerchant

> Note: Each application shares Redis and PostgreSQL databases.

The front end stack:

- Twitter Bootstrap
- SCSS
- CoffeeScript
- jQuery
- Rails UJS
- `.html.erb` views
- SimpleForm

The test stack:

- Travis CI
- RSpec
- Capybara
- PhantomJS
- FactoryGirl
- VCR

Third party APIs/providers:

- Route53 (DNS Build, PartnerPanel and WordPress domains)
- OpenSRS (Domain registration)
- Enom (Domain registration)
- Vantiv (Credit Card Payments)
- Braintree (Credit Card Payments)
- PayPal (PayPal Payments)
- Reamaze (Helpdesk)
- Let's Encrypt (SSL for Build, PartnerPanel and WordPress sites)
- Mandrill (Incoming email)
- Mailgun (Outgoing email)
- Honeybadger (Exception notifications)
- Telesign (Fraud verification)

## Helix

Helix is the application used by staff to manage customers and partners. A few
common actions:

- Reviewing fraudulent orders and optionally pushing them through for
  provisioning
- Reviewing, voiding, refunding, or manually creating Credit Card or PayPal
  charges
- Reviewing any errors that occured when provisioning a new WordPress site
- Suspending (disabling access) a WordPress site for non-payment or abuse
- Terminating (permanently destroying) a WordPress site
- Creating new products (WordPress site configurations with specific features
  and prices)
- Mass emailing customers or partners
- Reviewing the Sidekiq (background job) queue
- Managing customer information (name, email, address, etc)


## Build

Build is the customer facing application and is where all customer initiated
actions happen. Each partner has their own instance of Build available for
their customers (eg: `build.acme.com`), so all resources are scoped by that
partner where required. Build serves two primary functions for customers:
ordering services and managing services.

### Ordering Services

The Build order form is a mostly typical order form. It accepts the customer's
name, address, credit card, etc. It also prompts for their WordPress domain
and credentials. We use these later when we setup the site. Optionally, the
customer can order a new domain, in which case, we check for its availability
and eventually order it with a third party registrar.

### Managing Services

An authenticated Build user can manage their WordPress sites, domains, billing
information, and request help from our support team.

- WordPress site management includes: adding free SSL via Let's Encrypt,
  viewing SFTP credentials, signing in to WordPress Admin, and
  ordering/installing premium plugins.
- Domain management includes: ordering new domain names, updating WHOIS info,
  and updating DNS records.
- Billing information management includes your typical fare: updating name,
  address, payment method, viewing invoices, changing passwords or enabling
  Two Factor Authentication.
- Requesting help: Build integrates with our helpdesk at Reamaze to provide a
  way to create and view support tickets.

## PartnerPanel

PartnerPanel is the partner facing application. It is used by partners to
review their clients and generate revenue reports.

PartnerPanel also includes its own order form to let a partner sign up with
Pressed. This order process handles everything necessary to let them sell
WordPress hosting:

- A new domain name is ordered and we handle all DNS automatically
- Incoming and outgoing email support is configured (via DNS and third party
  providers)
- An SSL certificate is obtained from Let's Encrypt for the new domain name
- Build is activated on their new domain with a custom logo the partner's
  uploaded during sign up

## Cloudburst Provisioning

The Cloudburst Provisioning library handles the actual logic involved with
creating and managing a WordPress site programmatically. The library is a
collection of shell scripts in various languages, APIs, and a background job
processor built on top of AWS SQS.

Provisioning a site is discussed last since it is the most in depth action.

- Termination: The site is deleted. This happens when a customer has canceled
  or totally bailed without payment.
- Suspension: The site is disabled. Visitors cannot see it in their browser.
  This happens when the customer has not payed, their site has been hacked,
  etc.
- Resume: The site is enabled and visitors can see it in their browser again.
  This happens after a customer has paid a late bill or addressed an exploited
  site, etc.
- Change Domain: The domain name used by Wordpress is changed. This happens if
  the customer decides they want to use a new domain name.
- Impersonation: A user is authenticated in WordPress Admin securely, without
  a password. This is used by staff to aid in troubleshooting.

### Provisioning

Provisioning involes everything necessary to setup a new website on our
platform:

- The hosting platform is initialized for the user (eg: $HOME created, system
  configs put in place, etc)
- If a new domain was ordered, it is registered
- DNS records are created
- WordPress is installed
- Any plugins the partner has specified are installed
- Any themes the partner has specified are installed
- Any demo content the partner has specified is imported
- Final configuration is performed via WordPress hooks or raw SQL

Once this step is complete the customer's website is live and ready for use.

<!--
**Biggest Challenge:** 

**Biggest Triumph:**
-->
