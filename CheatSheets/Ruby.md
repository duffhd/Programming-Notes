This file was originally forked from https://github.com/ThibaultJanBeyer/cheatsheets

# Ruby Cheatsheet

##### Table of Contents

- [Basics](#basics)
- [Statements](#statements)
- [Data Structures](#data-structures)
- [Variables](#variables)  
- [Methods](#methods)
- [Classes](#classes)
- [Modules](#modules)
- [Blocks & Procs](#blocks--procs)  
- [Lambdas](#lambdas)
- [Calculation](#calculation)  
- [Comments](#comments)  
- [Conditions](#conditions)  
- [Printing & Putting](#printing--putting)  
- [User Input](#user-input)  
- [Loops](#loops)
- [Sorting & Comparing](#sorting--comparing)  
- [Error Handling](#error-handling)
- [Useful Methods](#useful-methods)

## Misc

- Everything in ruby is a method, even if it looks like a function
- `$ irb`: is used as an interactive ruby programming shell on the terminal

## Variables

```Ruby
my_variable = “Hello”
my_variable ||= "Hi" # Only create the variable if it was not set before.
```

### Constants

```Ruby
MY_CONSTANT = # something
```

### Symbols

```Ruby
:symbol # symbol is like an ID in html. :symbol != "symbol"
# Symbols are often used as Hash keys or referencing method names.
# They can not be changed once created. They save memory (only one copy at a given time). Faster.
:test.to_s # converts to "test"
"test".to_sym # converts to :test
"test".intern # :test
# Symbols can be used like this as well:
my_hash = { key: "value", key2: "value" } # is equal to { :key => "value", :key2 => "value" }
```

## Statements

Some languages use semicolons or just indentation to define statements, in ruby we use blocks with `end`

```Ruby
# ON variables we don't need to use end blocks
name = 'alice'

# Methods example
def method
  # Your code goes here
end

# One line statement
class DerivedClass < BaseClass; end 
```

## Data Structures

### Arrays

```Ruby
my_array = ["a", "b", "c", "d", "e"]
my_array[0] # "a" array's indexes always start at 0

# Access arrays with index ranges
my_array[0..2] # "a", "b", "c"

# Using negative numbers to access the last elements of the array
my_array[-1] # "e"
my_array[2..-1] # "c", "d", "e"

# Array of array's, matrix
multi_d = [[0,1],[0,1]] 
multi_d[0][0] # Accessing values

# Pushing new values to the array
[1, 2, 3] << 4 # [1, 2, 3, 4]
[1, 2, 3].push(4) # Same as above
```

### Hashes

`Key => value` pair

```Ruby
hash = { "key1" => "value1", "key2" => "value2" }

# The same hash using symbols instead of strings
hash = { key1: "value1", key2: "value2" } 

my_hash = Hash.new("default value")
hash.select{ |key, value| value > 3 } # selects all keys in hash that have a value greater than 3
hash.each_key { |k| print k, " " } # ==> key1 key2
hash.each_value { |v| print v } # ==> value1value2

my_hash.each_value { |v| print v, " " }
# ==> 1 2 3
```

#### Useful methods

```Ruby
"one, two".split(“,”) # split according to the parameter and return a list o the remaining elements ['one', 'two']

int_array = [1, 2, 3, 4]
int_array.join # Join the int array to a string '1234'
int_array.shuffle # Randomly changes the order of the values inside the array
```

## Methods

```Ruby
def greeting(hello, *names) # *names is a split argument, takes several parameters passed in an array 
  return "#{hello}, #{names}"
end

start = greeting("Hi", "Justin", "Maria", "Herbert") # call a method by name

def name(variable=default)
  ### The last line in here gets returned by default
end
```

## Classes

```Ruby
class Person
  # @@ denotes a class variable that is always initialized when the class is created
  @@count = 0

  # Attributes are used so that you can access (get and set) the instance variables outside
  # this class, they can be put under 'private' or 'protected' too.
  attr_reader :name # make it readable
  attr_writer :name # make it writable
  attr_accessor :name # makes it readable and writable

  # Initialize function is used to take the parameters when the class is being created
  def initialize(name)
    # @ denotes a class instance variable that is initialized with a value that comes from the initialize parameters
    @name = name
    @@count += 1
  end

  # Class methods
  def self.get_counts
    return @@count
  end

  private

  # Private methods go here
  def private_method; end 

  protected

  # Protected methods go here
  def protected_method; end
end

matz = Person.new("Yukihiro")
matz.show_name # Yukihiro
```

### Inheritance

```Ruby
class DerivedClass < Base
  def some_method
    # When you call super from inside a method, that tells Ruby to look in the superclass of the current class 
    # and find a method with the same name as the one from which super is called.           
    # If it finds it, Ruby will use the superclass' version of the method.
    super(optional args) 
      # Some stuff
    end
  end
end

# Any given Ruby class can have only one superclass. 
# Use mixins if you want to incorporate data or behavior from several classes into a single class.
```

## Modules

```Ruby
module ModuleName # module names are rather written in PascalCase
  # variables in modules doesn't make much sence since modules do not change. Use constants.
end

Math::PI # using PI constant from Math module. Double colon = scope resolution operator = tells Ruby where you're looking for a specific bit of code.

require 'date' # to use external modules.
puts Date.today # 2016-03-18

module Action
  def jump
    @distance = rand(4) + 2
    puts "I jumped forward #{@distance} feet!"
  end
end

class Rabbit
  include Action # Any class that includes a certain module can use those module's methods! This now is called a Mixin.
  extend Action # extend keyword mixes a module's methods at the class level. This means that class itself can use the methods, as opposed to instances of the class.
  attr_reader :name
  def initialize(name)
    @name = name
  end
end

peter = Rabbit.new("Peter")
peter.jump # include
Rabbit.jump # extend
```

## Blocks & Procs

### Code Blocks

_Blocks are not objects._ A block is just a bit of code between do..end or {}. It's not an object on its own, but it can be passed to methods like .each or .select.

```Ruby
def yield_name(name)
  yield("Kim") # print "My name is Kim. "
  yield(name) # print "My name is Eric. "
end

yield_name("Eric") { |n| print "My name is #{n}. " } # My name is Kim. My name is Eric.
yield_name("Peter") { |n| print "My name is #{n}. " } # My name is Kim. My name is Eric. My name is Kim. My name is Peter.
```

### Proc

_Saves blocks and are objects._ A proc is a saved block we can use over and over.

```Ruby
cube = Proc.new { |x| x ** 3 }
[1, 2, 3].collect!(&cube) # [1, 8, 27] # the & is needed to transform the Proc to a block.
cube.call # call procs directly
```

## Lambdas

```Ruby
lambda { |param| block }
multiply = lambda { |x| x * 3 }
y = [1, 2].collect(&multiply) # 3 , 6
```

Diff between procs and lambdas:

- a lambda checks the number of arguments passed to it, while a proc does not (This means that a lambda will throw an error if you pass it the wrong number of arguments, whereas a proc will ignore unexpected arguments and assign nil to any that are missing.)
- when a lambda returns, it passes control back to the calling method; when a proc returns, it does so immediately, without going back to the calling method.
  So: A lambda is just like a proc, only it cares about the number of arguments it gets and it returns to its calling method rather than returning immediately.

## Calculation

- Addition (+)
- Subtraction (-)
- Multiplication (\*)
- Division (/)
- Exponentiation (\*\*)
- Modulo (%)
- The concatenation operator (<<)
- you can do 1 += 1 –– which gives you 2 but 1++ and 1-- does not exist in ruby
- `"A " << "B"` => `"A B"` but `"A " + "B"` would work as well but `"A " + 4 + " B"` not. So rather use string interpolation (`#{4}`)
- `"A #{4} B"` => `"A 4 B"`

## Comments

```Ruby
=begin
Multiline comment
Needs a begin and end block.
=end
```

```Ruby
# inline comment
```

## Conditions

### If

```Ruby
if 1 < 2
puts “one smaller than two”
elsif 1 > 2 # *careful not to mistake with else if. In ruby you write elsif*
puts “elsif”
else
puts “false”
end

# Inline if statements
puts "be printed" if true
puts 3 > 4 ? "if true" : "else" # Ternary operator, else will be putted
```

### Unless

```Ruby
# Unless is the opposite from if, below it checks if true
unless false
  puts “I’m here”
else
  puts “not here”
end

# Inline
puts "not printed" unless true
```

### Case

```Ruby
number = 5

case number
when 0
    puts 'zero'
when 1...5
    puts 'between 1 and 4'
else
    puts '5 or higher'
end

# Inline when cases
case number
  when 0 then puts 'zero'
  when 1..5 then puts 'between 1 and 4'
  else puts '5 or higher'
end
```

- `&&`: and
- `||`: or
- `!`: not
- `(x && (y || w)) && z`: use parenthesis to combine arguments
- problem = false
- print "Good to go!" unless problem –– prints out because problem != true

## Printing & Putting

```Ruby
print "bla" # Print the comment
puts "test" # puts the text in a separate line
```

## String Methods

```Ruby
"Hello".length # 5
"Hello".reverse # “olleH”
"Hello".upcase # “HELLO”
"Hello".downcase # “hello”
"hello".capitalize # “Hello”
"Hello".include? "i" # equals to false because there is no i in Hello
"Hello".gsub!(/e/, "o") # Hollo
"1".to_i # transform string to integer –– 1
"test".to_sym # converts to :test
"test".intern # :test
:test.to_s # converts to "test"
```

## User Input

```Ruby
gets # Get the user input
gets.chomp # Remove extra lines from the input, this is the most common way to get input
```

## Loops

### While loop

```Ruby
i = 1
while i < 11
  puts i
  i = i + 1
end
```

### Until loop

```Ruby
i = 0
until i == 6
  puts i
  i += 1
end
```

### For loop

```Ruby
for i in 1...10 # ... tells ruby to exclude the last number (here 10 if we .. only then it includes the last num)
  puts i
end
```

### Loop iterator

```Ruby
i = 0
loop do
  i += 1
  print "I'm currently number #{i}” # a way to have ruby code in a string
  break if i > 5
end
```

### Next

```Ruby
for i in 1..5
  next if i % 2 == 0 # If the remainder of i / 2 is zero, we go to the next iteration of the loop.
  print i
end
```

### .each

```Ruby
things.each do |item| # for each things in things do something while storing that things in the variable item
  print “#{item}"
end
```

on hashes like so:

```Ruby
hashes.each do |x,y|
  print "#{x}: #{y}"
end
```

### .times

```Ruby
10.times do
  print “this text will appear 10 times”
end
```

### .upto / .downto

```Ruby
10.upto(15) { |x| print x, " " } # 10 11 12 13 14 15
"a".upto("c") { |x| print x, " " } # a b c
```

## Sorting & Comparing

```Ruby
array = [5,4,1,3,2]
array.sort! # = [1,2,3,4,5] – works with text and other as well.
"b" <=> "a" # = 1 – combined comparison operator. Returns 0 if first = second, 1 if first > second, -1 if first < second
array.sort! { |a, b| b <=> a } # to sort from Z to A instead of A to Z
```

## Error handling

```Ruby
begin
  Interger("A") # Trying to parse a character to int will fail
# Rescue will catch the error so you can handle it
rescue ArgumentError
  puts 'Input only numbers'
end
```

For the most part the `begin` keyword is ommited leaving only:

```Ruby
Interger("A")
rescue ArgumentError
  puts 'Input only numbers'
end
```

### Raise

You can raise your own errors

```Ruby
raise 'Error'

rescue 
  puts 'Captured a raised error'
```

### Ensure

The ensure block will always be executed no matter the error that occured

```Ruby
rescue ArgumentError
  # Handle the error
ensure
  puts 'This block always get executed'
```

## Useful Methods

```Ruby
1.is_a? Integer # returns true
:1.is_a? Symbol # returns true
"1".is_a? String # returns true
[1,2,3].collect!() # does something to every element (overwrites original with ! mark)
.map() # is the same as .collect
1.2.floor # 1 # rounds a float (a number with a decimal) down to the nearest integer.
cube.call # implying that cube is a proc, call calls procs directly
Time.now # displays the actual time
```

## Conventions

- Classes and Modules are written in PascalCase (UpperCamelCase)
- Methods ending with `?` denote that they return a boolean value
- Methods ending with `!` denotes a potentially dangerous method that can lead to errors
