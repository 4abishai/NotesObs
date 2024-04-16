1. Write a Java program which will overload the meth) function that takes a single number from the user as a parameter to:

A. Calculate the factorial of each digit of the input followed by adding their sum of factorials and printing the sum

B. Generating and printing a fibonacci series with the input number and (input number +1) as the first two numbers of such a series

C. Generating and printing a geometric progression with the input number as the first number followed by populating the rest of the progression using common

ratio = 3

Depending on whether the user selects to generate the factorial sum, fibonacci series, or geometric progression.

Next override the meth) to check if the input number:

A. is a palindrome or not, If the input number is a palindrome, print the message

"Palindrome spotted" followed by printing the number.

B. is a prime number or not. Similarly, if it is a prime number then print the message

"Prime number spotted" followed by printing the input.

In case the user inputs an invalid number, then raise a custom exception

"InvalidInputNumberException" which should be caught in the catch block and print the message "caught invalidinputnumberexception" with all the odd place characters printed as capital letters while even place characters are printed with lowercase letters.

```java
import java.util.Scanner;

public class NumberOperations {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter a number: ");
        int number = scanner.nextInt();

        System.out.println("Select an operation:");
        System.out.println("1. Factorial sum of digits");
        System.out.println("2. Fibonacci series");
        System.out.println("3. Geometric progression");
        System.out.println("4. Check if palindrome or prime");

        int choice = scanner.nextInt();

        switch (choice) {
            case 1:
                calculateFactorialSum(number);
                break;
            case 2:
                generateFibonacciSeries(number);
                break;
            case 3:
                generateGeometricProgression(number);
                break;
            case 4:
                checkPalindromeOrPrime(number);
                break;
            default:
                System.out.println("Invalid choice");
                break;
        }
    }

    public static void calculateFactorialSum(int number) {
        int sum = 0;
        while (number > 0) {
            int digit = number % 10;
            int factorial = 1;
            for (int i = 1; i <= digit; i++) {
                factorial *= i;
            }
            sum += factorial;
            number /= 10;
        }
        System.out.println("Factorial sum of digits: " + sum);
    }

    public static void generateFibonacciSeries(int number) {
        int a = number;
        int b = number + 1;
        System.out.print("Fibonacci series: " + a + ", " + b);
        for (int i = 0; i < 5; i++) {
            int c = a + b;
            System.out.print(", " + c);
            a = b;
            b = c;
        }
        System.out.println();
    }

    public static void generateGeometricProgression(int number) {
        System.out.print("Geometric progression: " + number);
        for (int i = 0; i < 4; i++) {
            number *= 3;
            System.out.print(", " + number);
        }
        System.out.println();
    }

    public static void checkPalindromeOrPrime(int number) {
        if (isPalindrome(number)) {
            System.out.println("Palindrome spotted: " + number);
        } else if (isPrime(number)) {
            System.out.println("Prime number spotted: " + number);
        } else {
            System.out.println("Neither palindrome nor prime: " + number);
        }
    }

    private static boolean isPalindrome(int number) {
        int originalNumber = number;
        int reversedNumber = 0;
        while (number > 0) {
            int digit = number % 10;
            reversedNumber = reversedNumber * 10 + digit;
            number /= 10;
        }
        return originalNumber == reversedNumber;
    }

    private static boolean isPrime(int number) {
        if (number <= 1) {
            return false;
        }
        for (int i = 2; i <= Math.sqrt(number); i++) {
            if (number % i == 0) {
                return false;
            }
        }
        return true;
    }

    private static class InvalidInputNumberException extends Exception {
        public InvalidInputNumberException(String message) {
            super(message);
        }
    }

    private static void handleInvalidInput(int number) {
        try {
            if (number < 0) {
                throw new InvalidInputNumberException("InvalidInputNumberException");
            }
        } catch (InvalidInputNumberException e) {
            String message = "caught invalidinputnumberexception";
            for (int i = 0; i < String.valueOf(number).length(); i++) {
                if (i % 2 == 0) {
                    message += String.valueOf(number).charAt(i);
                } else {
                    message += Character.toUpperCase(String.valueOf(number).charAt(i));
                }
            }
            System.out.println(message);
        }
    }
}
```

2. Write a java program that performs some operations on the following strings: [5 marks] String s1 ="Java", String s2 ="Labs", String s3 ="[[]]

a. Compare s1 and s2 find out which first in Alphabetical order

b. Create a new string which has the string s2 in the middle of s3

c. Create a new string that is the first character of s1 and the last character of s2. Next, capitalize all the letters of this created string.

d. Print out all the substrings of length 2 from the middle of the the resultant string obtained after the concatenation of s1 and s2

```java
public class StringOperations {
    public static void main(String[] args) {
        String s1 = "Java";
        String s2 = "Labs";
        String s3 = "[[]]";

        // a. Compare s1 and s2 and find out which comes first in alphabetical order
        System.out.println("a. Comparing s1 and s2 in alphabetical order:");
        if (s1.compareTo(s2) < 0) {
            System.out.println("s1 (\"Java\") comes before s2 (\"Labs\") in alphabetical order.");
        } else {
            System.out.println("s2 (\"Labs\") comes before s1 (\"Java\") in alphabetical order.");
        }

        // b. Create a new string with s2 in the middle of s3
        String newString = s3.substring(0, 2) + s2 + s3.substring(2);
        System.out.println("\nb. New string with s2 in the middle of s3: " + newString);

        // c. Create a new string that is the first character of s1 and the last character of s2, and capitalize all the letters
        char firstCharOfS1 = s1.charAt(0);
        char lastCharOfS2 = s2.charAt(s2.length() - 1);
        String createdString = Character.toString(firstCharOfS1) + Character.toString(lastCharOfS2).toUpperCase();
        System.out.println("\nc. Created string: " + createdString);

        // d. Print out all the substrings of length 2 from the middle of the resultant string obtained after the concatenation of s1 and s2
        String resultantString = s1 + s2;
        int middle = resultantString.length() / 2;
        System.out.println("\nd. Substrings of length 2 from the middle of the resultant string:");
        for (int i = middle - 1; i < middle + 1; i++) {
            System.out.println(resultantString.substring(i, i + 2));
        }
    }
}
```