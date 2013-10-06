There are good reasons for adding comments to your code, the best being to explain how to use your software masterpiece. These kinds of “how to” comments should focus on exactly that: how to use the thing. Don’t explain why you wrote it, the algorithm that it uses, or how you got it to run faster than fast. Just tell me how to use the thing and remember that examples are always welcome.

That voice you hear in your head, the one whispering that you need to add some comments, may just be your program crying out to be rewrit- ten.

For constants: all uppercase, punctuated by underscores:
      ANTLERS_PER_MALE_MOOSE = 2

Ruby programmers do tend to dispense with the parentheses when calling a method that is familiar, stands alone on its own line, and whose arguments are few in number.

If your block consists of a single statement, fold the whole statement into a single line and delimit the block with braces. Alternatively, if you have a multi- statement block, spread the block out over a number of lines, and use the do/end form.

The same way we use if/unless we can use while/until.

author = case title
         when 'War And Peace' then 'Tolstoy'
         when 'Romeo And Juliet' then  'Shakespeare'
         else "Don't know"
         end

A key thing to keep in mind about all of these case statements is that they use the ===2 operator to do the comparisons.

puts 'Sorry Dennis Ritchie, but 0 is true!' if 0

Ruby’s treatment of booleans means that there are two things that are false and an
infinite number of things that are true. Thus you should avoid testing for truth by testing for specific values.

if flag == true
  # do something
  # Is not just overly wordy, it is an invitation to disaster.
end

@first_name ||= ''
@first_name = @first_name || ''

Finally, be aware that this use of ||= suffers from exactly the kind of nil/false confusion that I warned you about earlier. If @first_name happened to start out as false, the code would cheerfully go ahead and reset it to the empty string. Moral of the story: Don’t try to use ||= to initialize things to booleans.

poem_words = %w{ twinkle little star how I wonder }

So that we can load a font like this:
    load_font( { :name => 'times roman', :size => 12 })
The bonus comes if the hash is at the end4 of the argument list—as it is in the load_font example. In that case we can leave the braces off when we call the method. Given this, we can shorten our call to load_font down to:
    load_font( :name => 'times roman', :size => 12 )

def index_for( word )
  words.find_index { |this_word| word == this_word }
end

doc.words.map { |word| word.size }

total = words.inject(0.0){ |result, word| word.size + result}

Ruby arrays and hashes have one thing in common that takes many programmers by surprise: They are both ordered. 

array = []
array[24601] = "Jean Valjean"
Since arrays are continuous, the second line above instantly created 24,602 new ele- ments of array, most of them set to nil.

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

Ruby strings are mutable. Since this aspect of the language takes many newcomers by surprise, let me say it again: Ruby strings are mutable.

You should treat Ruby strings like any other mutable data structure. Get in the habit of saying this:
    first_name = first_name.upcase
Instead of this:
    first_name.upcase!

\d will match any digit, so that \d\d will match any two digit number from 00 to 99.
\w, where the w stands for “word character,” will match any letter, number or the underscore.
\s will match any white space character including the vanilla space, the tab, and the newline.

puts /\d\d:\d\d (AM|PM)/ =~ '10:24 PM'
0, 6, nil

puts "It matches!" if /AM/i =~ 'am'

puts "Found it" if content =~ /^Once upon a time$/

In multi line strings the .* won’t match across the lines . . . unless we simply turn off this behavior by adding an m to our expression:

    /^Once upon a time.*happily ever after\.$/m

there can only ever be one instance of any given symbol: If I mention :all twice in my code, it is always exactly the same :all. So if I have:
a = :all b= a
c = :all
I know that a, b, and c all refer to exactly the same object. 

a == c
a === c
a.eql?(c)
a.equal?(c)

Another aspect of symbols that makes them so well suited to their chosen career is that symbols are immutable

the_string = :all.to_s
the_symbol = 'all'.to_sym

Ruby’s treatment of private methods is a bit idiosyncratic. The rule is that you cannot call a private method with an explicit object reference. Note that in Ruby, private methods are callable from subclasses. Think about it: You don’t need an explicit object reference to call a superclass method from a subclass.

 n = doc.send( :word_count )

