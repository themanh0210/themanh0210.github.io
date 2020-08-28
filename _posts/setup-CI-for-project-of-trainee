---
layout: post
title: Setup Sun*CI for project of trainee
subtitle: Sun*CI
cover-img: /assets/img/path.jpg
tags: [CI]
---
# Bước 1: Enable CI 
truy cập https://ci.sun-asterisk.com/account/github/repositories?scm=github&namespace=awesome-academy <br>
click enable CI cho project tương ứng
# Bước 2: Thêm gem
- thêm những gem sau nếu chưa có vào Gemfile:

```ruby
group :development, :test do
  gem "bundle-audit"
  gem "rails_best_practices"
  gem "rubocop", "~> 0.74.0", require: false
  gem "rubocop-checkstyle_formatter", require: false
  gem "rubocop-rails", "~> 2.3.2", require: false
  gem "rspec-rails", "~> 4.0.1"
end
```

```barsh
bundle install
rails generate rspec:install
```

# Bước 3: Tạo file .sun-ci.yml & database-ci.yml
## .sun-ci.yml:

```ruby
workspace: true

stages:
- build
- test

jobs:
- name: build:prepare
  stage: build
  image: sunci/ruby:2.7.0 #thay thế bản ruby tương ứng, ví dụ sunci/ruby:2.6.5, manhbnt/ruby:2.7.1 trường hợp ruby ver 2.7.1
  script:
  - cp database-ci.yml config/database.yml
  - bundle _2.1.2_ install --path vendor/bundle
  cache:
  - key: vendor_$CI_BRANCH
    paths:
    - vendor/bundle

- name: test:rspec
  stage: test
  image: sunci/ruby:2.7.0
  services:
  - image: mysql:5.7.22
    name: mysql
    environment:
      MYSQL_DATABASE: database_name
      MYSQL_USER: mysql_user
      MYSQL_PASSWORD: mysql_password
      MYSQL_ROOT_PASSWORD: root
  before_script:
  - bundle _2.1.2_ install --path vendor/bundle
  script:
  - bundle _2.1.2_ exec rspec

- name: test:bundle_audit
  stage: test
  image: sunci/ruby:2.7.0
  before_script:
  - bundle _2.1.2_ install --path vendor/bundle
  script:
  - bundle exec bundle audit check --update

- name: test:rubocop
  stage: test
  image: sunci/ruby:2.7.0
  before_script:
  - bundle _2.1.2_ install --path vendor/bundle
  script:
  - bundle exec rubocop --require rubocop/formatter/checkstyle_formatter --format RuboCop::Formatter::CheckstyleFormatter --no-color --out .framgia-ci-reports/rubocop.xml app/ lib/

- name: test:rails_best_practice
  stage: test
  image: sunci/ruby:2.7.0
  before_script:
  - bundle _2.1.2_ install --path vendor/bundle
  script:
  - bundle exec rails_best_practices -e "db/schema.rb,db/migrate,vendor,app/models/application_record.rb" app/ lib/ -c .rails_best_practices.yml
```

## database-ci.yml
```ruby
default: &default
  adapter: mysql2
  encoding: utf8
  port: 3306
  pool: 5

test:
  <<: *default
  database: database_name
  host: mysql_test
  username: mysql_user
  password: mysql_password
```
