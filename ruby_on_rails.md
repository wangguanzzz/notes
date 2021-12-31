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