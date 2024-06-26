The Liskov Substitution Principle (LSP) states that if `S` is a subtype of `T`, then objects of type `T` may be replaced with objects of type `S` (i.e., an object of type `T` may be substituted with any object of a subtype `S`) without altering the correctness of the program.

When you override a method in a subclass, the new method should not be more restrictive than the original method in the superclass. This means that:

1. **Return type**: The return type of the overridden method in the subclass should either be the same as the return type of the original method in the superclass, or it should be a subtype of the original return type.
2. **Exceptions**: The overridden method in the subclass should not throw any exceptions that are not declared (or are superclasses of those declared) by the original method in the superclass.
3. **Access modifiers**: The access modifier of the overridden method in the subclass should be the same or more lenient than the access modifier of the original method in the superclass (e.g., a `protected` method in the superclass can be overridden with a `public` method in the subclass, but a `public` method in the superclass cannot be overridden with a `protected` method in the subclass).

*If we are overriding any method, then overridden method (i.e. declared in subclass / child class) must not be more restrictive.*