The rules for protected methods are looser and a bit more complex: Any instance of a class can call a protected method on any other instance of the class. Thus, if we made word_count protected, any instance of Document could call word_count on any other instance of Document, including instances of subclasses like RomanceNovel.


class Person
  attr_accessor :salary  # A method call
  attr_reader :name      # Another method call
  attr_writer :password  # And another
end

 The problem lies with BaseDocument: It does nothing. Even worse, BaseDocument takes more than 30 lines to do nothing. BaseDocument only exists as a misguided effort to provide a common interface for the various flavors of documents. The effort is misguided because Ruby does not judge an object by its class hierarchy.

class Document
  # Body of the class unchanged...
end
class LazyDocument
  # Body of the class unchanged...
end

There are two lessons you can take away from our BaseDocument excursion. The first is that the real compactness payoff of dynamic typing comes not from leaving out a few int and string declarations; it comes instead from all of the BaseDocument style abstract classes that you never write, from the interfaces that you never create, from the casts and derived types that are simply irrelevant. The second lesson is that the payoff is not automatic. If you continue to write static type style base classes, your code will continue to be much bulkier than it might be.

The editorial department would like you to change the Document class so that they can use Title and Author instances instead of strings as the @title and @author values in Document instances, like this:

two_cities = Title.new( 'A Tale Of Two Cities',
                        '2 Cities', '0-999-99999-9' )
dickens = Author.new( 'Charles', 'Dickens' )
doc = Document.new( two_cities, dickens, 'It was the best...' )

Being a nice person and a consummate professional you immediately agree to undertake this task. And then you do nothing. Absolutely nothing. You do nothing because the Document class already works with Title and Author instances. There are no interfaces to extract, no declarations to change, no class hierarchies to adjust, noth- ing. It just works.
It works because Ruby’s dynamic typing means that you don’t declare the classes of variables and parameters. That means that your classes are not frozen together in a rigid network of type relationships. In Ruby, any two classes that can work together will work together. 

Programmers new to Ruby will sometimes try to cope with the loss of static typing by adding type-checking code to their methods:

def initialize( title, author, content )
  raise "title isn't a String" unless title.kind_of? String
  raise "author isn't a String" unless author.kind_of? String
  raise "content isn't a String" unless content.kind_of? String
  @title = title
  @author = author
  @content = content
end

This kind of pseudo-static type checking combines all the disadvantages of the two camps: It destroys the wonderful loose coupling of dynamic typing. It also bloats the code while doing little to improve reliability. Don’t do this.

Considerations of code flexibility and compactness aside, there is simply no arguing with the fact that a few type declarations:

# Pseudo-Ruby! Don't try this at home!
def initialize( String title, String author, String content )

Would make it easier to figure out how to create a Document instance. The flip side of this argument is that not all methods benefit—in a documentation sense—from type declarations. Take this hypothetical Document method:

def is_longer_than?( n )
  @content.length > n
end

Even without type declarations, most coders would have no trouble deducing that is_longer_than? takes a number and returns a boolean. Unfortunately, when type declarations are required, you need put them in your code whether they make your code more readable or not—that’s why they call it required.

First, don’t create more infrastruc- ture than you really need. Keep in mind that Ruby classes don’t need to be related by inheritance to share a common interface; they only need to support the same meth- ods. Don’t obscure your code with pointless checks to see whether this really is an instance of that.

So when you say this:
    sum = first + second
What you are really saying is:
    sum = first.+(second)

class Document
  # Most of the class omitted...
  def +(other)
    Document.new( title, author, "#{content} #{other.content}" )
  end
end

Although technically these bracketed methods are not operators, the Ruby parser sprinkles some very operator-like syntac- tic sugar on them: When you say foo[4] you are really calling the [] method on foo, passing in four as an argument. Similarly, when you say foo[4] = 99, you are actu- ally calling the []= method on foo, passing in four and ninety-nine.

 x.should == 42
