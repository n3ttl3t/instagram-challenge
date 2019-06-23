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
Still not working so I changed the yml to not use the orb.
 - Got the error:
```
Error calling workflow: 'build-deploy'
# Cannot find a definition for job named ...
```
so I installed the CircleCI cli:
```
brew install circleci
```
so I could run:
```
circleci config validate
```
This let me make small changes and quickly test if the yml was valid. I removed the build job but kept the deploy job and it was valid, so I pushed.
 - Still having authentication errors, tried following the docs and googling but nothing worked, so switched to Travis so I can follow the instructions I wrote on the team project:
 https://medium.com/p/7f13fefd28b6
 - After being stuck on CI for hours, decided to cut it out.
   - deleted any references to travis
   - disabled CI check in Heroku
 - Changed to use pg instead of sqlite
 - Continuing error:
 ```
 Warning: Multiple default buildpacks reported the ability to handle this app. The first buildpack in the list below will be used.
 			Detected buildpacks: Ruby,Node.js
 ```
 - New error:
 ```
 There was an error parsing your Gemfile, we cannot continue
 !     /app/tmp/buildpacks/b7af5642714be4eddaa5f35e2b4c36176b839b4abcd9bfe57ee71c358d71152b4fd2cf925c5b6e6816adee359c4f0f966b663a7f8649b0729509d510091abc07/vendor/ruby/heroku-18/lib/ruby/2.5.0/rubygems.rb:289:in `find_spec_for_exe': can't find gem bundler (>= 0.a) with executable bundle (Gem::GemNotFoundException)
 ```
 - Searched it and was advised to run
 ```
 gem update --system
 bundle install
 ```
 - Error persisting, not getting anywhere.
 - Found a json file, could be to do with the buildpacks confliction, deleted it.
 - Noticed error references ruby 2.5.0 but I'm specifying 2.6.0, so changed it with rvm and in the gemfile and ran bundle yet again.
 - Still not working and googling the errors isn't helping. The official docs suggest using bundler 2.0.1 but the terminal commands won't set it to that:
 ```
 Makerss-MacBook-Pro:instagram-challenge student$ gem install bundler -v 2.0.1
 Successfully installed bundler-2.0.1
 Parsing documentation for bundler-2.0.1
 Done installing documentation for bundler after 5 seconds
 1 gem installed
 Makerss-MacBook-Pro:instagram-challenge student$ bundler -v
 Bundler version 2.0.2
 ```
 So I tried changing it manually in the gem lock file, which worked, (which makes this whole thing seem janky and stupid) and it deployed to Heroku, but the app isn't running.
 - May as well try and reincorporate Travis now, since the errors were the same when I was using it (they were actually to do with Heroku) and now they are solved. I hope.
 - Included Travis gem
 - Ran bundle
 - Initialised Travis
 - Got the error:
 ```
 The command "bundle exec rspec" exited with 127.
 ```
 So I ran it in the terminal and got:
 ```
 can't find executable rspec for gem rspec-core. rspec-core is not currently included in
  the bundle, perhaps you meant to add it to your Gemfile?
  ```
 So I did, and now it works locally.
