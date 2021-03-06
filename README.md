# Modern Rails Template

## Description

This is the application template for modern Rails 5.1 projects, including `react_on_rail`, `webpacker` and `foreman` . 


## Requirements

This template currently works with:

* Rails 5.1.x
* PostgreSQL
* chromedriver

If you need help setting up a Ruby development environment, check out my [Rails OS X Setup Guide](https://mattbrictson.com/rails-osx-setup-guide).

## Installation

*Optional.*

To make this the default Rails application template on your system, create a `~/.railsrc` file with these contents:

```
-d postgresql
-m https://raw.githubusercontent.com/johnmcdowall/modern-rails-template/master/template.rb
```

## Usage

This template assumes you will store your project in a remote git repository (e.g. Bitbucket or GitHub) and that you will deploy using Capistrano to staging and production environments. It will prompt you for this information in order to pre-configure your app, so be ready to provide:

1. The git URL of your (freshly created and empty) Bitbucket/GitHub repository
2. The hostname of your staging server
3. The hostname of your production server

To generate a Rails application using this template, pass the `-m` option to `rails new`, like this:

```
rails new blog \
  -d postgresql \
  -m https://raw.githubusercontent.com/johnmcdowall/modern-rails-template/master/template.rb
```

*Remember that options must go after the name of the application.* The only database supported by this template is `postgresql`.

If you’ve installed this template as your default (using `~/.railsrc` as described above), then all you have to do is run:

```
rails new blog
```

## What does it do?

The template will perform the following steps:

1. Generate your application files and directories
2. Ensure bundler is installed
3. Create the development and test databases
4. Commit everything to git
5. Check out a `development` branch
6. Push the project to the remote git repository you specified

## What is included?

#### These gems are added to the standard Rails stack

* Core
    * [active_type][] – for building simple and effective form/service objects
    * [sidekiq][] – Redis-based job queue implementation for Active Job
* Configuration
    * [dotenv][] – in place of the Rails `secrets.yml`
* Utilities
    * [annotate][] – auto-generates schema documentation
    * [awesome_print][] – try `ap` instead of `puts`
    * [better_errors][] – useful error pages with interactive stack traces
    * [guard][] – runs tests as you develop; mandatory for effective TDD
    * [livereload][] – magically refreshes browsers whenever you save a file
    * [rubocop][] – enforces Ruby code style
    * [xray-rails][] – inspect view partials in the browser
* Deployment
    * [capistrano-atlas][] – capistrano recipes
* Security
    * [brakeman][] and [bundler-audit][] – detect security vulnerabilities
    * [secure_headers][] – hardens your app against XSS attacks
* Testing
    * [mocha][] – excellent mocking for Test::Unit/Minitest
    * [simplecov][] – code coverage reports
    * [shoulda][] – shortcuts for common ActiveRecord tests

#### Postmark

I like to use [Postmark][] for transactional email, and so I've included the [postmark-rails][] gem and configured it in `environments/production.rb`. Make sure to sign up for a Postmark account to get an API key, or switch to your own preferred email provider before deploying your app.


#### Other tweaks that patch over some Rails shortcomings

* A much-improved `bin/setup` script
* Log rotation so that development and test Rails logs don’t grow out of control

#### Plus lots of documentation for your project

* `README.md`
* `PROVISIONING.md`
* `DEPLOYMENT.md`

## How does it work?

This project works by hooking into the standard Rails [application templates][] system, with some caveats. The entry point is the [template.rb][] file in the root of this repository.

Normally, Rails only allows a single file to be specified as an application template (i.e. using the `-m <URL>` option). To work around this limitation, the first step this template performs is a `git clone` of the `mattbrictson/rails-template` repository to a local temporary directory.

This temporary directory is then added to the `source_paths` of the Rails generator system, allowing all of its ERb templates and files to be referenced when the application template script is evaluated.

Rails generators are very lightly documented; what you’ll find is that most of the heavy lifting is done by [Thor][]. The most common methods used by this template are Thor’s `copy_file`, `template`, and `gsub_file`. You can dig into the well-organized and well-documented [Thor source code][thor] to learn more.

[active_type]:https://github.com/makandra/active_type
[sidekiq]:http://sidekiq.org
[dotenv]:https://github.com/bkeepers/dotenv
[annotate]:https://github.com/ctran/annotate_models
[awesome_print]:https://github.com/michaeldv/awesome_print
[better_errors]:https://github.com/charliesome/better_errors
[guard]:https://github.com/guard/guard
[livereload]:https://github.com/guard/guard-livereload
[rubocop]:https://github.com/bbatsov/rubocop
[capistrano-atlas]:https://github.com/johnmcdowall/capistrano-atlas
[Postmark]:http://postmarkapp.com
[postmark-rails]:http://www.rubydoc.info/gems/postmark-rails/0.12.0
[brakeman]:https://github.com/presidentbeef/brakeman
[bundler-audit]:https://github.com/rubysec/bundler-audit
[secure_headers]:https://github.com/twitter/secureheaders
[mocha]:https://github.com/freerange/mocha
[shoulda]:https://github.com/thoughtbot/shoulda
[simplecov]:https://github.com/colszowka/simplecov
[application templates]:http://guides.rubyonrails.org/generators.html#application-templates
[template.rb]: template.rb
[thor]: https://github.com/erikhuda/thor