Let’s pass over the details of what the should method does and focus on the == operator. RSpec has turned the meaning of the == operator completely inside out. The garden variety == method is there to answer a question—“Are these two things equal?” But the RSpec version of == is more like an enforcer: If the two values are equal, noth- ing much happens; but if they are not equal, the test fails. The cool thing about RSpec is that its authors managed to find an alternative meaning for == that not only works in Ruby but also in the squishy computer between your ears. Quite a trick, but not one that is easy to repeat.

It turns out that Ruby’s Object class defines no less than four equality methods. There is eql? and equal? as well as == (that’s two equal signs), not to mention === (that’s three equal signs).

def ==(other)
  return true if other.equal?(self)
  return false unless other.instance_of?(self.class)
  folder == other.folder && name == other.name
end 

The problem is that we have defined equality relationships that violate the principal of symmetry: People tend to expect that ifa == bthenb == a.

In our efforts to do something sensible we have managed to create a situation where(a == b)and(b == c)but(a != c).

The other way out is to ask yourself whether you really need to use the == opera- tor for everything. It is easy enough to add your own specialized comparison method to VersionedIdentifier:

def is_same_document?(other)
  other.folder == folder && other.name == name
end

def hash
    folder.hash ^ name.hash
end

def eql?(other)
  return false unless other.instance_of?(self.class)
  folder == other.folder && name == other.name
end

Ruby has a fairly fine- grained model of equality—we have the equal? method, strictly for object identity. We have the everyday equality method, ==, and we also have the === method, which comes out mostly for case statements. We also have eql?, and its friend hash, to cope with hash tables. 

By default, === calls the double equals method, so unless you specifically override ===, wherever you send ==, === is sure to follow. It’s probably a good idea to leave === alone unless doing so results in really ugly case statements. You can do so for case statements.

We will also see how class methods are actually just singleton methods by another name.
In Ruby, a singleton method is a method that is defined for exactly one object instance
Let me also hasten to add that the term “singleton” as it is used here has nothing to do with the Singleton Pattern of design patterns fame. It’s just an unfortunate collision of terminology. 

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

Singleton classes are also known as metaclasses or eigenclasses. Although terminology is mostly in the ear of the beholder, I like the more descriptive and less pretentious “'singleton.”

You can even get hold of the singleton class, like this:

singleton_class = class << hand_built_stub_printer
  self
end

If you think about it, this all makes sense: Any given class, say, Document, is an instance of Class. This means that it inher- its all kinds of methods from Class, methods like name and superclass. When we want to add a class method, we want that new method to exist only on the one class (Document in the example), not on all classes. Since the object that goes by the name of Document is an instance of Class, we need to create a method that exists only on the one object (Document) and not on any of the other instances of the same class. What we need is a singleton method.

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

Here’s where the story gets interesting: If the class variable is not defined in the current class, Ruby will go looking up the inheritance tree for it. So if there is no @@default_paper_size in the current class, Ruby will look for one in the superclass and then the super superclass, until it either finds a @@default_paper_size defined on one of those classes or runs out of classes.

The Document class needs to be loaded before Resume and Presentation—it’s the super- class after all. This means the Document class @@default_font will get set first, which means that whenever either of the subclasses goes looking for @@default_font, it will find the one in Document. Remember, Ruby looks up the inheritance tree first. So your seemingly low-impact change to the Document class has changed the behavior of both subclasses. Instead of two separate default font variables, one attached to Presentation and the other to Resume, there is now only one variable, one that lives up in the Document class.

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

even nicer:

class Document
  @default_font = :times
  class << self
    attr_accessor :default_font
  end
  # Rest of the class omitted...
end

So when should you create a name space module and when should you let your classes go naked? An easy rule of thumb is that if you find yourself creating a lot of names that all start with the same word, perhaps TonsOTonerPrintQueue and TonsOTonerAdministration, then you just may need a TonsOToner module.

A Home for Those Utility Methods:

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

You might, for instance, have a module full of methods that know how to locate documents:

