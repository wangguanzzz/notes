## rails + boostrap integraiton
rails 6.1.4.1
ruby 2.7.4
yarn 1.22.17
node 14.15.4



switch gemset
```
#rvm gemset create rails6
# gem install rails -v 6.1.4.1

bash --login
rvm 2.7.4@rails6
```

```
rails new <project>
```


other code change
https://github.com/udemyrailscourse/alpha-blog-6/commit/3cf2925664761c697c156ecf7687721086071adc


## Heroku deploymnet
* Gemfile
```ruby
# move split3 to development
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem 'sqlite3', '~> 1.4'
end


group :production do
  gem 'pg'
end

```
* deploy to heroku
```console
git push heroku master
```
* perform heroku db:migrate
```console
heroku run rails db:migrate
```


## Naming convention
| Functional      | Naming |
| ----------- | ----------- |
| controller file name     | pages_controller.rb       |
| controller class name    | PagesController < ApplicationController |
| view   | pages/<mehtod>.html.erb        |
| model name | page |
| model file name | page.rb|
| model class name | Page |
| Table name  | pages |
| migration convention | create_pages|

## 0.7.3 using scaffold

```
# rails generate scaffold Article title:string description:text
# rails db:migrate
```
## 0.7.6
generate migration
use rails cli will make the migration file has the timestamp, that is the reason
command to create table
```
rails generate migration create_articles
```

code example: create a new table
```ruby
  def change
    create_table :articles do |t|
      t.string :title
      t.text :description
    end
  end
```

execute it
```
rails db:migrate
```
roll back the migration ( not prefered way), **always create new mirgration**
```
rails db:rollback
```

code example: add a column to existing table
command of generate migration
```
rails generate migration add_timestamps_to_articles
```
```ruby
  def change
    # created_date and upated_at are magical field names
    add_column :articcles, :created_at , :datetime
    add_column :articcles, :updated_at , :datetime
  end
```
## 0.7.8
create model
```ruby
class Article < ApplicationRecord
end
```
open the console
```
rails c
#get all artiles
Article.all
# create an article
Article.create(title: "first article",description: "desc")

a = Article.new
a.title = 'aa'
a.save

```

## 0.8.0 CRUD in console
```ruby
# in rails c
Article.find(2)
Article.first
Article.last

article = Article.first
article.destroy

```

## 0.8.2 validation
edit in model
```ruby
class Article < ApplicationRecord
    validates :title, presence: true, length: { minimum: 6, maximum: 100}
    validates :description, presence: true, length: {minimum: 10, maximum:300}

end
```
command to reload the console
```ruby
#check save error
article.save # return false
article.errors
article.errors.full_messages
```

## 0.8.4 show articles
1 create routes

routes code 
```ruby
Rails.application.routes.draw do
  resources :articles, only: [:show]
end
```
check routes command
```
# rails routes --expanded
```

in view
```
output
<%= %>

not output
<% %>
```
## 0.8.6 index articles
## 0.8.8 new article ( create a form) and create ( get the post request)
below form example makes sure it is a form use post rather than ajax
```ruby
<%= form_with scope: :article, url: articles_path, local: true do |f| %>
 <p>
    <%= f.label :title%><br/>
    <%= f.text_field :title%>
 </p>

 <p>
    <%= f.label :description%><br/>
    <%= f.text_area :description%>
 </p>
 <p>
    <%= f.submit %>
 </p>
<% end %>
```
for creating , the params need to be whitelisted ( strong parameters), and redirect_to
```ruby
  def create
    @article = Article.new(params.require(:article).permit(:title, :description))
    #render plain: @article.inspect
    if @article.save
    #redirect_to article_path(@article)  below is the short format
      redirect_to @article
    else
      render 'new'
    end
  end
```

## 0.9.2
common use
```
flash[:notice] 
flash[:alert]
```
commonly it can be put above yield in body
```ruby
    <% flash.each do | name ,msg | %>
      <%=msg%>
    <% end %>
    <%= yield %>
```

