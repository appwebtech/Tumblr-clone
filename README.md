<div align="center">
<h1><a href="https://appwebtech-tumblr.herokuapp.com/">Tumblr Clone App (Old Interface)</a></h1> 
<h2>Software Development Documentation</h2>
  

<div align="center">
  <sub>Built by
  <a href="https://github.com/appwebtech">Joseph M Mwania</a>
    
  </a>
</div>
</sub>
</div>

<h3>Table of Contents</h3>

- [Introduction](#introduction)
- [What the app should do ](#what-the-app-should-do )
- [Technologies](#technologies)
  - [Devise](#devise)
  - [Normalize-css](#normalize-css)
  - [Simple form](#simple-form)
- [Deployment](#deployment)
- [Developer](#developer)
- [Contacts](#contacts)
- [License](#license)


# Introduction

In the last couple of months I decided to create short apps introducing briefly [Object Oriented Design (OOD)](https://en.wikipedia.org/wiki/Object-oriented_design) and the versatility of [RoR](https://en.wikipedia.org/wiki/Ruby_on_Rails) as a software design programming language and framework. I didn't use [TDD](https://en.wikipedia.org/wiki/Test-driven_development) or [BDD](https://en.wikipedia.org/wiki/Behavior-driven_development), but there is an application that I used Unit tests (not Rspec) that I will push later after I finish working with the UI.  

For those who are learning RoR and have been writing asking me to deploy and send links, I will deploy the apps but I don't know for how long they will stay online because I use Heroku's free dynos account when doing Mockups which has limitations and is very slow. When possible, I'll deploy images in AWS and have them served from there because Heroku deletes them one hour after deploying.

I've used ActiveRecord as the interface of the DB which has an awesome orm and is supplied with Rails 5. I used sqlite3 for development and postgres for production. I had an issue when deploying and had to do an asset precompile by running; 

```shell
NODE_ENV=production bundle exec rails assets:precompile
```

which allowed the compiler to execute successfully. A few warnings of deprecate files which I solved by updating my gems were thrown. After deploying I ran unsuccessful DB migrations. There is a bug that sometimes forces one to change the DB adapter to PG. In the past I had to use a VPN to migrate DB's due to port restrictions with my provider in Italy but now everything ran as planned. 


I won't go into depth as I'm trying to make a single documentation for about five apps (not laziness, just busy). While few clients would want an app as simple as this, it's important to note that this and other apps like Eternity_note, Market_place, Project_app etc (which I will deploy momentarily) can be integrated in a web application or even  enhanced to create one massive web application thanks to their scalability. 


# What the app should do 

The app is backed by an [RDBMS](https://en.wikipedia.org/wiki/Relational_database_management_system) DB with three models which will communicate to the DB via the controller. Below is the schema of the DB. 

```ruby 
ActiveRecord::Schema.define(version: 20171114193520) do

  create_table "comments", force: :cascade do |t|
    t.string   "name"
    t.text     "body"
    t.integer  "post_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
    t.index ["post_id"], name: "index_comments_on_post_id"
  end

  create_table "posts", force: :cascade do |t|
    t.string   "title"
    t.text     "body"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "users", force: :cascade do |t|
    t.string   "email",                  default: "", null: false
    t.string   "encrypted_password",     default: "", null: false
    t.string   "reset_password_token"
    t.datetime "reset_password_sent_at"
    t.datetime "remember_created_at"
    t.integer  "sign_in_count",          default: 0,  null: false
    t.datetime "current_sign_in_at"
    t.datetime "last_sign_in_at"
    t.string   "current_sign_in_ip"
    t.string   "last_sign_in_ip"
    t.datetime "created_at",                          null: false
    t.datetime "updated_at",                          null: false
    t.index ["email"], name: "index_users_on_email", unique: true
    t.index ["reset_password_token"], name: "index_users_on_reset_password_token", unique: true
  end

end

```


As you can see from the schema, the DB is well designed thanks to the foreign keys that I added whilst making associations and thus normalising it to 3NF. Although the [OLTP](https://en.wikipedia.org/wiki/Online_transaction_processing) are not costly for this app due to less data, it's good practice to maintain international design standards.

Via the UI, the user will register an account thanks to [Devise](https://github.com/plataformatec/devise), then input data which will be managed by the [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) functionalities using [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer) web services with the MVC architecture. 

# Technologies

The technologies I've used are mainly; 
* **ERB**, which stands for Embedded Ruby and is the standard way of generating [HTML](https://en.wikipedia.org/wiki/HTML) in Rails. 
* **Sprockets**, which compiles all JS together and serves it through the asset pipeline. In my professional applications I use [Babel ECMAScript](https://babeljs.io/learn-es2015/)  which compiles JS in a sweet flavor and works great with Isomorphic HTML. This is good for SEO as it allows the full markup to be available in the [DOM](https://en.wikipedia.org/wiki/Document_Object_Model) which in turn enabled Google, Bing, etc to index your  page contents.

The following are the gems that I used on top of those out of the box. 

## Devise 

[![Build Status](https://api.travis-ci.org/plataformatec/devise.svg?branch=master)](http://travis-ci.org/plataformatec/devise)
[![Code Climate](https://codeclimate.com/github/plataformatec/devise.svg)](https://codeclimate.com/github/plataformatec/devise)

A very cool authentication solution for rails. I love to write less code and use the DRY principle to avoid bugs in my applications. This one came in handy.

Devise is a flexible authentication solution for Rails based on Warden. It:

* Is Rack based;
* Is a complete MVC solution based on Rails engines;
* Allows you to have multiple models signed in at the same time;
* Is based on a modularity concept: use only what you really need.

```ruby
# I installed the gem file.
gem 'devise'
```

```console
$ rails generate devise:install
```

Devise interacts well with rails and creates nested routes automatically. No need to update them. 

## Normalize CSS

<a href="https://github.com/necolas/normalize.css"><img
  src="https://necolas.github.io/normalize.css/logo.svg" alt="Normalize Logo"
  width="80" height="80" align="right"></a>

> A modern alternative to CSS resets

[![npm][npm-image]][npm-url] [![license][license-image]][license-url]
[![changelog][changelog-image]][changelog-url]
[![gitter][gitter-image]][gitter-url]


**NPM**

```sh
npm install --save normalize.css
```

**Bower**

```sh
bower install --save normalize-css
```

**CDN**

See https://cdnjs.com/libraries/normalize

**Download**

See https://necolas.github.io/normalize.css/latest/normalize.css

### What does it do?

* Preserves useful defaults, unlike many CSS resets.
* Normalizes styles for a wide range of elements.
* Corrects bugs and common browser inconsistencies.
* Improves usability with subtle modifications.
* Explains what code does using detailed comments.


## Simple form 

Build forms fast. 

```ruby
$ gem 'simple_form'
```

Installed and ran the generator.

```ruby
$ bundle install
$ rails generate simple_form:install
```

Example of a form;

```erb
<%= simple_form_for @book, :html => { :multipart => true } do |josembi| %>
    <%= select_tag(:category_id, options_for_select(@categories), :prompt => "Select a category") %>
    
    <%= josembi.file_field :book_img %> <!--Add img upload function.-->
    <%= josembi.input :title, label: "Book Title" %>
    <%= josembi.input :description %>
    <%= josembi.input :author %>
    <%= josembi.button :submit, :class => "btn-custom2" %>
<% end %>
```

# Deployment 

Deployed to heroku. See the [Eternity note](https://appwebtech-tumblr.herokuapp.com/) live web page. 

# Developer

Fullstack development done by;

Joseph Mwania

## Contacts

[GitHub](https://github.com/appwebtech)

[Appwebtech](http://www.theappwebtech.com/)

[Twitter](https://twitter.com/appwebtech)

[Facebook](https://www.facebook.com/appwebtech/)

[Pinterest](https://it.pinterest.com/appwebtech/)

[Google +](https://plus.google.com/u/1/104000565731100573953)

[LinkedIn](https://www.linkedin.com/company/18389649/admin/updates/)

## License

Available as open source under [MIT License](https://opensource.org/licenses/MIT)
