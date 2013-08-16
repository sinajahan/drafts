# 5 GUI Patterns That help to have skinny rails classes

Big classes can create maintainability problems in application. You start with clean beautiful class and before you know it has become hundred lines of code. Big classes are usually a sign that your class is doing too many things. They violate the [single responsibility principle](https://en.wikipedia.org/wiki/Single_responsibility_principle).

## View helpers - know them but you can do better

When I wrote my "hello world" application in Rails I fell in love with [view helpers](http://api.rubyonrails.org/classes/ActionController/Helpers.html). I was coding in my erb file and I had some logic to handle.

````ruby
  module ApplicationHelper
  # app/helpers/application_helper.rb
  ...

    def application_status
      if candidate.has_distortion?
        caption = "Application Distortion"
      else
        caption = "Application Incomplete"
      end
     caption
    end
  ...
  end

````
It was cool to put this in a helper and forget about it. Rails was "magically" loading my method in erb and I could simple call it:

````erb
  <%= application_status %>
````

After all this was all "convention over configuration" of Rails. Lets take a close look now with your object oriented expert hat on. This helper is a ruby module. That means no inheritance, hard reuse...and no objects.

## View Objects aka Presenters


## Decorators 
