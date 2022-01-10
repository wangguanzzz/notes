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

## set up gem source mirror ( important in china)
```
bundle config mirror.https://rubygems.org https://gems.ruby-china.com
```