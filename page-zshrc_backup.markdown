---
layout: page
title:  ".zshrc backup"
---
```sh
# Setup nano as default editor

export EDITOR=nano

# Setup default node env to development

export NODE_ENV=development

# Setup default rails env to development

export RAILS_ENV=development

# Setup Github token to use gh cli

export GH_TOKEN=********

# Open current folder with Finder

alias f="open ."

# Open current folder with VSCode

alias c="code ."

# Go to desktop quickly

alias cdd="cd /Users/galante/Desktop"

# Go to Workspace quickly

alias cdw="cd /Users/galante/Workspace"

# Go to Wordpress themes folder quickly on Local project

alias wp-local-themes="cd app/public/wp-content/themes"

# Go to Wordpress plugins folder quickly on Local project

alias wp-local-plugins="cd app/public/wp-content/plugins"

# Do a rapid commit and push to Github

gcom () {
    git add -A
    git commit -m "$1"
    git pull
    git push
    git fetch
}

# Clean up github branches

gclean () {
    git fetch -p ; git branch -r | awk '{print $1}' | egrep -v -f /dev/fd/0 <(git branch -vv | grep origin) | awk '{print $1}' | xargs git branch -d
}

# Clean up Workspace folder

alias sizesw="cd /Users/galante/Workspace && find . -maxdepth 1 -mindepth 1 -type d -exec du -hs {} \;"

cleanw () {
  cd /Users/galante/Workspace
  # Remove all .DS_Store files
  find . -name '*.DS_Store' -type f -delete
  # Remove all node_modules folders
  find . -name 'node_modules' -type d -prune -exec rm -rf '{}' +
  # Remove all .log files
  find . -name '*.log' -type f -delete
  # Remove all development.sqlite3 files
  find . -name 'development.sqlite3' -type f -delete
}

# Run rails tasks quickly

alias rails-db-dev="rails db:drop RAILS_ENV=development && rails db:create RAILS_ENV=development && rails db:migrate RAILS_ENV=development && rails db:seed RAILS_ENV=development"

alias rails-db-test="rails db:drop RAILS_ENV=test && rails db:create RAILS_ENV=test && rails db:migrate RAILS_ENV=test && rails db:seed RAILS_ENV=test"

# Compile React native project for Android

alias rn-compile-aab="cd android && ./gradlew clean && ./gradlew bundleRelease && cd app/build/outputs/bundle/release && open ./"

alias rn-compile-apk="cd android && ./gradlew clean && ./gradlew assembleRelease && cd app/build/outputs/apk/release && open ./"

alias rn-compile-apk-dev="cd android && ./gradlew assembleDebug && cd app/build/outputs/apk/debug && open ./"

# Run ngrok quickly

alias ngrok-80="ngrok http --domain=current-amusing-moray.ngrok-free.app 80"

alias ngrok-3000="ngrok http --domain=current-amusing-moray.ngrok-free.app 3000"
```