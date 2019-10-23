So now that we have all of our classes and subclasses set up we should take a look at using them. We'll do this through code, and observe the output in the Corona console.

If you want to follow along, you can [download the demo project](https://s3.amazonaws.com/develephant-plugins/classy/classy-demo.zip) which contains the "battle" code as well as all the classes in a Corona project.

Here I'm just going to point out some important parts of the code. You can view the full code further below.

## Code Setup

At the start of the file we localize a few `string` methods for output. You might notice we don't need to `require` the __Classy OOP__ plugin here. As a matter of fact we've only needed the plugin in a couple of the code class pages.

## Classes

### Characters

Now we bring in the [Character](/tutorial/character/#character) classes that we created:

```lua
...

local Mage    = require("classes.Mage")
local Warrior = require("classes.Warrior")

...
```

You should notice that we don't need to bring in the base __Character__ class file. We only need the subclasses, as they already have references to the base __Character__ class.

### Weapons

We follow up with the [Weapon](/tutorial/weapon/#weapon) classes:

```lua
...

local Wand    = require("classes.Wand")
local Melee   = require("classes.Melee")

...
```

Again, we don't need the base __Weapon__ class file. We only need the subclasses.

## Instances

### Weapons

Now let's create our weapon instances. We are doing this first so that we can "equip" them on our upcoming character instances. We could of course equip them after the fact using the character class `setWeapon` method, but this is just as well.

```lua
...

local sword = Melee:new('Titan Blade', 'sword', 30)
local axe   = Melee:new('Gilded Axe', 'battle axe', 50)
local wand  = Wand:new('Enchanted Stick', 'wand', 10)

...
```

Above we are creating two [Melee](/tutorial/weapon/#melee-weapon) class weapons, and one [Wand](/tutorial/weapon/#wand). The [Weapon](/tutorial/weapon/#weapon) base class takes a `name`, `isType`, and `attack` argument, which we pass for each of the instaces we created.

We now have three weapon instances that we can "equip" on our characters.

### Characters

Building the character instances is very much the same as our weapons:

```lua
...

local hero  = Warrior:new('Thundar', 'warrior', 100, 100, sword)
local boss  = Warrior:new('Azarak', 'warrior', 200, 200, axe)
local mage  = Mage:new('Chalice', 'mage', 60, 60, wand, 100, 100)

...
```

Above we have created two warriors, one a hero and the other a boss (baddie). We also have a mage along with us.

The [Warrior](/tutorial/character/#warrior_1) class needs a `name`, `isType`, `health`, `maxHealth`, and `weapon` argument passed to it. The [Mage](/tutorial/character/#mage_1) is a special subclass that also takes a `mana` and `maxMana` argument along with the others.

As you can see, we pass the previously generated weapon instances directly to the character instances.

## Output Methods

After our charactes and weapons there are a handful of utility methods to help print out the "battle" status as it takes place. These should be fairly self-explanatory.

There is one method that should be pointed out:

```lua
...

local function attackStatus(attacker, attacked)
  local weapon = attacker:getWeapon()

  psf("%s attacks %s with %s for %d HP", 
  attacker:getName(),
  attacked:getName(),
  weapon:getName(),
  weapon:getAttack())

  --Specialized for Mage type
  if attacker:instanceOf(Mage) then
    psf("%s now has %d MP", 
    attacker:getName(),
    attacker:getMana())
  end
end

...
```

In the method above, we are getting the "attack" status after an `attack` method has been called in battle. Since a [Mage](/tutorial/character/#mage_1) has an extra `mana` property, we need to treat it a little differently. We can use the [`instanceOf`](/classy/#instanceof) method to do a type check (in this case for a [Mage](/tutorial/character/#mage_1) type) and then add our logic as needed. In this case printing out the remaining `mana`.

## Battle Methods

The "battle" itself takes place in a couple of rounds. We actually only use two methods through the entire thing, those being `attack` and `heal`.

The majority of the methods are used to print out the results after each method is called.

### attack

The default `attack` method is defined in our base [Character](/tutorial/character/#character) class. It looks like this:

```lua
...

function Character:attack(character)
  character:hit(self.weapon:getAttack())
end

...
```

The method is called like so:

```lua
<attacker>:attack(<attacked>)
```

In our "battle" code it ends up like this, depending on whose doing the attacking:

```lua
...

hero:attack(boss)

...
```

The [Mage](/tutorial/character/#mage_1) character class is a little different to facilitate its `mana` property. The default `attack` method is overidden, and looks like so in the [Mage](/tutorial/character/#mage_1) class:

```lua
...

--override parent method
function Mage:attack(character)
  local weapon = self:getWeapon()
  self:useMana(weapon:getManaCost())
  
  --Call parent attack method
  Mage.super.attack(self, character)
end

...
```

In the overridden method, we want to deplete some of the mages `mana`, and then we call the `Mage.super.attack` method, which does the rest of the work of the default [Character](/tutorial/character/#character) `attack` method.

The `attack` method is still used the same in the "battle" code:

```lua
...

mage:attack(boss)

...
```

### heal

The `heal` method is defined in the base [Character](/tutorial/character/#character) class, and looks like so:

```lua
...

function Character:heal(amount)
  amount = self:getHealth() + amount
  self:setHealth(math.min(amount, self:getMaxHealth()))
end

...
```

We simply call this method in the "battle" on the instance we want to heal, along with the `amount` to heal with:

```lua
...

mage:heal(20)

...
```

## The "Battle Code

If you'd like to run the full project in Corona, and see the output, [download the demo project](https://s3.amazonaws.com/develephant-plugins/classy/classy-demo.zip). The main "battle" code is shown below:

```lua
--#############################################################################
--# Classy OOP - Mock Battle Demo
--# (c)2018 C. Byerley (develephant)
--#############################################################################
local sf = string.format
local rep = string.rep
--#############################################################################
--# Character + Weapon Classes
--#############################################################################
local Mage    = require("classes.Mage")
local Warrior = require("classes.Warrior")
local Wand    = require("classes.Wand")
local Melee   = require("classes.Melee")
--#############################################################################
--# Create Weapon Instances
--#############################################################################
local sword = Melee:new('Titan Blade', 'sword', 30)
local axe   = Melee:new('Gilded Axe', 'battle axe', 50)
local wand  = Wand:new('Enchanted Stick', 'wand', 10)
--#############################################################################
--# Create Character Instances
--#############################################################################
local hero  = Warrior:new('Thundar', 'warrior', 100, 100, sword)
local boss  = Warrior:new('Azarak', 'warrior', 200, 200, axe)
local mage  = Mage:new('Chalice', 'mage', 60, 60, wand, 100, 100)
--#############################################################################
--# Battle Messaging / Utilities
--#############################################################################
local psf = function(str, ...) print(sf(str, ...)) end
local pblock = function(str)
  print(rep("#", 80))
  print("# "..str)
  print(rep("#", 80))
end

local function warriorPresence(warrior)
  psf("The %s %s has appeared with %d HP", 
  warrior:getType(), 
  warrior:getName(), 
  warrior:getHealth())
  
  local weapon = warrior:getWeapon()

  psf("%s welds %s, a %s with %d attack", 
  warrior:getName(),
  weapon:getName(), 
  weapon:getType(), 
  weapon:getAttack())
end

local function magePresence(mage)
  psf("The %s %s has appeared with %d HP and %d MP", 
  mage:getType(), 
  mage:getName(), 
  mage:getHealth(),
  mage:getMana())
  
  local weapon = mage:getWeapon()

  psf("%s welds %s, a %s with %d attack", 
  mage:getName(),
  weapon:getName(), 
  weapon:getType(), 
  weapon:getAttack())
end

local function attackStatus(attacker, attacked)
  local weapon = attacker:getWeapon()

  psf("%s attacks %s with %s for %d HP", 
  attacker:getName(),
  attacked:getName(),
  weapon:getName(),
  weapon:getAttack())

  --Specialized for Mage type
  if attacker:instanceOf(Mage) then
    psf("%s now has %d MP", 
    attacker:getName(),
    attacker:getMana())
  end
end

local function healStatus(character, amount)
  psf("%s heals for %d HP, now has %d HP.", 
  character:getName(),
  amount,
  character:getHealth())
end

local function isDead(character)
  if character:isDead() then
    psf("%s has been killed!", character:getName())
  end
end

local function healthStatus(character)
  psf("%s now has %d HP", 
  character:getName(),
  character:getHealth())

  isDead(character)
end

pblock("Classy OOP - Mock Battle Demo")

--#############################################################################
--# Battle Start Introductions
--#############################################################################
pblock("Battle Start")

--# Hero (warrior)
warriorPresence(hero)
--# Mage
magePresence(mage)
--# Boss (warrior)
warriorPresence(boss)

--#############################################################################
--# Attack Round One
--#############################################################################
pblock("Round One")

--# Mage attacks Boss
mage:attack(boss)
attackStatus(mage, boss)
healthStatus(boss)

--# Hero attacks Boss
hero:attack(boss)
attackStatus(hero, boss)
healthStatus(boss)

--# Boss attacks Mage
boss:attack(mage)
attackStatus(boss, mage)
healthStatus(mage)

--# Boss attacks Hero
boss:attack(hero)
attackStatus(boss, hero)
healthStatus(hero)

--#############################################################################
--# Attack Round Two
--#############################################################################
pblock("Round Two")

--# Mage attacks Boss
mage:attack(boss)
attackStatus(mage, boss)
healthStatus(boss)

--# Heal Mage
mage:heal(20)
healStatus(mage, 20)

--# Hero attacks Boss
hero:attack(boss)
attackStatus(hero, boss)
healthStatus(boss)

--# Boss attacks Hero
boss:attack(hero)
attackStatus(boss, hero)
healthStatus(hero)

--# Boss attacks Mage
boss:attack(mage)
attackStatus(boss, mage)
healthStatus(mage)

pblock("Battle Done")

--#############################################################################
--# UI
--#############################################################################
display.newText({
  text = "See console for demo output.",
  x = display.contentCenterX,
  y = display.contentCenterY,
  font = native.systemFontBold
})
```

