Before using __Classy__ you must `require` the plugin in your code file:

```lua
local Classy = require("plugin.classy")
```

## Classy

### create

Create a new base class. See also [Classes](/usage/class/) in the __Usage__ guide.

```lua
Classy.create(className[, defaults])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|className|The name of the class.|_String_|__Y__|
|defaults|A key/value table of default properties.|_Table_|__N__|

__Returns__

A new base class.

__Example__

_Basic_

```lua
local Person = Classy.create("Person")
```

_Defaults_

```lua
local Person = Classy.create("Person", { age = 18 })
```

---

### isClass

Utility function to check if an object is a base class or subclass.

```lua
Classy.isClass(object)
```

__Returns__

A __Boolean__ value.

__Example__

```lua
local Person = Classy.create("Person")

print(Classy.isClass(Person)) --> true
```

---

### isInstance

Utility function to check if an object is an instance of a class or subclass.

```lua
Classy.isInstance(object)
```

__Returns__

A __Boolean__ value.

__Example__

```lua
local person = Person:new()

print(Classy.isInstance(person)) --> true
```

---

## Classes

### constructor

An optional constructor function that is called when creating new instances of a class (or subclass).

```lua
<className>:constructor([args])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|args|Common delimited set of arguments to be consumed when a new instance of the class (or subclass) is created. See [new](#new).|_Various_|__N__|

__Returns__

_This method has no return value._

__Example__

```lua
function Person:constructor(name, age)
  self.name = name
  self.age = age
end
```

__Notes__

 - The passed arguments can be of any Lua type.

---

### extends

Creates a __subclass__ of a base class (or subclass). See also [Subclasses](/usage/subclass/) in the __Usage__ guide.

```lua
<className>:extends(subclassName[, defaults])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|subclassName|The name of the subclass.|_String_|__Y__|
|defaults|A key/value table of default properties.|_Table_|__N__|

__Returns__

A new subclass of the class.

__Example__

_Basic_

```lua
local Boss = Person:extends("Boss")
```

_Defaults_

```lua
local Boss = Person:extends("Boss", { title = "The Big Cheese" })
```

__Notes__

 - Subclasses can use the [constructor](#constructor) method.
 - You can use [extends](#extends) to subclass a subclass.
 - Syntactically this method can be read as "Boss extends Person".

---

### super

Access the super class, which is the class (or subclass) parent. See also [Subclasses](/usage/subclass/) in the __Usage__ guide.

<span class="far fa-bell"></span> <em>`super` is a property, not a method</em>.

```lua
--Call parent methods
<className>.super.<classMethod>(self, [args])
```

__Notes__

  - To call a `super` (parent) method you must use dot (__.__) syntax, and pass a reference of `self` as the first parameter.

## Instances

### new

Create a new instance of a class or subclass. See also [Instances](/usage/instance/) in the __Usage__ guide.

```lua
<className>:new([args])
```

__Arguments__

|Name|Description|Type|Required|
|----|-----------|----|--------|
|args|Common delimited set of arguments to pass to the class (or subclass) constructor method, if one exists. See [constructor](#constructor).|_Various_|__N__|

__Returns__

A new instance of the class or subclass.

__Example__

_Basic_

```lua
local person = Person:new()
```

_Arguments_

```lua
local person = Person:new("Tina", 23)
```

__Notes__

 - The passed arguments can be of any Lua type.


## Utilities

The following methods are called directly on an object using colon (__:__) syntax.

### instanceOf

A utility method to check if an instance is an instance of a specific class or subclass.

```lua
<instance>:instanceOf(<Class>)
```

__Returns__

A __Boolean__ value.

__Example__

```lua
local person = Person:new()

print(person:instanceOf(Person)) --> true
```

---

### subclassOf

A utility method to check if a subclass is a subclass of a specific class.

```lua
<Subclass>:subclassOf(<Class>)
```

__Returns__

A __Boolean__ value.

__Example__

```lua
local Wand = Weapon:extends("Wand")

print(Wand:subclassOf(Weapon)) --> true
```

---

### classOf

A utility method to check if a subclass is derived from a specific class.

```lua
<Class>:classOf(<Subclass>)
```

__Returns__

A __Boolean__ value.

__Example__

```lua
local Person = Classy.create("Person")
local Manager = Person:extends("Manager")

print(Person:classOf(Manager)) --> true
```

__Notes__

Syntactically this method can be read in reverse as in "is Manager a class of Person".