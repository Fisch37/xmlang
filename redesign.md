# XMLang Redesign 1
This is the first (and hopefully last) complete redesign of the XMLang specification. This file will only exist for as long as the redesign is ongoing. Over time the specifications will be changed to fit this plan.

## The Adaptive Execution Model
Instead of having a stack of local frames, XMLang will now use the code of the program as the variable frame itself.  At runtime, the interpreter will first create a DOM from the XML source code. It will then sequentially evaluate parts of the code. Whenever an object is evaluated, it will be replaced in the DOM with its evaluation result. The original object will then no longer exist.

## Object evaluation
All objects can now be evaluated, however the default evaluation result of an object is that object itself. Evaluation will not operate recursively, i.e. when an evaluation results in an object that implements an evaluation, that object will not be evaluated, even if the evaluation would not result in that same object again.

Custom evaluation of objects can still be specified using an `<@execute>`¹ child element. When an object is evaluated, all the children in its execute tag will be evaluated sequentially. The evaluation result of the last child will be used as the evaluation result of the object that was originally evaluated.

¹ _Note: I would prefer to use @ for all metatags, if at all possible. Current specification uses :, but this will hopefully change._

## Referencing
Referencing another object in the DOM will now be done using XPath. A reference object will evaluate to the object pointed to by the XPath located in its `target` attribute.
_ToDo: Can XPath find multiple objects?_

### Error handling 
