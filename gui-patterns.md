# 5 GUI Patterns That help to have skinny rails classes

Big classes can create maintainability problems in application. They are hard to change and everyone on your team hatest to touch them. You start with clean beautiful class and before you know it has become hundred lines of code. Big classes are usually a sign that your class is doing too many things. They violate the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle) and they should be refactored. Here I am going to review a couple of refactoring patters specific to user interface that you can have in your toolbox. Applying these kind of extracting logic from classes should be a an ongoing process in any code base.

## View Helpers from ActionController 
### Know them but you can do better

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

## View Objects or Presentors

The idea is very simple: Your controller will deligate the logic of view
related functions to a seperate poro object.

For example imagine the logic of showing the default candidate avatar when user
has not provided any image.

````ruby
  class CandidateView
    def initialize candidate
      @candidat = candidate
    end
  
    def avatar_name
      if candidate.avatar_image_name.present?
        candidate.avatar_image_name
      else
        "default.png"
      end
    end

  end
````

For cleaner codebase I have created a folder `app/view_objects` in my
rails app and have asked rails to load classes in that folder in my
`application.rb` file:

````ruby
  ...
  class Application < Rails::Application
    # Custom directories with classes and modules you want to be autoloadable.
    config.autoload_paths += %W(
      #{Rails.root}/app/view_objects
    )
````

And then I use it in my Controller:

````ruby
  class WelcomeController < ApplicationController
    ...
    def index 
      @view = CandidateView.new Candidate.find(params[:id])
    end
  end
````

And finally my view will just call the required method on the `@view`
object:

````erb
  <%= @view.avatar_name =%>
````

As you saw in above example we are extracting view logic to a separate
class without any need from Rails magic. Sience we are using plain old
ruby object there is no magic required in testing these objects either.


## Decorators

Decorator pattern was originally introduced by [Gang of Four's book](http://www.amazon.ca/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612).
It is a design pattern that allows behaviour to be added to an
individual object.

In our view object example we are initializating a new class and having
methods implement view logic. In decorator pattern for view templates we
are wrapping the active record model class and decorating it with new
behaviour.

There are multiple ways to implement a decorator pattern in Rails. There
is an awesome gem called [draper](https://github.com/drapergem/draper)
only for this purpose. Rails cast has [an episode](http://railscasts.com/episodes/286-draper) on it too which he goes
through the process of refactoring a heavy view templater and helper to
decorator design pattern.

The idea of using a decorator design pattern for extracting view logic
is simple. You have your rails model class that has all the data you
want. Now lets add some methods as behaviour to it for view and have
them separated in their own class.

## Exhibitors


