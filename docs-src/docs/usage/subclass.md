
Subclasses allow you to "extend" the functionality of another [class](/usage/class/) (or subclass). Once you create a subclass, you gain all the methods and properties of the class you are subclassing and can then build upon it.

This is a powerful concept in OOP which spares you from rewriting functionality, and instead adding only what you need to an existing class.

## Discussion

### Conventions

Generally a subclass name will be represented as a capitalized string identifier. For example "Boss" is a good subclass identifier.

### Subclass Creation

Subclasses are created using the [extends](/classy/#extends) method on the class you want to extend.

### Super Constructor

When creating a subclass, we are inheriting functionality from another class. The class being subclassed is the "parent" or "superclass" of the subclass.

When we need access to the "parent", we use the __super__ property of the subclass. This is used most often when a subclass contains a [constructor](/classy/#constructor) function to pass information up the inheritance chain.

The following example and discussion demonstrates how this works:

```lua
local Classy = require("plugin.classy")

--Person base class
local Person = Classy.create("Person")
function Person:constructor(name)
  self.name = name
end

function Person:getName()
  return self.name
end

--Manager subclass
local Manager = Person:extends("Manager")
function Manager:constructor(name)
  --# Pass up the arguments to the parent class
  Manager.super.constructor(self, name)
end

--Boss subclass
local Boss = Person:extends("Boss")
function Boss:constructor(name)
  --# Pass up the arguments to the parent class
  Boss.super.constructor(self, name)
end

--instances
local manager = Manager:new("Jenny")
print(manager:getName()) --> Jenny

local boss = Boss:new("Jorge")
print(boss:getName()) --> Jorge
```

In the example above, we use a __Person__ class to represent a person. We have only defined a `name` property and `getName` method, but could add many others. All sublasses of a Person class "type" will have a `name` associated with them.

We only want to store the `name` property once. We also have access to the `Person:getName()` method, which again, we only want to define once for any subclass of a Person "type".

Our subclasses represent a __Manager__ and __Boss__, which both fit under the Person "type" and so we don't need to recreate the `name` property or `getName` method, which lives in the Person base class.

But, we do need to make sure the Person base class gets the `name` property, so we call the `<Class>.super.constructor` function in each of our subclasses `constructor` functions, which passes the name up. 

Because both the Manager and Boss are subclasses of the Person class, their `super` property points to the Person base class. You can use the `super` property to call any methods or manipulate any properties on the "parent" class. This is shown in some examples below.

<span class="far fa-bell"></span> <strong>Pay attention to the signature of the `super.constructor` function. You need to make sure you pass a reference of `self` to it, along with any additional arguments, and use only dot (__.__) syntax:</strong>

```lua
<Class>.super.constructor(self[, args])
```

## Subclass Examples

### Basic SubClass

```lua
local Classy = require("plugin.classy")

--base class (with defaults)
local Person = Classy.create("Person", { age = 24 })

function Person:getAge()
  return self.age
end

--subclass
local Boss = Person:extends("Boss")

--#########################################################
--# Instance Examples
local boss = Boss:new()

print(boss:getAge()) --> 24 (Person default)
```

### Subclass with Defaults

```lua
local Classy = require("plugin.classy")

--base class
local Person = Classy.create("Person", { name = "Unknown", age = 25 })

function Person:getAge()
  return self.age
end

function Person:getName()
  return self.name
end

--subclass
local Boss = Person:extends("Boss", { title = "Big Cheese" })

function Boss:getTitle()
  return self.title
end

--#########################################################
--# Instance Examples
local boss = Boss:new()

print(boss:getTitle()) --> Big Cheese
print(boss:getName()) --> Unknown
print(boss:getAge()) --> 25
```

### SubClass with Constructor

```lua
local Classy = require("plugin.classy")

--base class
local Person = Classy.create("Person")
function Person:constructor(name, age)
  self.name = name
  self.age = age
end

function Person:getAge()
  return self.age
end

function Person:getName()
  return self.name
end

--subclass
local Boss = Person:extends("Boss")
function Boss:constructor(name, age, title)
  Boss.super.constructor(self, name, age)

  self.title = title
end

function Boss:getTitle()
  return self.title
end

--#########################################################
--# Instance Examples
local boss = Boss:new("Mary", 42, "Big Cheese")

print(boss:getName()) --> Mary
print(boss:getAge()) --> 42
print(boss:getTitle()) --> Big Cheese

local person = Person:new("Jane", 32)

print(person:getName()) --> Jane
print(person:getTitle()) -- error (method not on Person class)
```

### Subclass Method Override

```lua
local Classy = require("plugin.classy")

--base class
local Person = Classy.create("Person")
function Person:constructor(name)
  self.name = name
end

function Person:getName()
  return self.name
end

--subclass
local Boss = Person:extends("Boss")
function Boss:constructor(name)
  Boss.super.constructor(self, name)
end

--override
function Boss:getName()
  return "I'm the boss, "..self.name
end

--#########################################################
--# Instance Examples
local person = Person:new("Jimmy")
local boss = Boss:new("Poppy")

print(person:getName()) --> Jimmy
print(boss:getName()) --> I'm the boss, Poppy
```

### Subclass Method Override with Super Call

```lua
local Classy = require("plugin.classy")

--base class
local Person = Classy.create("Person")
function Person:constructor(name)
  self.name = name
end

function Person:printName()
  print("My name is "..self.name)
end

--sub class (with defaults)
local Boss = Person:extends("Boss", { title = "The Main Taco" })
function Boss:constructor(name)
  Boss.super.constructor(self, name)
end

--override
function Boss:printName()
  --call base class method
  Boss.super.printName(self)

  print ("I am "..self.title)
end

--#########################################################
--# Instance Examples
local boss = Boss:new("Marky")
boss:printName()
-- Outputs
--> My name is Marky
--> I am The Main Taco
```

### Subclassing Subclasses

```lua
local Classy = require("plugin.classy")

--base class
local Person = Classy.create("Person")
function Person:constructor(name)
  self.name = name
end

--Boss subclass (with defaults)
local Boss = Person:extends("Boss", { title = "Top Dog" })
function Boss:constructor(name)
  Boss.super.constructor(self, name)
end

--Meal subclass
local Meal = Boss:extends("Meal")
function Meal:constructor(name, food)
  Meal.super.constructor(self, name)

  self.food = food
end

function Meal:getLunch()
  local sf = string.format

  local name = self.name
  local title = self.title
  local food = self.food

  return sf("%s the %s is having a %s for lunch.", name, title, food) 
end

--#########################################################
--# Instance Examples
local meal1 = Meal:new("Tony", "Sandwhich")
print(meal1:getLunch())
--> Tony the Top Dog is having a Sandwhich for lunch.

local meal2 = Meal:new("Tammy", "Salad")
print(meal2:getLunch())
--> Tammy the Top Dog is having a Salad for lunch.
```