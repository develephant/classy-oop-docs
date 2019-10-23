Now that all the classes have been created, let's look at a few important key points in the classes and subclasses.

## Getters/Setters

You should notice that for every property there is at least a __get<Prop\>__ method and some properties also have a __set<Prop\>__ method.

This is a good practice as it allows one to add additional needs in the future when manipulating a property. You should never directly set a property, but always use its "getter" or "setter" for better control.

For example, in our Mage class:

```lua
...

function Mage:constructor(name, isType, health, maxHealth, weapon, mana, maxMana)
  Mage.super.constructor(self, name, isType, health, maxHealth, weapon)

  self.mana = mana
  self.maxMana = maxMana
end

function Mage:setMana(amount)
  self.mana = amount
end

function Mage:getMana()
  return self.mana
end

...
```

We have a `mana` property, but we would always use the `getMana` and `setMana` methods to manipulate that property. You would never want to to do `self.mana = val` or `mana = self.mana` in any of your methods.

To refill our mana we use a seperate method called `refillMana`:

```lua
...

function Mage:refillMana(amount)
  amount = amount or self:getMaxMana()
  amount = self:getMana() + amount
  self:setMana(math.min(amount, self:getMaxMana()))
end

...
```

You should notice that we never call the property directly, but use the "getter" and "setter" methods instead. 

In the example code above we "clamp" the `mana` value to the `maxMana` (using `math.min`). It's better to do this in a seperate method so we don't pollute the setting of the raw `mana` property.

The `setMana` method itself will take any value:

```lua
...

function Mage:setMana(amount)
  self.mana = amount
end

...
```

This allows us to override the `maxMana` if we want to with a "buff" or potion. For example, let's assume we have some "buff" functionality built into our Mage class that adds additional bonus mana when active, we can then easily adjust our `refillMana` method:

```lua
...

function Mage:refillMana(amount)
  amount = amount or self:getMaxMana()
  amount = self:getMana() + amount

  --do we have a buff?
  if self:hasActiveBuff() then
    amount = amount + self:getBuffValue()
    self:setMana(amount) --no clamping here
  else
    self:setMana(math.min(amount, self:getMaxMana()))
  end
end

...
```

You could put the `refillMana` functionality directly in the `setMana` method, but it is better to abstract the functionality out, keeping the property "getter" and "setter" clean.

## Subclass Notes

All subclass are created using the `extends` method on the class or subclass we want to extend. We do not use `Classy.create`, but we need to pull in the base class using `require` so that we can subclass it.

```lua
...

--The base class (or subclass)
local Character = require("classes.Character")

--Create a subclass by "extending"
local Warrior = Character:extends("Warrior")

...
```

When creating a subclass, we need to make sure to pass up the required base class arguments by using the `constructor` function. This done using the __super__ property:

```lua
...

function Warrior:constructor(name, isType, health, maxHealth, weapon)

  --Pass the arguments up the hierarchy using "super"
  Warrior.super.constructor(self, name, isType, health, maxHealth, weapon)
end

...
```

<span class="far fa-bell"></span> <strong>Pay attention to the signature of the `super.constructor` function. You need to make sure you pass a reference of `self` to it, along with any additional arguments, and use only dot (__.__) syntax:</strong>

```lua
<Class>.super.constructor(self[, args])
```

<em>This is very important to remember, or your subclasses will not work as expected.</em>.

In the case of subclasses that have their own specific properties, we only need to pass the ones the base class needs, and we then store the others on the subclass itself:

```lua
...

function Mage:constructor(name, isType, health, maxHealth, weapon, mana, maxMana)

  --Pass the base class arguments up the hierarchy using "super"
  Mage.super.constructor(self, name, isType, health, maxHealth, weapon)

  --Store properties specific to this subclass
  self.mana = mana
  self.maxMana = maxMana
end

...
```

