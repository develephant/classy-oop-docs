# Character Classes

The first thing we need to do is determine what our various Character class attributes will be, Then we can then start building out the classes.

## Class Attributes

### Base Character

#### Properties

To keep things simple for this tutorial, we will just work with a few properties.

```
Character
  .name
  .isType
  .health
  .maxHealth
  .weapon
```

#### Methods

Now let's collect the methods we will be needing for the class.

```
Character
  :hit
  :heal
  :attack
  :getName
  :getType
  :setHealth
  :getHealth
  :getMaxHealth
  :getWeapon
  :setWeapon
  :isDead
```

### Warrior

The Warrior will be a subclass of the base Character class, but will not need any additional properties or methods.

### Boss (Warrior)

The Boss will be a direct instance of the Warrior subclass, which will be demonstrated in the [Mock Battle](/tutorial/battle/) code.

### Mage

The Mage will be a subclass of the Character class, and will need a few additional properties and methods. Since the Mage is a subclass of the Character class, the following properties and methods are _in addition_ to the properties and methods already on the Character class.

#### Properties

```
Mage
  .mana
  .maxMana
```

#### Methods

```
Mage
  :setMana
  :getMana
  :getMaxMana
  :useMana
  :hasMana
  :refillMana
```

## Classes/Subclasses

### Character

The base Character class.

_classes/Character.lua_

```lua
local Classy = require("plugin.classy")
local Character = Classy.create("Character")

function Character:constructor(name, isType, health, maxHealth, weapon)
  self.name = name
  self.isType = isType
  self.health = health
  self.maxHealth = maxHealth
  self.weapon = weapon
end

function Character:hit(amount)
  amount = self:getHealth() - amount
  self:setHealth(math.max(0, amount))
end

function Character:heal(amount)
  amount = self:getHealth() + amount
  self:setHealth(math.min(amount, self:getMaxHealth()))
end

function Character:attack(character)
  character:hit(self.weapon:getAttack())
end

function Character:getName()
  return self.name
end

function Character:getType()
  return self.isType
end

function Character:setHealth(amount)
  self.health = amount
end

function Character:getHealth()
  return self.health
end

function Character:getMaxHealth()
  return self.maxHealth
end

function Character:getWeapon()
  return self.weapon
end

function Character:setWeapon(weapon)
  self.weapon = weapon
end

function Character:isDead()
  if self:getHealth() <= 0 then
    return true
  end
  return false
end

return Character
```

#### Notes

  - This is the base class for the subclasses, and is the only time we need to use the `Classy.create` method for our character class needs.

  - All sublassess of the Character class will inherit the methods and properties in the class. So this is where we put the most common functionality that can be shared among the subclasses.

---

### Warrior

The Warrior subclass. We will also create the Boss instance from this subclass.

_classes/Warrior.lua_

```lua
local Character = require("classes.Character")
local Warrior = Character:extends("Warrior")

function Warrior:constructor(name, isType, health, maxHealth, weapon)
  Warrior.super.constructor(self, name, isType, health, maxHealth, weapon)
end

return Warrior
```

#### Notes

 - The Warrior is a subclass of the Character class. We need to pull in the Character class by using `require` so that we can "extend" it.

 - To create the Warrior class we use the `extends` method on the Character class, not `Classy.create`.

 - We need to pass in the same arguments that the base Character class is expecting in the `constructor` function. We use the `super` property to pass those values up the hierarchy. 

 - The Warrior subclass uses all of the original methods and properties of the base Character class. We could just create a direct instance from the Character class, but by creating a subclass of the Warior type we can add specialized functionality in the future to the Warrior class very easily.

---

### Mage

The Mage subclass.

_classes/Mage.lua_

```lua
local Character = require("classes.Character")
local Mage = Character:extends("Mage")

function Mage:constructor(name, isType, health, maxHealth, weapon, mana, maxMana)
  Mage.super.constructor(self, name, isType, health, maxHealth, weapon)

  self.mana = mana
  self.maxMana = maxMana
end

--override parent method
function Mage:attack(character)
  if self:getWeapon():hasCharges() then
    self:getWeapon():useCharges(3)
    self:setMana(math.max(0, self:getMana() - 10))
    --Call parent attack method
    Mage.super.attack(self, character)
  end
end

function Mage:setMana(amount)
  self.mana = amount
end

function Mage:getMana()
  return self.mana
end

function Mage:getMaxMana()
  return self.maxMana
end

function Mage:useMana(amount)
  amount = self:getMana() - amount
  self:setMana(math.max(0, amount))
end

function Mage:hasMana()
  if self:getMana() > 0 then
    return true
  end
  return false
end

function Mage:refillMana(amount)
  amount = amount or self:getMaxMana()
  amount = self:getMana() + amount
  self:setMana(math.min(amount, self:getMaxMana()))
end

return Mage
```

#### Notes

  - The Mage is a subclass of the Character class. We need to pull in the Character class by using `require` so that we can "extend" it.

  - To create the Mage class we use the `extends` method on the Character class, not `Classy.create`.

  - The Mage class has a number of customized methods and properties added to it. These will be available _in addition_ to what is in the base Character class.

  - The Mage subclass takes some additional arguments to its `constructor` function, that are specific to the subclass(`mana` and `maxMana`). We still need to pass the arguments that the base Character class is expecting in the `constructor` function. We use the `super` property to pass those values up the hierarchy, but only including the ones needed by the base Character class. 
