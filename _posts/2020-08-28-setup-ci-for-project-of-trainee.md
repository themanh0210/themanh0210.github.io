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
- thêm gem sau nếu chưa có vào Gemfile:

```ruby
group :development, :test do
  gem "rubocop", "~> 0.74.0", require: false
  gem "rubocop-checkstyle_formatter", require: false
  gem "rubocop-rails", "~> 2.3.2", require: false
  gem "rspec-rails", "~> 4.0.1"
end
```
tạo file **config/database.yml**
```
cp config/database.yml.example config/database.yml
```
sau đó 
```
bundle install
rails generate rspec:install
```
chạy `yarn install --ignore-engines` nếu Yarn packages are out of date 


# Bước 3: Tạo file .sun-ci.yml & database-ci.yml
check image ruby hiện có tại [đây](https://hub.docker.com/r/sunci/ruby/builds)
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
       MYSQL_DATABASE: db_test
      MYSQL_USER: user_test
      MYSQL_PASSWORD: password_test
      MYSQL_ROOT_PASSWORD: password_test
  before_script:
  - bundle _2.1.2_ install --path vendor/bundle
  script:
  - bundle _2.1.2_ exec rspec

- name: test:rubocop
  stage: test
  image: sunci/ruby:2.7.0
  before_script:
  - bundle _2.1.2_ install --path vendor/bundle
  script:
  - bundle exec rubocop --require rubocop/formatter/checkstyle_formatter --format RuboCop::Formatter::CheckstyleFormatter --no-color --out .framgia-ci-reports/rubocop.xml app/ lib/
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
  database: db_test
  host: mysql_test
  username: user_test
  password: password_test
```
