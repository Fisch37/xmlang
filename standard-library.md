# Standard Library
This is specification for the expected Standard Library every interpreter should cover.

The Standard Library is available automatically without a need for imports. Every feature of this library should behave as if they were implemented in XMLang, but do not have to be. In all likelihood most if not all tags will not be implemented in XMLang, though this is up to the interpreter implementation where possible. The Standard Library should follow the goals set out in [the README](README.md)


## Numerical Operations

### Binary operations
Binary numerical operations expect two children evaluating to number tags and themselves evaluate to number tags. The following operations exist:
- `add`
- `sub`
- `mul`
- `div`
- `exp`
- `and`
- `or`
- `xor`

_Note: `and`, `or`, and `xor` only work on whole numbers. For that reason they will round to the closest integer before the operation._

### Unary operations
Standard Library specifies one unary operation: The `not` operation. It expects one child that evaluates to a number tag and itself evaluates to a number tag.

_Note: `not` is a binary operation and thus only works on whole numbers. As such all non-whole numbers will be rounded to the closest integer._

## Working with Branches
The centrepiece of branching is the `condition` tag. It serves as a container for one or multiple children tags. The executable block inside should evaluate to a number tag (ideally one behaving like a boolean). A condition is assumed fulfilled when the number value is non-zero.

### Comparison Operations
**TBD**
- `equal`
- `less-than`
- `greater-than`
- `less-than-or-equal`
- `greater-than-or-equal`

### Branching
The `branch` tag indicates a... branching operation. It expects its first child to be a `condition` tag followed by a `:execute` tag. An optional child tag `else` is also accepted.


## Looping
The `while` tag allows running a `:execute` tag as long as a `condition` is met.