module Finders
  def find_by_name( name )
    # Find a document by name...
  end

  def find_by_id( doc_id )
    # Find a document by id
  end 
end

You would like to get the Finders methods into your Document class as class methods. One way to get this done is to do this:

class Document
  # Most of the class omitted...
  class << self
    include Finders
  end
end

This code includes the module into the singleton class of Document, effectively making the methods of Finders singleton—and therefore class—methods of Document:
    war_and_peace = Document.find_by_name( 'War And Peace' )
Including modules into the singleton class is a common enough task that Ruby has a special shortcut for it in the form of extend:
class Document
  extend Finders
  # Most of the class omitted...
end

Run this code and you will end up with class-level Document.find_by_name and find_by_id methods.


When you tack a block onto the end of a method call, Ruby will package up the block as sort of a secret argument and (behind the scenes) passes this secret argument to the method. Inside the method you can detect whether your caller has actually passed in a block with the block_given? method and fire off the block (if there is one) with yield:

def do_something
  yield if block_given?
end

class Document
  # Most of the class omitted...
  def each_word_pair
    word_array = words
    index = 0
    while index < (word_array.size-1)
      yield word_array[index], word_array[index+1]
    index += 1 end
  end 
end

doc = Document.new('Donuts', '?', 'I love donuts mmmm donuts' )
doc.each_word_pair{ |first, second| puts "#{first} #{second}" }

The Enumerable module is a mixin that endows classes with all sorts of interesting collection-related methods. Here’s how Enumerable works: First, you make sure that your class has an each method, and then you include the Enumerable module in your class.

doc.include?("make")
to_a
each_cons

Finally, if the elements in your collection define the <=> operator, you can use the Enumerable-supplied sort method.


This simple “bury the details in a method that takes a block” technique goes by the name of execute around. 

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

All of the variables that are visible just before the opening do or { are still visible inside the code block. Code blocks drag along the scope in which they were cre- ated wherever they go. In the last example, this means that @doc object is automati- cally visible inside the code block—no need to pass it down as an argument.(The technical term for objects with this scope dragging property is closure, and some Ruby pro- grammers will use the terms closure and block interchangeably.)

This doesn’t mean that respectable execute around methods don’t take any argu- ments. A good rule of thumb is that the only arguments you should pass from the
application into an execute around method are those that the execute around method itself, not the block, will use. We can see this in our with_logging method. We passed in strings like 'Document load' and 'Document save' to the with_logging method, strings used by the method itself.
Similarly, there is nothing wrong with the execute around method passing argu- ments that originate in the method itself into the block; in fact, many execute around methods do exactly that. For example, imagine that you need a method that opens a database connection, does something with it, and then ensures that the connection gets closed. You might come up with something like this:

def with_database_connection( connection_info )
  connection = Database.new( connection_info )
  begin
    yield( connection ) ensure
    connection.close
  end
end

Note that the with_database_connection method creates the new database connec- tion and then passes it into the block.

Explicit Blocks

def run_that_block( &that_block )
  puts "About to run the block"
  that_block.call
  puts "Done running the block"
end

that_block.call if that_block

Event listners:
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


or with blocks:

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


Lazy initialization:

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


When it comes to creating Proc objects, beware of false friends. Although calling Proc.new is nearly synonymous with lambda:
    from_proc_new = Proc.new { puts "hello from a block" }
It’s not quite synonymous enough......
Lesson one is that if you are calling a method that takes a block, pause for a second before you put a return, next, or break in that block. Does it make sense here? Lesson two is that if you want a block object that behaves like the ones that Ruby generates when you pass a couple of braces into a method, use Proc.new. If you want something that will behave more like a reg- ular object with a single method, use lambda.2

scoping variable:

def some_method(doc)
  big_array = Array.new(10000000)
  # Do something with big_array... # And now get rid of it! big_array = nil
  doc.on_load do |d|
    puts "Hey, I've been loaded!"
  end 
end


In the Wild: it...do end in RSpec, before_filter do | controller |, task :list_home, :role => 'production' do...

Meta

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

