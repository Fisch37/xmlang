# XMLang 0.1 Specifications
## Core Principles
Anything in the top-level of the program file is considered code and will be executed (unless it is a XML meta instruction or a comment).

## Types
XMLang doesn't really have types. What it has is a weird limbo state where an object defines its type by being an implementation of its type. In reasonable conversation an object's type is just the name of its tag. See the following:
```xml
<sequence>
    <element><number>42</number></element>
    <element><text>Hello World</text></element>
</sequence>
```

This is an example of a sequence tag. It is a sequence tag because it is named `sequence`. It has two elements in it. They are elements because they are named that. One of the elements is a number, the other a text, etc. It is up to the programmer to ensure that tag names don't overlap with tags that have differing content and attributes.

Still, XMLang has a few built-in [templates](#templates) which currently are `number` and `text`. A few tag names are also reserved for custom behaviour such as [`var` and `reference`](#variables)

## Variables
In XMLang there is no distinction between variable declaration and initialisation. Again it is up to the programmer to ensure their variables aren't referenced before they exist.

To create a new variable, use the `var` tag. Referencing an existing variable is done using the `reference` tag.

```xml
<!-- Initialisation -->
<var label="i">
    <number>312</number>
</var>
<!-- Referencing -->
<reference label="i"/> <!-- Returns 312 -->
```

**TODO: What happens when a variable does not exist?**

Note that in XMLang variables contain expressions instead of data. That means any code added in a variable will be evaluated lazily and may change its output if itself referencing outside variables. To evaluate the expression inside a variable do the following:
```xml
<var label="i" immediate=1>
    <number>312</number>
</var>
```
_Note that XML does not have booleans. Instead, whenever setting a flag in XMLang use a number value of 0 or 1._

Changing the value of a variable is done the same way as initialisation. To protect against accidental changing of variables, use the `constant=1` attribute in the `var` tag.


### Variable Frames
Variables don't have their own scopes by default. This means any variables initialised during evaluation will exist in the outside scope as well. To guard against that, the `frame` tag can be used.
```xml
<frame>
    <!-- Anything declared in here will be void 
    when leaving the frame -->
</frame>
```
Note that external variables are still accessible inside the frame and modifying these will persist into outside code.

**TODO: Add a `sandboxed` attribute to disable stack inheritance?**

`frame` tags can be used at any point in the program. They are not required to be inside a variable.

### Variable Injections
**NOTE: Variable Injections are a draft feature. The virtual implementation of `add` for example would not match this feature at all. Another idea is elaborated on in [Executable Objects](#executable-objects)**

Non-immediate `reference` tags may contain `inject` tags to temporarily inject variables into the referenced variable's execution frame.

Here's an example:
```xml
<var label="function">
    <!-- Note: Current idea is to return the result of the 
    last operation -->
    <add>
        <reference label="a"/>
        <reference label="b"/>
    </add>
</var>

<reference label="function">
    <inject label="a"><number>420</number></inject>
    <inject label="b">
        <reference label="some-other-variable">
    </inject>
</reference>
```

## Templates
**TBD**

### Executable Objects

## Libraries & External code
**TBD**

Using XML Entities like this:
```xml
<!ENTITY library SYSTEM "http://example.org/xmlang/somelib.xml">
<var label="library" immediate=1 constant=1>&library</var>
```

## Multithreading
**Most likely very far off. Spinning up threads shouldn't be difficult (likely just creating a new object). Their target code will probably be a `var` or a `sandboxed` `frame`.**

**Race conditions can be addressed with one of the following methods:**
- Add a Global Interpreter Lock (most unlikely)
- Add individual locks to every variable (very unlikely)
- Sandbox all thread execution (allowing only constants to be accessed)
    - Allowing a list of synchronised variables (or something like that) to be passed to the thread.