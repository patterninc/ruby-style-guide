# Ruby Style Guide
Ruby Style Guide for Pattern Ruby Projects


# Updating rubocop.yml
When you modify rubocop.yml, all consuming repositories would start getting voilations depending on the rules you change. Whenever this file is updated, we need to regenerate .rubocop_todo.yml in all projects. This will add newly discovered voiloations into .rubocop_todo.yml file and prevent builds from breaking.

Regenrate todo using this command
`rubocop --auto-gen-config --auto-gen-only-exclude --exclude-limit 10000`
