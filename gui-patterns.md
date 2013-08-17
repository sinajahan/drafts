# 5 GUI Patterns That help to have skinny rails classes

Big classes can create maintainability problems in application. They are hard to change and everyone on your team hatest to touch them. You start with clean beautiful class and before you know it has become hundred lines of code. Big classes are usually a sign that your class is doing too many things. They violate the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle) and they should be refactored. Here I am going to review a couple of refactoring patters specific to user interface that you can have in your toolbox. Applying these kind of extracting logic from classes should be a an ongoing process in any code base.

## ActionController View Helpers - know them but you can do better

When I wrote my "hello world" application in Rails I fell in love with [view helpers](http://api.rubyonrails.org/classes/ActionController/Helpers.html). You are coding in your erb file and have some logic specific to view. Rails has already provided a place for that. Yay! Lets add a helper in `app/helpers`. For example a simple logic to handle caption of a label: 

````ruby
  module CandidateHelper
  # app/helpers/candidate_helper.rb
  ...
    def caption candidate
      candidate.distorted? ? "Application Distortion" : "Application Pass"
    end
  ...
  end

````

It was cool to put this in a helper and forget about it. Rails was "magically" loading my method in erb and I could simple call it:

````erb
  <% @candidates.each do |candidate| %>
    <%= caption candidate %><br>
  <% end %>
````

After all this was all "convention over configuration" of Rails. After Rails 3 you can easily write tests for your helpers too:

````ruby
  require 'test_helper'
  
  class WelcomeHelperTest < ActionView::TestCase
    test 'returns the correct caption' do
      c = Candidate.new(distorted:true)
      assert_equal "Application Distortion", caption(c)
    end
  end
````

But lets take a closer look now with your object oriented expert hat on. This helper is a ruby module. That means no inheritance, hard reuse...and you don't get to work with your lovely objects. 

## View Objects 

## Presenters

## Exhibitors

## Decorators 
