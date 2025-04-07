# XMLang Redesign 1
This is the first (and hopefully last) complete redesign of the XMLang specification. This file will only exist for as long as the redesign is ongoing. Over time the specifications will be changed to fit this plan.

## The Adaptive Execution Model
Instead of having a stack of local frames, XMLang will now use the code of the program as the variable frame itself.  At runtime, the interpreter will first create a DOM from the XML source code. It will then sequentially evaluate parts of the code. Whenever an object is evaluated, it will be replaced in the DOM with its evaluation result. The original object will then no longer exist.

## Object evaluation
All objects can now be evaluated, however the default evaluation result of an object is that object itself. Evaluation will not operate recursively, i.e. when an evaluation results in an object that implements an evaluation, that object will not be evaluated, even if the evaluation would not result in that same object again.

Custom evaluation of objects can still be specified using an `<evaluate>`¹ child element. When an object is evaluated, all the children in its execute tag will be evaluated sequentially. The evaluation result of the last child will be used as the evaluation result of the object that was originally evaluated.

¹ Replacing the previous `<:execute>` tag

## Referencing
Referencing another object in the DOM will now be done using XPath. A reference object will evaluate to the object pointed to by the XPath located in its `target` attribute. 
The resulting object will be a deep copy of the referenced object. That means any changes made to a reference will not be applied to the original object. To modify objects at another location, a type of set tag should be used.

If the XPath cannot find a tag at that position, it will return an error object of the following type:
```xml
<error type="failed-reference" target="[the XPath that caused this error]" />
```

If the XPath would apply for multiple objects, the reference will return an error object of the following type:
```xml
<error type="ambiguous-reference" target="[the XPath that caused this error]" matches=[the number of objects that matched]/>
```

Note that a reference may also evaluate to an error successfully, if the XPath points to such an object.

_ToDo: Can XPath find multiple objects?_

## Error handling
When an evaluation cannot succeed in its normal operation, it should instead evaluate to an `<error>` object. This object may have any data you wish and can be used just as a normal object.