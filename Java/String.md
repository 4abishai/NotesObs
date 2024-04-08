### String Functions

| Method                                                         | Description                                                                                                                                |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| `length()`                                                     | Returns the length of the string.                                                                                                         |
| `charAt(n)`                                                    | Returns the character at the specified index `n`.                                                                                         |
| `getChars(n1, n2, s, n3)`                                       | Copies characters from a string into a character array.                                                                                  |
| `equals(s)`                                                    | Compares this string to the specified string. The result is `true` if and only if the argument is not `null` and represents the same sequence of characters as this object.                                                                                                     |
| `equalsIgnoreCase(s)`                                          | Compares this `String` to another `String`, ignoring case considerations.                                                                |
| `compareTo()`                                                  | Compares two strings lexicographically. The result is a negative integer if this `String` object lexicographically precedes the argument string. The result is a positive integer if this `String` object lexicographically follows the argument string. The result is zero if the strings are equal.                                                                                                     |
| `indexOf(c)`                                                   | Returns the index within this string of the first occurrence of the specified character.                                                 |
| `indexOf(c, n)`                                                | Returns the index within this string of the first occurrence of the specified character, starting the search at the specified index.       |
| `lastIndexOf(c)`                                               | Returns the index within this string of the last occurrence of the specified character.                                                  |
| `lastIndexOf(c, n)`                                            | Returns the index within this string of the last occurrence of the specified character, searching backward starting at the specified index.                                                  |
| `substring()`                                                  | Returns a new string that is a substring of this string.                                                                                 |
| `concat()`                                                     | Concatenates the specified string to the end of this string.                                                                             |
| `replace()`                                                    | Returns a new string resulting from replacing all occurrences of the specified substring in this string with the specified replacement.  |
| `trim()`                                                       | Returns a copy of the string, with leading and trailing whitespace omitted.                                                              |
| `toUpperCase()`                                                | Converts all of the characters in this `String` to upper case.                                                                           |
| `toLowerCase()`                                                | Converts all of the characters in this `String` to lower case.                                                                           |
| `toString()`                                                   | Returns the value of the `String` object.                                                                                                |
| `valueOf()`                                                    | Returns the string representation of the value of its argument.                                                                          |

## StringBuffer

| Method                                                         | Description                                                                                                                                                             |
| -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `length()`                                                     | Returns the current length of the StringBuffer.                                                                                                                         |
| `capacity()`                                                   | Returns the total allocated capacity of the StringBuffer.                                                                                                               |
| `ensureCapacity(int minimumCapacity)`                          | Ensures that the capacity of the StringBuffer is at least equal to the specified minimum capacity.                                                                      |
| `setLength(int newLength)`                                     | Sets the length of the StringBuffer to the specified `newLength`.                                                                                                       |
| `charAt(int index)`                                            | Returns the character at the specified `index`.                                                                                                                         |
| `setCharAt(int index, char ch)`                                | Sets the character at the specified `index` to the specified `char`.                                                                                                    |
| `getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)` | Copies the specified characters from this StringBuffer into the destination character array.                                                                            |
| `append(Object obj)`                                           | Appends the string representation of the specified `Object` to the end of this StringBuffer.                                                                            |
| `append(String str)`                                           | Appends the specified `String` to the end of this StringBuffer.                                                                                                         |
| `append(int i)`                                                | Appends the string representation of the specified `int` to the end of this StringBuffer.                                                                               |
| `insert(int offset, String str)`                               | Inserts the specified `String` into this StringBuffer at the specified `offset`.                                                                                        |
| `insert(int offset, char c)`                                   | Inserts the specified `char` into this StringBuffer at the specified `offset`.                                                                                          |
| `insert(int offset, Object obj)`                               | Inserts the string representation of the specified `Object` into this StringBuffer at the specified `offset`.                                                           |
| `reverse()`                                                    | Reverses the characters in this StringBuffer.                                                                                                                           |
| `replace(int start, int end, String str)`                      | Replaces the characters in a substring of this StringBuffer with the specified `String`.                                                                                |
| `substring(int start)`                                         | Returns a new `String` that contains a subsequence of characters currently contained in this character sequence, starting from the `start` index.                       |
| `substring(int start, int end)`                                | Returns a new `String` that contains a subsequence of characters currently contained in this sequence, starting from the `start` index and ending at the `end-1` index. |
| `delete(int start, int end)`                                   | Removes the characters in a substring of this StringBuffer, from the `start` index to the `end-1` index.                                                                |
| `deleteCharAt(int index)`                                      | Removes the character at the specified `index` in this StringBuffer.                                                                                                    |
| `indexOf(String str)`                                          | Returns the index within this `String` of the first occurrence of the specified `String` substring.                                                                     |
| `indexOf(String str, int fromIndex)`                           | Returns the index within this `String` of the first occurrence of the specified `String` substring, starting at the specified `fromIndex`.                              |
| `lastIndexOf(String str)`                                      | Returns the index within this `String` of the rightmost occurrence of the specified `String` substring.                                                                 |
| `lastIndexOf(String str, int fromIndex)`                       | Returns the index within this `String` of the last occurrence of the specified `String` substring, searching backward starting at the specified `fromIndex`.            |
| `trimToSize()`                                                 | Attempts to reduce storage used for the character sequence.                                                                                                             |
### StringBuffer vs StringBuilder
In Java, `StringBuilder` and `StringBuffer` are both classes that are used to manipulate strings, but they have some differences:

