Access control is a mechanism, an attribute of encapsulation which
restricts the access of certain members of a class to specific parts of a
program.
Access to members of a class can be controlled using the **access**
**modifiers**.

1. **Private**: The access level of a private modifier is only within the *class*. It cannot be accessed from outside the class.
2. **Default**: The access level of a default modifier is only within the *package*. It cannot be accessed from outside the package. If you do not specify any access level, it will be the default.
3. **Protected**: The access level of a protected modifier is within the package and outside the package through *child class*. If you do not make the child class, it cannot be accessed from outside the package.
4. **Public**: The access level of a public modifier is *everywhere*. It can be accessed from within the class, outside the class, within the package and outside the package.

**`Private` -> `Default` -> `Protected`-> `Public`**
`class`   -> `package` -> `subclass` -> `everywhere`

![[Pasted image 20240408014531.png]]

**Note**: A class cannot be private or protected.

![[Pasted image 20240423161141.png]]
#### Private Constructor
If we make any class constructor private, then we cannot create the
instance of that class from outside the class. For example:
![[Pasted image 20240408014850.png]]

**Note**: [[Liskov Substitution Principle]]
![[Pasted image 20240408015246.png]]
![[Pasted image 20240408015302.png]]