Comments
========

There are good reasons for adding comments to your code, the best being to explain how to use your software masterpiece. These kinds of “how to” comments should focus on exactly that: how to use the thing. Don’t explain why you wrote it, the algorithm that it uses, or how you got it to run faster than fast. Just tell me how to use the thing and remember that examples are always welcome.

That voice you hear in your head, the one whispering that you need to add some comments, may just be your program crying out to be rewrit- ten.

Snipets
=======

````ruby
# how to do constants:
ANTLERS_PER_MALE_MOOSE = 2

# if 0
puts 'Sorry Dennis Ritchie, but 0 is true!' if 0

# array of strings
poem_words = %w{ twinkle little star how I wonder }

# map
doc.words.map { |word| word.size }

# inject
total = words.inject(0.0){ |result, word| word.size + result}

str = %q{"Stop", she said, "I can't live without 's and "s."}
str = %Q<The time in now #{Time.now}>
a_multiline_string = "a multi-line
string"
yet_another = %Q{another multi-line string with \
no newline}
heres_one = <<EOF
This is the beginning of my here document.
And this is the end.
EOF

puts 'yes yes'.sub( 'yes', 'no' )
puts 'yes yes'.gsub( 'yes', 'no' )

"It was a dark and stormy night\n".chomp
' hello'.lstrip

'Bill:Shakespeare:Playwright:Globe'.split( ':' )

@author.each_byte { |b| puts b }

@content.each_line { |line| puts line }

# Ruby strings are mutable:
first_name = first_name.upcase
# Instead of this:
first_name.upcase!

class Person
  attr_accessor :salary  # A method call
  attr_reader :name      # Another method call
  attr_writer :password  # And another
end
````


Why dynamic languages are more efficient
========================================
   The problem lies with BaseDocument: It does nothing. Even worse, BaseDocument takes more than 30 lines to do nothing. BaseDocument only exists as a misguided effort to provide a common interface for the various flavors of documents. The effort is misguided because Ruby does not judge an object by its class hierarchy.

There are two lessons you can take away from our BaseDocument excursion. The first is that the real compactness payoff of dynamic typing comes not from leaving out a few int and string declarations; it comes instead from all of the BaseDocument style abstract classes that you never write, from the interfaces that you never create, from the casts and derived types that are simply irrelevant. The second lesson is that the payoff is not automatic. If you continue to write static type style base classes, your code will continue to be much bulkier than it might be.

````ruby
def is_longer_than?( n )
  @content.length > n
end
````

Even without type declarations, most coders would have no trouble deducing that is_longer_than? takes a number and returns a boolean. Unfortunately, when type declarations are required, you need put them in your code whether they make your code more readable or not—that’s why they call it required.

First, don’t create more infrastruc- ture than you really need. Keep in mind that Ruby classes don’t need to be related by inheritance to share a common interface; they only need to support the same meth- ods. Don’t obscure your code with pointless checks to see whether this really is an instance of that.

how rspec should works
======================

`x.should == 42`

Let’s pass over the details of what the should method does and focus on the == operator. RSpec has turned the meaning of the == operator completely inside out. The garden variety == method is there to answer a question—“Are these two things equal?” But the RSpec version of == is more like an enforcer: If the two values are equal, noth- ing much happens; but if they are not equal, the test fails. The cool thing about RSpec is that its authors managed to find an alternative meaning for == that not only works in Ruby but also in the squishy computer between your ears. Quite a trick, but not one that is easy to repeat.

Singleton methods in ruby
=========================

We will also see how class methods are actually just singleton methods by another name.
In Ruby, a singleton method is a method that is defined for exactly one object instance
Let me also hasten to add that the term “singleton” as it is used here has nothing to do with the Singleton Pattern of design patterns fame. It’s just an unfortunate collision of terminology. 

````ruby
hand_built_stub_printer = Object.new
def hand_built_stub_printer.available?
  true
end
def hand_built_stub_printer.render( content )
  nil 
end

hand_built_stub_printer = Object.new
class << hand_built_stub_printer
  def available?
    true
  end
  def render
    nil
  end
end
````

Singleton classes are also known as metaclasses or eigenclasses. Although terminology is mostly in the ear of the beholder, I like the more descriptive and less pretentious “'singleton.”

You can even get hold of the singleton class, like this:

````ruby
singleton_class = class << hand_built_stub_printer
  self
end
````

If you think about it, this all makes sense: Any given class, say, Document, is an instance of Class. This means that it inherits all kinds of methods from Class, methods like name and superclass. When we want to add a class method, we want that new method to exist only on the one class (Document in the example), not on all classes. Since the object that goes by the name of Document is an instance of Class, we need to create a method that exists only on the one object (Document) and not on any of the other instances of the same class. What we need is a singleton method.

````ruby
def Document.explain
  puts "self is #{self}"
  puts "and its class is #{self.class}"
end

class Document
  class << self
    def find_by_name( name )
      # Find a document by name...
    end

    def find_by_id( doc_id )
      # Find a document by id
    end 
  end
end
````

Class Variables
===============

````ruby
class Document
  @@default_paper_size = :a4
  def self.default_paper_size
    @@default_paper_size
  end

  def self.default_paper_size=(new_size)
    @@default_paper_size = new_size
  end

  def initialize(title, author, content)
    @paper_size = @@default_paper_size
  end
  # Rest of the class omitted...
end
````

Here’s where the story gets interesting: If the class variable is not defined in the current class, Ruby will go looking up the inheritance tree for it. So if there is no @@default_paper_size in the current class, Ruby will look for one in the superclass and then the super superclass, until it either finds a @@default_paper_size defined on one of those classes or runs out of classes.

The Document class needs to be loaded before Resume and Presentation—it’s the super- class after all. This means the Document class @@default_font will get set first, which means that whenever either of the subclasses goes looking for @@default_font, it will find the one in Document. Remember, Ruby looks up the inheritance tree first. So your seemingly low-impact change to the Document class has changed the behavior of both subclasses. Instead of two separate default font variables, one attached to Presentation and the other to Resume, there is now only one variable, one that lives up in the Document class.

````ruby
class Document
  @default_font = :times
  def self.default_font=(font)
    @default_font = font
  end

  def self.default_font
    @default_font
  end
# Rest of the class omitted...
end
````

even nicer:

````ruby
class Document
  @default_font = :times
  class << self
    attr_accessor :default_font
  end
  # Rest of the class omitted...
end
````

Modules
========

So when should you create a name space module and when should you let your classes go naked? An easy rule of thumb is that if you find yourself creating a lot of names that all start with the same word, perhaps TonsOTonerPrintQueue and TonsOTonerAdministration, then you just may need a TonsOToner module.

A Home for Those Utility Methods:

````ruby
module WordProcessor
  def self.points_to_inches( points )
    points / 72.0
  end

  def self.inches_to_points( inches )
    inches * 72.0
  end
  # Rest of the module omitted
end

an_inch_full_of_points = WordProcessor.inches_to_points(1.0)
````

You might, for instance, have a module full of methods that know how to locate documents:

````ruby
module Finders
  def find_by_name( name )
    # Find a document by name...
  end

  def find_by_id( doc_id )
    # Find a document by id
  end 
end
````

You would like to get the Finders methods into your Document class as class methods. One way to get this done is to do this:

````ruby
class Document
  # Most of the class omitted...
  class << self
    include Finders
  end
end
````

This code includes the module into the singleton class of Document, effectively making the methods of Finders singleton—and therefore class—methods of Document:
````ruby
    war_and_peace = Document.find_by_name( 'War And Peace' )
````

Including modules into the singleton class is a common enough task that Ruby has a special shortcut for it in the form of extend:
````ruby
class Document
  extend Finders
  # Most of the class omitted...
end
````

Run this code and you will end up with class-level Document.find_by_name and find_by_id methods.

Blocks
======

When you tack a block onto the end of a method call, Ruby will package up the block as sort of a secret argument and (behind the scenes) passes this secret argument to the method. Inside the method you can detect whether your caller has actually passed in a block with the block_given? method and fire off the block (if there is one) with yield:

````ruby
def do_something
  yield if block_given?
end
````

This simple “bury the details in a method that takes a block” technique goes by the name of execute around. 

````ruby
class SomeApplication
  def do_something
    with_logging('load') { @doc = Document.load( 'resume.txt' ) }
    # Do something with the document...
    with_logging('save') { @doc.save } end
    
    # Rest of the class omitted...
  def with_logging(description)
    begin
      @logger.debug( "Starting #{description}" )
      yield
      @logger.debug( "Completed #{description}" )
    rescue
      @logger.error( "#{description} failed!!")
      raise
    end 
  end
end
````

All of the variables that are visible just before the opening do or { are still visible inside the code block. Code blocks drag along the scope in which they were created wherever they go. In the last example, this means that @doc object is automatically visible inside the code block—no need to pass it down as an argument.(The technical term for objects with this scope dragging property is closure, and some Ruby pro- grammers will use the terms closure and block interchangeably.)

This doesn’t mean that respectable execute around methods don’t take any argu- ments. A good rule of thumb is that the only arguments you should pass from the
application into an execute around method are those that the execute around method itself, not the block, will use. We can see this in our with_logging method. We passed in strings like 'Document load' and 'Document save' to the with_logging method, strings used by the method itself.
Similarly, there is nothing wrong with the execute around method passing argu- ments that originate in the method itself into the block; in fact, many execute around methods do exactly that. For example, imagine that you need a method that opens a database connection, does something with it, and then ensures that the connection gets closed. You might come up with something like this:

````ruby
def with_database_connection( connection_info )
  connection = Database.new( connection_info )
  begin
    yield( connection ) ensure
    connection.close
  end
end
````

Note that the with_database_connection method creates the new database connec- tion and then passes it into the block.

Explicit Blocks
---------------

````ruby
def run_that_block( &that_block )
  puts "About to run the block"
  that_block.call
  puts "Done running the block"
end

that_block.call if that_block

````
Event listners
--------------

````ruby
class Document
  attr_accessor :load_listener
  attr_accessor :save_listener
  # Most of the class omitted...
  def load( path )
    @content = File.read( path )
    load_listener.on_load( self, path ) if load_listener
  end

  def save( path )
    File.open( path, 'w') { |f| f.print( @contents ) }
    save_listener.on_save( self, path ) if save_listener
  end 
end
````

or with blocks:

````ruby
class Document
  # Most of the class omitted...
  def on_save( &block )
    @save_listener = block
  end
  def on_load( &block )
    @load_listener = block
  end
  def load( path )
    @content = File.read( path )
    @load_listener.call( self, path ) if @load_listener
  end
  
  def save( path )
    File.open( path, 'w' ) { |f| f.print( @contents ) }
    @save_listener.call( self, path ) if @save_listener
  end 
end

````

Lazy initialization:

````ruby
class BlockBasedArchivalDocument
  attr_reader :title, :author
  def initialize(title, author, &block) @title = title
    @author = author
    @initializer_block = block
  end

  def content
    if @initializer_block
      @content = @initializer_block.call
      @initializer_block = nil
    end
    @content 
  end
end

````

When it comes to creating Proc objects, beware of false friends. Although calling Proc.new is nearly synonymous with lambda:
````ruby
    from_proc_new = Proc.new { puts "hello from a block" }
````
It’s not quite synonymous enough......
Lesson one is that if you are calling a method that takes a block, pause for a second before you put a return, next, or break in that block. Does it make sense here? Lesson two is that if you want a block object that behaves like the ones that Ruby generates when you pass a couple of braces into a method, use Proc.new. If you want something that will behave more like a reg- ular object with a single method, use lambda.2

scoping variable:

````ruby
def some_method(doc)
  big_array = Array.new(10000000)
  # Do something with big_array... # And now get rid of it! big_array = nil
  doc.on_load do |d|
    puts "Hey, I've been loaded!"
  end 
end
````


In the Wild: it...do end in RSpec, before_filter do | controller |, task :list_home, :role => 'production' do...

Meta
====

def self.inherited(subclass)
  DocumentReader.reader_classes << subclass
end

def self.inherited(subclass)
  DocumentReader.reader_classes << subclass
end

at_exit do
  puts "Have a nice day."
end

method_missing,
method_added
proc_object = proc do |event, file, line, id, binding, klass|
  puts "#{event} in #{file}/#{line} #{id} #{klass}"
end
set_trace_func(proc_object)
require 'date'

def const_missing(const_name)
def method_missing( missing, *args )


Rake uses const_missing to provide helpful warnings about depre- cated names. It seems that earlier versions of Rake defined the core Rake classes at the top level, without any encapsulating modules. In those bygone days the class of a Rake task was simply Task, with no module. 

Deligate:

def method_missing(name, *args)
  check_for_expiration
  @original_document.send(name, *args)
end

require 'delegate'
class DocumentWrapper < SimpleDelegator
  def initialize( real_doc )
    super( real_doc )
  end
end

That’s pretty much it. With just seven lines of code, we have a fully functional wrapper for any document.

Method for replace_*:

def method_missing( name, *args )
    string_name = name.to_s
    return super unless string_name =~ /^replace_\w+/
    old_word = extract_old_word(string_name)
    replace_word( old_word, args.first )
end

You also need to be aware of the likelihood that using method_missing will muck up the respond_to? method. 

def respond_to?(name)
  string_name = name.to_s
  return true if string_name =~ /^replace_\w+/
  super
end

Aside from letting you easily give a method several different names, alias_ method comes in handy when you are messing with the innards of an existing class. Here’s a version of our String monkey patch that uses alias_method to avoid repro- ducing the logic of the original + method:
class String
  alias_method :old_addition, :+
  def +( other )
    if other.kind_of? Document
      new_content = self + other.content
      return Document.new(other.title, other.author, new_content)
    end
    old_addition(other)
  end 
end

Although the name suggests otherwise, alias_method actually copies a method implementation, giving it a new name along the way. 

In the same way that we aliased an existing method, we can make a public method private:
class Document
  private :word_count
end
Or a private method public again:
class Document
  public :word_count
end
We can even get rid of it all together:
class Document
  remove_method :word_count
end

if RUBY_VERSION >= '1.9'
    def char_at( index )
      @content[ index ]
    end
  else
    def char_at( index )
      @content[ index ].chr
    end
end

class Document
  def self.reload
    remove_instance_methods
    load( __FILE__ )
  end
  def self.remove_instance_methods
    instance_methods(false).each do |method|
      remove_method(method)
    end
  end
  # Rest of the class omitted...
end

--------------

class StructuredDocument
  attr_accessor :title, :author, :paragraphs
  def initialize( title, author )
    @title = title
    @author = author
    @paragraphs = []
    yield( self ) if block_given?
  end
...
end

russ_cv = Resume.new( 'russ', 'resume') do |cv|
  cv.name( 'Russ Olsen' )
  cv.address( '1313 Mocking Bird Lane' )
  cv.email( 'russ@russolsen.com' )
  # Etc... 
end

class Object
  def self.simple_attr_reader(name)
    code = "def #{name}; @#{name}; end"
    class_eval( code )
  end
end

class Object
  def self.simple_attr_writer(name)
    method_name = "#{name}="
    define_method( method_name ) do |value|
      variable_name = "@#{name}"
      instance_variable_set( variable_name, value )
    end
  end 
end

