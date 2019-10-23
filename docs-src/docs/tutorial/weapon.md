# Weapon Classes

Now we need to determine what our various Weapon class attributes will be, Then we can then start building out the classes.

## Class Attributes

### Base Weapon

#### Properties

```
Weapon
  .name
  .isType
  .attack
```

#### Methods

```
Weapon
  :getName
  :getType
  :getAttack
```

### Melee Weapon

The Melee Weapon will be a subclass of the Weapon type, but will require no additional properties of methods.

The sword and battle axe will be direct instances of the Melee subclass. 

### Magic Wand

The magic wand will be a subclass of the base Weapon class, and will have a few additional properties and methods.

#### Properties

```
Magic Wand
  .manaCost
```

#### Methods

```
Magic Wand
  :getManaCost
```

## Classes/Subclasses

### Weapon

The base Weapon class.

_classes/Weapon.lua_

```lua
local Classy = require("plugin.classy")
local Weapon = Classy.create("Weapon")

function Weapon:constructor(name, isType, attack)
  self.name = name
  self.isType = isType
  self.attack = attack
end

function Weapon:getName()
  return self.name
end

function Weapon:getType()
  return self.isType
end

function Weapon:getAttack()
  return self.attack
end

return Weapon
```

#### Notes

  - This is the base class for the subclasses, and is the only time we need to use the `Classy.create` method for our weapon class needs.

  - All sublassess of the Weapon class will inherit the methods and properties in the class. So this is where we put the most common functionality that can be shared among the subclasses.
 
---

### Melee

The Melee subclass.

_classes/Melee.lua_

```lua
local Weapon = require("classes.Weapon")
local Melee = Weapon:extends("Melee")

function Melee:constructor(name, isType, attack)
  Melee.super.constructor(self, name, isType, attack)
end

return Melee
```

#### Notes

 - The Melee type is a subclass of the Weapon class. We need to pull in the Weapon class by using `require` so that we can "extend" it.

 - To create the Melee class we use the `extends` method on the Weapon class, not `Classy.create`.

 - We need to pass in the same arguments that the base Weapon class is expecting in the `constructor` function. We use the `super` property to pass those values up the hierarchy. 

 - The Melee subclass uses all of the original methods and properties of the base Weapon class. We could just create a direct instance from the Weapon class, but by creating a subclass of the Melee type we can add specialized functionality in the future to the Melee class very easily.

---

### Wand

The Wand subclass.

_classes/Wand.lua_

```lua
local Weapon = require("classes.Weapon")
local Wand = Weapon:extends("Wand")

function Wand:constructor(name, isType, attack, manaCost)
  Wand.super.constructor(self, name, isType, attack)

  self.manaCost = manaCost
end

function Wand:getManaCost()
  return self.manaCost
end

return Wand
```

#### Notes

  - The Wand is a subclass of the Weapon class. We need to pull in the Weapon class by using `require` so that we can "extend" it.

  - To create the Wand class we use the `extends` method on the Weapon class, not `Classy.create`.

  - The Wand class has an extra properties and method added to it. This will be available _in addition_ to what is in the base Weapon class.

  - The Wand subclass takes an additional argument to its `constructor` function that is specific to the subclass(`manaCost`). We still need to pass the arguments that the base Weapon class is expecting in the `constructor` function. We use the `super` property to pass those values up the hierarchy, but only including the ones needed by the base Weapon class. 