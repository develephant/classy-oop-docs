Classes are the template by which you create [instances](/usage/instance/). Classes can also be [subclassed](/usage/subclass/), allowing you to extend (reuse) functionality of an existing class. This concept of extending classes and creating instances is the driving force behind OOP practices.

## Discussion

### Conventions

Generally a class name will be represented as a capitalized string identifier. For example "Person" is a good class identifier.

### Organization

You should generally create a single Lua file for each class or subclass you wish to create, using the class indentifier as the file name. For example, a "Person" class should be stored in a file named __Person.lua__.

How you organize classes is left to personal preference, but a good practice is to keep your classes in a "classes" directory, and `require` them as needed.

For example, your file tree might look like the following:

```
project
  classes/
    Person.lua
  main.lua
```

### Class Creation

A base class is created using the __Classy__ [create](/classy/#create) method. You can optionally add an [constructor](/classy/#constructor) function if you plan on passing arguments to your class when creating instances.

See the [Subclasses](/usage/subclass/) section for details on creating a subclass.

### A Class File

A class file simply consists of the class template, which is then returned at the end of the file.

__Example__

_classes/Person.lua_

```lua
local Classy = require("plugin.classy")

--#########################################################
--# Person Class
--#########################################################
local Person = Classy.create("Person")

function Person:constructor(name)
  self.name = name
end

function Person:getName()
  return self.name
end

return Person
```

You can then `require` this class wherever you need it.

__Example__

_main.lua_

```lua
local Person = require("classes.Person")

--create instance
local person = Person:new("Marla")
--use instance
print(person:getName()) --> Marla
```

See the [Subclasses](/usage/subclass/) section for information on how to set up your subclass files. See the [Instances](/usage/instance/) section for more information on using instances.

## Class Examples

### Basic Class

```lua
local Classy = require("plugin.classy")

--class
local Person = Classy.create("Person")

--#########################################################
--# Instance Examples
local p1 = Person:new()
p1.age = 26

local p2 = Person:new()
p2.age = 45

print(p1.age) --> 26
print(p2.age) --> 45
```

### Class with Defaults

```lua
local Classy = require("plugin.classy")

--class
local Person = Classy.create("Person", { age = 23 })

--#########################################################
--# Instance Examples
local person = Person:new()
print(person.age) --> 23 (default)
```

### Class with Constructor

```lua
local Classy = require("plugin.classy")

--class
local Person = Classy.create("Person")
function Person:constructor(age)
  self.age  = age
end

--#########################################################
--# Instance Examples
local p1 = Person:new(23)
print(p1.age) --> 23
```

### Class with Constructor and Defaults

```lua
local Classy = require("plugin.classy")

--class
local Person = Classy.create("Person", { age = 23 })
function Person:constructor(age)
  self.age  = age
end

--#########################################################
--# Instance Examples
local p1 = Person:new()
print(p1.age) --> 23 (default)

local p2 = Person:new(45)
print(p2.age) --> 45
```

### Class with Methods

```lua
local Classy = require("plugin.classy")

--class
local Person = Classy.create("Person")
function Person:constructor(name)
  self.name = name
end

function Person:getName()
  return self.name
end

--#########################################################
--# Instance Examples
local p1 = Person:new("Tim")
local p2 = Person:new("Jane")

print(p1:getName()) --> Tim
print(p2:getName()) --> Jame
```