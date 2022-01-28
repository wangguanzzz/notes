## install ruby
restrictly follow this link in china
    https://ruby-china.org/wiki/install_ruby_guide

install ruby 2.7.4
```
rvm install 2.7.4 --disable-binary
```
start rvm
```
bash --login
rvm use 2.7.4 --default
```
install apt
```
apt-get install libpq-dev
```
## install nodejs
follow this link in china
    https://npmmirror.com/

install heroku
```
cnpm install -g heroku
```

## heroku push
if meet cannot resovle git.heroku.com, need to manually add heroku.com ip to /etc/hosts

## heroku deployment process
1. login 
```console

heroku login -i
user:wangguanzzz@gmail.com

```
2. add the production part in Gemfile
```ruby
group :production do
  gem 'pg'
end
```
3. move the gem 'sqlite3' and put into :development ,:test env
```ruby
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem 'sqlite3', '~> 1.4'
end
```
4. install gem, must be **bundle install --without production**
5. git add; git commit ; git push heroku master
6. do heroku db:migrate
```console
heroku run rails db:migrate
```
## set up gem source mirror ( important in china)
```
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
```

## 2.4.5
to quickly create controller and method, can use below command
```console
rails generate controller welcome index
```