1. **Thread Safety**:
   - `StringBuffer` is *synchronized*, which means it is *thread-safe*. This means that multiple threads can access and modify a `StringBuffer` object without causing any concurrency issues.
   - `StringBuilder` is *not synchronized*, which means it is *not thread-safe*. This makes it more efficient for single-threaded applications, but it is not safe for use in multi-threaded environments.

2. **Performance**:
   - `StringBuilder` is generally faster than `StringBuffer` because it does not have the overhead of synchronization.
   - In single-threaded applications, `StringBuilder` is the preferred choice for performance reasons.

3. **Modification**:
   - Both `StringBuilder` and `StringBuffer` provide methods for appending, inserting, replacing, and deleting characters in the string.
   - The methods in `StringBuffer` are synchronized, while the methods in `StringBuilder` are not.

Here's an example that demonstrates the differences:

```java
// Thread-safe StringBuffer
StringBuffer sbf = new StringBuffer("Hello");
sbf.append(" World");
System.out.println(sbf.toString()); // Output: Hello World

// Non-thread-safe StringBuilder
StringBuilder sbl = new StringBuilder("Hello");
sbl.append(" World");
System.out.println(sbl.toString()); // Output: Hello World
```

In general, you should use `StringBuilder` if you are working in a single-threaded environment and performance is a concern. If you are working in a multi-threaded environment and need to ensure thread safety, you should use `StringBuffer`.

#### toString() vs valueOf
The primary difference between `toString()` and `valueOf()` when converting a `char` to a `String` is the way they are implemented and the classes they belong to.

1. `toString()`:
   - `toString()` is an instance method, meaning it is called on a specific object instance.
   - In the case of converting a `char` to a `String`, `toString()` is a method of the `Character` class.
   - The implementation of `Character.toString(char)` simply returns a new `String` object containing the single character.

2. `valueOf()`:
   - `valueOf()` is a static method, meaning it is called directly on the class itself, without an instance.
   - The `valueOf()` method is provided by the `String` class.
   - The implementation of `String.valueOf(char)` also returns a new `String` object containing the single character.


In general:
- `toString()` is used when you have an object and you want to convert it to a `String` *representation*.
- `valueOf()` is used when you have a primitive value and you want to convert it to a `String` *object*.

The choice between `toString()` and `valueOf()` is largely a matter of personal preference and coding style. Some developers prefer the more explicit `valueOf()` method, while others find the shorter `toString()` method more concise.

```java
public class CharToStringExample {
    public static void main(String[] args) {
        char myChar = 'A';

        // Using toString()
        String usingToString = Character.toString(myChar);
        System.out.println("Using toString(): " + usingToString);

        // Using valueOf()
        String usingValueOf = String.valueOf(myChar);
        System.out.println("Using valueOf(): " + usingValueOf);
    }
}
```

The output of this program will be:

```
Using toString(): A
Using valueOf(): A
```