the form_with options need to check below for detail
### form_with options
https://apidock.com/rails/ActionView/Helpers/FormHelper/form_with

below edit form example, model will directly set the url and method, here is to update
```ruby
<%= form_with model: @article, local: true do |f| %>
```

update method example
```ruby
  def update
    #byebug
    @article = Article.find(params[:id]) 
    @article.update(params.require(:article).permit(:title, :description))
  end
```

## 0.9.4 edit and update
edit is making the form
update is handling the update request

## 0.9.6 destroy
controller part code example
```ruby
  def destroy
    @article = Article.find(params[:id]) 
    @article.destroy
    redirect_to articles_path
  end
```
link in view example
```ruby
 <td><%= link_to 'Delete', article_path(article), method: :delete %></td>
```
### link_to options
https://apidock.com/rails/ActionView/Helpers/UrlHelper/link_to


## 1.0.0 DRY
1. controller part
*  private method , which need to be declear at the end of class(pricate is not block, it doesn't have end). example:
; params below is a hash wrap all request params, it is like flash, accessible from all controller methods
```ruby
  private
    # Use callbacks to share common setup or constraints between actions.
    def set_article
      @article = Article.find(params[:id])
    end

    # Only allow a list of trusted parameters through.
    def article_params
      params.require(:article).permit(:title, :description)
    end
```
* before_action
```ruby
class ArticlesController < ApplicationController
  before_action :set_article, only: [ show edit update destroy ]
end
```

2. partial in view
* layout partial, you need to add layouts/
```ruby
# partial file is layouts/_messages.html.erb
  <body>
    <%= render 'layouts/messages'%>
    <%= yield %>
  </body>
```

## 1.1.0 Build homepage
1. update the css in app/assets/custom.css.scss
2. image donwload to app/assets/images

## 1.1.2 Change link
change the boostrap html link to rails link_to
```ruby
<a class="navbar-brand" id="logo" href="#">ALPHA BLOG</a>
<%= link_to 'ALPHA BLOG', root_path, class: "navbar-brand" , id: "logo" %>
```
## 1.1.4
## useful methods
text truncate example
```ruby
truncate(article.description, length: 100)
```
time ago in word
```ruby
time_ago_in_words(article.created_at)
```
## 1.1.6 form styling example
```ruby
   <div class="row justify-content-center">
      <div class="col-10">
      <%= form_with model: @article, local: true, class: "shadow mb-3 bg-info rounded p-3" do |f| %>
         <div class="form-group row" > 
            <%= f.label :title ,class: "col-2 col-form-label"%>
            <div class="col-10">
               <%= f.text_field :title, class: "form-control shadow rounded"%>
            </div>
         </div>

      <div class="form-group row" > 
         <%= f.label :description, class: "col-2 col-form-label"%>
         <div class="col-10">
            <%= f.text_area :description , class: "form-control shadow rounded", rows: 10 %>
         </div>
      </div>
      <div class="form-group row justify-content-center" >
         <%= f.submit  class: "btn btn-outline-light btn-lg" %>
      </div>

      <% end %>
      <%= link_to 'Return to articles listing', articles_path, class: "text-info text-center "  %>
      </div>
   </div>
```

## 1.1.8 validation and flash message styling
**disable default validation style**
```ruby
#config/environment.rb add
ActionView::Base.field_error_proc = Proc.new do | html_tag, instance |
    html_tag.html_safe
end
```
2. move the error message part into shared/_error.html.erb, invoking is
```ruby
<%= render 'shared/errors' %>
```

## 1.2.0
keep the original format for text
```ruby
<p class="card-text"><%= simple_format(@article.description) %></p>
```


## 1.2.4 Association
https://guides.rubyonrails.org/v6.0/association_basics.html

## 1.2.5 one to many association
1. create db migration add user_id to article table
2. add has_many in model User
3. add belongs_to in model Article
create method
```ruby
#1
Article.create(username: "", user_id: 1)
#2
Article.create(username: "", user: @user)
#3
user1.articles.build(title: "abc")
#4
user1.articles << @article

```

## 1.2.6
**git local feature branch**
```console
# create a new local branch
git checkout -b create-user-table-model
# check branch
git branch
# local add and commit
git add -A
git commit -m "update"
# back 
git checkout master
# merge
git merge <feature branch name>
```
then remove the local branch
```
git branch -d <name>
```

## 1.2.8
vaidation details
https://guides.rubyonrails.org/v6.0/active_record_validations.html

## 1.3.0
update all exising moddel , after updating the association
```ruby
Article.update_all(user_id: User.first.id)
```

## 1.3.4 model hook before_save
```ruby
class User < ApplicationRecord
    before_save {self.email = email.downcase }
end
```
## 1.3.6 add secure password
**devise gem make the authN very easy** 
 
 here just use native rail
1. uncommment bcrypt in Gemfile
2.  add password_digest column to users table
```console
$ rails generate migration add_password_digest_to_users
```
3. update User model
```ruby
has_secure_password
```
4. user authN
```
user = User.first
user.password= "password"
# if pass, will return user
user.authenticate("password")
```

## 1.3.8 user signup form

user signup routes
```ruby
#routes.rb
# root route
root "chatroom#index"
get 'signup', to: "users#new"
```
**email field and password_field**
 ```ruby
 <%= f.email_field :email , class: "form-control shadow rounded" %>
  <%= f.password_field :email , class: "form-control shadow rounded" %>
```
**VERY IMPORTANT** how to share partial
```ruby
# invoking partial, and replace obj with model article
<%= render 'shared/errors', obj: @article %>
```
## 1.4.2 edit user
check if obj is new record
```ruby
<%= f.submit @user.new_record?  ? "sign up": "submit",  class: "btn btn-outline-light btn-lg" %>
```

## 1.4.4 APPLICATION HELPER
**in helpers folder, you could create global helper methods**
in view
```ruby
<%= gravatar_for @user %>
```

in helpers/application_helper.rb
```ruby
    def gravatar_for(user, options = {size: 80 })
        email_address = user.email.downcase
        hash = Digest::MD5.hexdigest(email_address)
        size = options[:size]
        gravatar_url = "https://www.gravatar.com/avatar/#{hash}?s=#{size}"
        image_tag(gravatar_url, alt: user.username)
    end
```

## 1.4.6 user index
link_to is not only creating the link text, it can also used for picture
```ruby
<%= link_to gravatar_for(user, size:80 ) %>
```


**pluralize**复数方法
```ruby
pluralize(user.articles.count, "article“)
```

## 1.5.0 pagination
add **will_paginate ** gem
https://github.com/mislav/will_paginate
``` ruby
 gem 'will_paginate', '~> 3.1.0'
```

styling:
http://mislav.github.io/will_paginate/?page=2

1. in controller
```ruby
 @articles = Article.paginate(page: params[:page], per_page: 3)
```
2. in view
```ruby
  <div class="flickr_pagination mb-2">  
    <%= will_paginate @articles, :container => false %>
  </div>
```

## 1.5.2 login && sessions management
1. routes
```ruby
  get 'login', to: "sessions#new"
  post 'login', to: "sessions#create"
  delete 'logout', to "sessions#destroy"
```
2. controller

3. viewer
in viewer form, the model cannot be used any more, it has to use scope
```ruby
<%= form_with scope: :session, url: login_path, local: true, class: "shadow mb-3 bg-info rounded p-3" do |f| %>
```
then the handle parameter will get all params in params[:session]

## 1.5.4 create and detroy session
**session** 
https://guides.rubyonrails.org/v6.0/action_controller_overview.html#accessing-the-session


example:
```ruby
    def create
        user = User.find_by(email: params[:session][:email].downcase)
        if user && user.authenticate(params[:session][:password])
            session[:user_id] = user.id
            flash[:notice] = "log in successfully"
            redirect_to user
        else
            flash.now[:alert] = "there is something wrong with your username/password"
            render 'new'
        end
    end

    def destroy
        session[:user_id] = nil
        flash[:notice] = "Logged out"
        redirect_to root_path
    end

```

## 1.5.6 authN helper
```ruby
    def current_user
        @current_user ||= User.find(session[:user_id]) if session[:user_id]
    end

    def logged_in?
        !!current_user
    end
```


@current_user avoid to read the database again and again.
**application helper can be only userd in view** 

## 1.5.8 controller helper & view helper
move the current_user to application_controller, and add helper_method , make sure it can be accessed anywhere
```ruby
class ApplicationController < ActionController::Base

    helper_method :current_user,:logged_in?

    def current_user
        @current_user ||= User.find(session[:user_id]) if session[:user_id]
    end
 
    def logged_in?
        !!current_user
    end


end
```

## 1.6.4 restiruction from controller
* global level ,#applicaton_controller.rb
```ruby
#check if login
    def require_user
        if !logged_in?
          flash[:alert] = "you must be logged in "
          redirect_to login_path
        end
    end
```
* model specific level, model controller (articles_controller.rb)
```ruby
    def require_same_user
      if current_user != @article.user
        flash[:alert] = "wront user"
        redirect_to @article
      end
    end
```

* the order of checking in controller is important
```ruby
# article controller
class ArticlesController < ApplicationController
  before_action :set_article, only: [ :show,:edit, :update, :destroy ]
  before_action :require_user, except: [:index,:show]
  before_action :require_same_user, only: [:edit, :update, :destroy]
```

## 1.6.8 delete user & its associated articles
model level
```ruby
class User < ApplicationRecord
  has_many :articles, dependent: :destroy
end
```

in user controller
```ruby
def destroy
  @user.destroy
  session[user_id] = nil
end
```

## 1.7.0 admin user [permissions functionality]
in ERP or CRM , it will need the permissions table
in simple applicaiton, there is some typical roles like admin, moderator, etc

1. in db migration, add_colume has default option defines the default value
2. for boolean field, User.admin? can directly check value
3. User.toggle!(:admin) can revert the boolean field value

## 1.7.4 change flash color
**the ruby in html <%%> can update any place**
```ruby
  <div class="alert alert-<%= name == "notice"? "success" : "danger" %> alert-dismissible fade show" role="alert">
```


## 1.7.6 Heroku
enter heroku rails console
```console
heroku run rails console
```

## 1.7.8 测试框架

asserts
https://guides.rubyonrails.org/testing.html#available-assertions

create unit test example

    test/models/category_test.rb

unit test 主要针对model , validation 

code example: https://github.com/udemyrailscourse/alpha-blog-6/commit/8f207aa1d5f7239aea7aff88ee07fd171451cfce

## 1.8.1 unit test

一下方法会每个 test 前运行
```ruby
def setup
  #you could intialize an object
  @cattegory = Category.new()

end
```
code example: https://github.com/udemyrailscourse/alpha-blog-6/commit/50fcd16273f5295f4ef7c92ef7019115f6f06e7f

## 1.8.2 functional test (测试controller)
会检查
1. route
2. controller
3. view templates

use below command generate functional test for category

```bash
# rails generate test_unit:scaffold category
```
it will generate

    test/controllers/categories_controller_test.rb
    test/system/categories_test.rb

run controller test
```
rails test test/controllers

# run unit and controller
rails test
```

## 1.8.4
check the number of category change
```ruby
assert_difference('Category.count',1) do 
end
``` 

## 1.8.5 integration test

  https://guides.rubyonrails.org/testing.html#integration-testing

create test
```
rails generate integration_test user_flows
```

code example:
https://github.com/udemyrailscourse/alpha-blog-6/commit/fd0c4f8a2fee0adeef9a7058f25c139cf0eb6760



## 1.8.8
check if component exists 
```
assert_select 'div.alert'
assert_select 'h4.alert-heading'
```
## 1.9.0
check link exist with specific value
```
assert_select "a[href=?]", catogory_path(@category), text: @category.name
```
## 1.9.2
helper like sign_in_as(user) can be defined in test/controllers/test_helper.rb

## 1.9.6 many to many association
has_many through
https://guides.rubyonrails.org/association_basics.html#the-has-many-through-association

## 1.9.7
when association is built, can use below operation to add mapping
``` ruby
category = Category.last
category.articles << artile
```

## 1.9.9
whitelist category

```ruby
def article_params
  params.require(:article).permit(:title, :description, category_ids: [])
end
```

mutliple selection & bootstrap looks:
https://github.com/udemyrailscourse/alpha-blog-6/commit/d307ea65a16e93aa700fa6a691694cd82a38fca1

## 2.0.1
viewer, when render an array, it will render it one by one. it expect partial category
```ruby
<%=render @uarticle.categories%>
```

parial _category.html.erb
```ruby
<%= category %>
```

## 2.1.4 Intergration with Semantic UI (rails5)
add below in Gemfile
```ruby
gem 'semantic-ui-sass'
gem 'jquery-rails'
```
follow the steps in 
https://github.com/doabit/semantic-ui-sass

video steps
https://www.udemy.com/course/the-complete-ruby-on-rails-developer-course/learn/lecture/12715243#overview


enable dropdown link, append it after application.js
```javascript
$(document).on('turbolinks:load',function (){
  $('.ui.dropdown').dropdown();
})

```

## 2.1.7 Favicon
download the favicon from website like https://gauger.io/fonticon/
put the favicon.ico into app/assets/images/
```ruby
# update the applicaton.html.erb
    <title>MessageMe</title>
    <%= favicon_link_tag %>
```

## 2.2.2 db seed 
in db/migration/seeds.rb, the initial data can be added
```ruby
User.create(username: 'wangguanzzz1', password: 'password')
User.create(username: 'wangguanzzz2', password: 'password')
User.create(username: 'wangguanzzz3', password: 'password')
```
```console
rails db:seed
```
in Gemfile add gem 'hirb', it can show data in rails console in table format
```console
#in rails console
Hirb.enable
User.all
```

validate uniqueness:
```ruby
validates :username, uniqueness: {case_sensitive: false}
```

## 2.2.5 render array partial
in controller level
```ruby
    def index
        @messages = Message.all
    end
```

as it will render model Message array, 
1. create view/messages
2. create view/messages/_message.html.erb
```ruby
<div class="event">
    <div class="content">
    <div class="summary">
        <em><%= message.user.username%></em>: <%= message.body %>
    </div>
    </div>
</div>
```
3. in view of chatroom index
```ruby
<div class="ui  feed">
    <%= render @messages %>
</div>
```

## 2.2.6 authentication system
below code looks can be used in any application.
```ruby
class ApplicationController < ActionController::Base
  helper_method :current_user, :logged_in?

  def current_user
    @current_user ||= User.find(session[:user_id]) if session[:user_id]
  end
  
  def logged_in?
    !!current_user
  end

  def require_user
    if !logged_in?
        flash[:error] = "you must log in"
        redirect_to login_path
    end
  end
end

```
login in controller
```ruby
class SessionsController < ApplicationController 
  def new      
  end

  def create
    session_params
    user = User.find_by(username: params[:session][:username])
    if user && user.authenticate(params[:session][:password])
        session[:user_id] = user.id
        flash[:notice] = "log in successfully"
        redirect_to root_path
    else
        flash.now[:alert] = "there is something wrong with your username/password"
        render 'new'
    end
  end

  private
  def session_params
    params.require(:session).permit(:username, :password)
  end
end
```

## 2.2.8 flash messages
to enable dismissable messsage
```javascript
# applicaiton.js
$(document).on('turbolinks:load',function (){
  $('.ui.dropdown').dropdown();
  $('.message .close')
  .on('click', function() {
    $(this)
      .closest('.message')
      .transition('fade')
    ;
  });
})
```

## 2.3.0 form for message IN INDEX view (IMPORTANT)
1. create message handle in route
```ruby
  post 'message', to: "messages#create"
```
2. in the chatroom view, build a form send post request to /message
```ruby
<%= form_for @message, class: "ui reply form", url: message_path do |f|%>
  <div class="field">
      <div class="ui  fluid icon input ">
      <%= f.text_field :body,placeholder: "input text" %>
      # create a form button with icon
      <%= f.button  '<i class="bordered inverted orange icon edit"></i>'.html_safe %>
      </div>
  </div>
<% end %>
```
3. in message controller, handle the request, and redirect to chatroom view
```ruby
class MessagesController < ApplicationController 
    before_action :require_user

    def create
        message = @current_user.messages.build(message_params)
        redirect_to root_path if message.save
    end

    private
    def message_params
        params.require(:message).permit(:body)
    end
end
```

## 2.3.2 Websocket on ACTIONCABLE
1. generate a chatroom channel
2. update messages_controller create  - boradcast data to channel
3. write some JS to handle the data - receive the data and append to chat window
4. update styling to handle the data

## 2.3.3 Generate chatroom channel
```console
rails generate channel chatroom
```
routes.rb
```ruby
mount ActionCable.server, at: '/cable'
```
chatroom_channel.rb
```ruby
class ChatroomChannel < ApplicationCable::Channel
  def subscribed
    stream_from "chatroom_channel"
  end

  def unsubscribed
    # Any cleanup needed when channel is unsubscribed
  end
end

```
**needed in production** 
in config
```ruby
 config.action_cable.allowed_request_origins = ["https://cloudbase_url"]
```

## 2.3.5 display messages using partial
make usre the form is submitted by async javascript, remote true
```ruby
 <%= form_for @message, class: "ui reply form", url: message_path, remote: true do |f|%>
```

in controller sending out the broad cast message, update the broadcast message with a html message, use a private controller method build it, which can use existing partial, add the local variable 
```ruby
    def create
        message = @current_user.messages.build(message_params)
        if message.save
            ActionCable.server.broadcast "chatroom_channel", 
            mod_message: message_render(message)
        end
    end

    private
    #render the message partial from controller
    def message_render(message)
        render(partial: 'message', locals: {message: message})
    end
```
then update the coffe script
```coffee
App.chatroom = App.cable.subscriptions.create "ChatroomChannel",
  connected: ->
    # Called when the subscription is ready for use on the server

  disconnected: ->
    # Called when the subscription has been terminated by the server

  received: (data) ->
    $('#message-container').append data.mod_message

```

## 2.3.6 add scrolling to chat window
add id to the view
```html
<div class="content" id="messages">  
        <div class="ui  feed" id="message-container">
            <%= render @messages %>
        </div>
</div>
```
add overflow auto to css
```css
#messages {
    height: 15em; // this line affect the look
    overflow: auto
}
```

add scroll down function to application js. and use it everytime reload chatroom
```javascript
scroll_bottom =function(){
  if ($('#messages').length > 0 ){
    $('#messages').scrollTop($('#messages')[0].scrollHeight);
  }
}

$(document).on('turbolinks:load',function (){
  $('.ui.dropdown').dropdown();
  $('.message .close')
  .on('click', function() {
    $(this)
      .closest('.message')
      .transition('fade')
    ;
  });
  scroll_bottom();
})

```
in channel coffeee, invoke this funciton too, when message received.
```coffee
App.chatroom = App.cable.subscriptions.create "ChatroomChannel",
  connected: ->
    # Called when the subscription is ready for use on the server

  disconnected: ->
    # Called when the subscription has been terminated by the server

  received: (data) ->
    $('#message-container').append data.mod_message
    scroll_bottom()

```

## 2.4.6 Devise -- install devise gem
https://github.com/heartcombo/devise#starting-with-rails

```ruby
# Gemfile
gem 'devise'
```

```console
rails generate devise:install
```
## 2.4.7 create user with Device
```console
rails generate devise User
rails db:migrate
```
```ruby
#application.html.erb, this is required in devise instruments
  <body>
    <p class="notice"><%= notice %></p>
    <p class="alert"><%= alert %></p>
    <%= yield %>
  </body>
```
add checking in all controller
```ruby
class ApplicationRecord < ActiveRecord::Base
  before_action :authenticate_user!
  self.abstract_class = true
end

```
