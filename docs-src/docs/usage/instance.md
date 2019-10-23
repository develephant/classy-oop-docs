
Instances are self contained objects that contain the methods and properties that were originally set up in the [class](/usage/class/) or [subclass](/usage/subclass/) template they are instatiated from.

Instances have their own state once created. If the class supports it, you can pass arguments when you create an instance, allowing you to customize the state of each instance.

Multiple instances can be created from a single class or subclass. Instances can be passed to other classes and subclasses as well.

## Discussion

### Conventions

An instance name is generally represented as a lowercase string. For example, "person" is a good instance identifier.

### Instance Creation

Instances are created using the [new](/classy/#new) method on the class you want to create an instance of. If a class supports arguments, you can also pass those along with the [new](/classy/#new) method.

See the [Classes](/usage/class/) and [Subclasses](/usage/subclass/) sections for details on creating class templates.

### Calling Methods

Instance methods are always called using the Lua colon (__:__) syntax, for example:

```lua
--using colon syntax
local name = person:getName()

--THIS WILL NOT WORK
--using dot syntax
local name = person.getName()
```

### Get/Set Properties

To get or set any properties of the instance, you use the standard dot (__.__) syntax, for example:

```lua
--get
local name = person.name

--set
person.name = "Billy"
```

## Instance Example

```lua
local Person = require("classes.Person")

local p1 = Person:new("Tammy")
local p2 = Person:new("Jimmy")

print(p1:getName()) --> Tammy
print(p2:getName()) --> Jimmy
```

See the class example [here](/usage/class/#class-with-methods) to get an idea of what the class looks like that produces the instances in the example above.