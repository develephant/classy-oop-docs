In this tutorial we'll use __Classy OOP__ to create and use some classes in an RPG setting.

While the focus will be on two main categories; Characters and Weapons, the concepts demonstrated can be applied to a variety of use cases.

## Concept

Build some classes and subclasses to support the following Character types:

  - Warrior
  - Mage
  - Boss (Warrior)

Build some classes and subclasses to create the following Weapon types:

  - Sword
  - Magic Wand
  - Battle Axe

Create a mock battle using these classes and watch it play out (via code).

## Discussion

Writing OOP classes themselves is fairly simple, the true test of good OOP is how and where you abstract your functionality.

It's real easy to abstract out your functionality too far, so the most important part of the process is thinking through your classes, and their relationships way before your start coding them.

With OOP there are many ways to approach the same problem, so there is always room for debate on best practices. Here we will try and work with a balanced approach of usability and abstraction.

## Class Trees

The first thing to think about is what our class trees will look like. In our case we know we want a Character and Weapon type, so those will most likely be our base classes.

From there we will need to create some subclasses to support the differences between the types, like a Warrior and Mage. We will also need to support the differences between a wand and melee weapons.

We also need to think about the characteristics that types will have in common. For instance we can be fairly sure that all Character types will have health (HP), and other attributes like strength, etc.

This is true of Weapon types as well, which will most likely have an attack rating attribute at the very least.

As noted above, our boss character will be a warrior, so our Character class tree might look something like:

```
Character
  Warrior
    *Boss
  Mage
```

While our Weapon class tree might be:

```
Weapon
  Melee
    *Sword
    *Battle Axe
  Magic Wand
```

<em>Note: The asterisk (__*__) represents a direct instance of the preceding subclass.</em>

Just by looking at these class trees, we can get a good idea of how we need to build out our classes and subclasses, which we will start doing in the proceeding sections.

## Project Layout

As a best practice all classes should live in their own Lua files. Additionally, we will store the class files in a _classes_ sub-directory. It's a common practice to capitalize class files.

We could of course have additonal sub-directories in the _classes_ directory for _characters_ and _weapons_, but we'll skip that for now.

```
Project
  classes/
    Character.lua
    Mage.lua
    Melee.lua
    Wand.lua
    Warrior.lua
    Weapon.lua
  main.lua
```

The __main.lua__ file will be our "battleground" for the mock battle.
