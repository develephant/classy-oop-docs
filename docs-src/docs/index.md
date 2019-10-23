# ![logo-sm](img/logo192.png)

<h2>Simple Object-Oriented Programming (OOP) for Corona</h2>

The __Classy OOP__ plugin enables you to build out your [Corona](https://coronalabs.com/) projects using lightweight classes and subclasses all wrapped up in a simple to use API.

### Basic Class

```lua
local Classy = require("plugin.classy")

--class
local Person = Classy.create("Person")
function Person:constructor(age)
  self.age = age
end

function Person:getAge()
  return self.age
end

--instances
local p1 = Person:new(26)
local p2 = Person:new(45)

print(p1:getAge()) --> 26
print(p2:getAge()) --> 45
```