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