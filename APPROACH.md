# Approach
---
### Planning

- Made a Trello board with a set up card and user stories cards, all with tasks in each. I estimated the time to set up the project and complete the first story, and set timers on the cards.

 https://trello.com/b/zro69NRX/instragram-clone
---
### Setup

- Forked and then cloned the repo:
```
git clone git@github.com:n3ttl3t/instagram-challenge.git
```
- Set up the environment:
 - Created the gem file
```
bundle init
```
 - Uncommented the rails gem and added version
 - Added the rubocop and rubocop-rails gems
 - Installed the gems
 ```
bundle
 ```
 Bundle gave this error:
 ```
 Bundler could not find compatible versions for gem "rubocop":
  In Gemfile:
    rubocop (= 0.48.1)

    rubocop-rails was resolved to 2.0.1, which depends on
      rubocop (>= 0.70.0)
 ```
 So I just removed the versions except for Ruby. Trying to bundle now gave this error:

   ```
   Rails cops will be removed from RuboCop 0.72. Use the rubocop-rails gem instead.
   Put this in your Gemfile.

   gem 'rubocop-rails'

   And then execute:

   $ bundle install

   Put this into your .rubocop.yml.

   require: rubocop-rails
   ```
   So I removed the rubocop gem and tried again.
  - Added the rubocop rails requirement to the rubocop yml
  - Initialised a new rails project:
  ```
rails new instagram-challenge
    ```
    Which created the app folder structure inside a folder, so I moved the gems over to the new gemfile, deleted the old ones, moved the rails structure to the root and deleted the now empty folder.
    In the future I'd do things in this order instead:
    - Fork and clone repo, or start a new one and git init and add remote
    - Rails init
    - Add gems
    - Bundle
 - Initialised rspec:
  ```
rspec --init
  ```
  - Trying to run linter:
  ```
rubocop
  ```
got the error:
```
.rubocop.yml: Style/IndentationConsistency has the wrong namespace - should be Layout
Configuration file not found: /Users/student/.rvm/gems/ruby-2.6.0/gems/rubocop-rails-2.0.1/config/rails.yml
```
tried:
```
bundle exec rubocop
```
same error. Couldn't get past it after 20 mins of googling, adding files ect, so just removed rubocop.

 - Researched a CI tool to use instead of Travis. Chose Circle CI because it can have multiple users (other CIs free tiers don't), and it seems to have good documentation.
 - Getting to this point had taken longer than expected, so I had to change the timers on the Trello cards.
 - Connected Github to CircleCI
 - Created .circleci folder, a config.yml file inside that and copied the provided code on the CircleCI project setup page.
 - Pushed the changes and started the build on CircleCI
 - Added a new project on Heroku, linked it to the github repo, and add the Heroku orb to the CircleCI yml.
 - Got this error in CircleCI:
 ```
remote: !	WARNING:
remote: !	Do not authenticate with username and password using git.
remote: !	Run `heroku login` to update your credentials, then retry the git command.
remote: !	See documentation for details: https://devcenter.heroku.com/articles/git#http-git-authentication
fatal: Authentication failed for 'https://heroku:@git.heroku.com/.git/'
Exited with code 128
 ```
 - So I logged in with:
 ```
heroku login
 ```
 and tried again. I got the same error, so I generated an API_KEY with:
 ```
heroku authorizations:create
 ```
and added it to the config vars on Heroku.
