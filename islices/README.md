# Package islices

islices is a collection of utility functions for manipulating golang slices. It
__extends__ the `slices` package in the standard library, and as such, does not try to
provide functionality that is already there.

As a first principle, if the functionality is in the std lib, we are not going to
provide it here. We shall, though, use the std lib, whenever possible. And we strive to
be our only dependency.

## The design of islices

`islices` follows some basic principles.  Deviations from these principles must be extensively documented, including the reasons why we deviated from them.  The basic principles are:

1. `islices` prioritizes pure functions.  That is, the functions in this library will __not__ modify its parameters.  Exceptions to this rule will be extensively documented, and reasons for that should be clear.  Note that the standard library adopts another primary strategy.  Many of the std's library functions in the `slice` package modify the given (slice) parameter in place. 


## Reason on some design decisions

We reason here some of our design decisions. A summary of these decisions are first presented, followed by explanations at the sub-sections below.

| Design decision | Short explanation |
|-------- |------- |
| [_Set_-like operations should be handle in a `set` package](#set-like-operations-should-be-handled-in-a-set-package) | Operations like Union, Intersection, etc... are ill defined for arrays, and lists.  They should be handled in a `set` datastructure where their semantics is well formed |

### __Set__-like operations should be handled in a 'set' package

`Set` operations, such as `Union`, `Intersection`, `SymmetricDifference`, `Difference`, etc. will be handled in another package.  Most likely in a package for `set` datastructures. The reason is that these functions are ill-defined in ordered sequences, like arrays or slices:

1. It is not clear how to deal with the order of elements during these operations.
2. Some operations such as `Intersection` or `Difference` are ambiguous on how to handle.  Should the individual elements of the arrays be considered, or are the whole arrays considered as an element?  A function could define and document its semantics, but it is still confusing for its use.
3. These operations tend to be inefficient when using linear, ordered structure of the slices(arrays).

For these reasons, our design strategy is to _not_ provide direct functions for that in this library interface.  Instead, use a combination of the functions here with the [`iset`](../iset) package.