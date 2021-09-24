# Ruby Style Guide
Ruby Style Guide for Pattern Ruby Projects

# Installation
1. Add below gems to your gemfile. If you have remote, development and test group in your gemfile, please add these gems to all envs.
    ```ruby
    gem 'rubocop'
    gem 'rubocop-thread_safety'
    gem 'rubocop-rails'
    gem 'rubocop-rspec'
    gem 'rubocop-performance'
    gem 'rubocop-sequel'
    gem 'rubocop-rake'
    ```
1. Create a new file .rubocop.yml. If you already have .rubocop.yml file, we recommend that you delete it as it may have been obsolute.
    ```bash
    touch .rubocop.yml
    ```

1. Add below code to your .rubocop.yml
    ```yaml
    inherit_from:
    - https://raw.githubusercontent.com/patterninc/ruby-style-guide/main/rubocop.yml
    - .rubocop_todo.yml
    ```

1. Run the below command to generate your todo. This will put all existing violations into TODO file which will let you ignore them for intital setup. This shouldn't be a practice to run it to exclude your violations. 
    ```bash
    rubocop --auto-gen-config --auto-gen-only-exclude --exclude-limit 10000
    ```
1. Run Rubocop before every commit to prevent new violations.
    ```bash
    bundle exec rubocop
    ```
1. Setup CircleCI. Add below code to your .circleci/config.yml
    ```yaml
    run:
        name: Static Code Analysis
        command: bundle exec rubocop
    ```
1. Add this to your .gitignore file
    ```
    .rubocop-https---raw-githubusercontent-com-patterninc-ruby-style-guide-*
    ```
# Updating Ruby Style Guide
When you modify rubocop.yml, all consuming repositories would start getting voilations depending on the rules you change. Whenever this file is updated, we need to regenerate .rubocop_todo.yml in all projects. This will add newly discovered voiloations into .rubocop_todo.yml file and prevent builds from breaking.

Regenrate todo using this command
```
rubocop --auto-gen-config --auto-gen-only-exclude --exclude-limit 10000
```

# Regenerating .rubocop_todo.yml file
Always regenerate todo file using below command
```
rubocop --regenerate-todo --auto-gen-only-exclude --exclude-limit 10000`
```