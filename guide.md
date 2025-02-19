# **Guide: Converting a Base-10 Number to Base-7 in C#**

## **Introduction**
Number systems are fundamental in computing. Base-10 (decimal) is the standard system we use daily, but computers often use different bases. In this guide, we'll explore how to convert a decimal (base-10) number into base-7 using C#.

---

## **Understanding Base Conversion**
To convert a decimal number \( N \) into base-7:
1. Divide \( N \) by **7**.
2. Record the **remainder**.
3. Update \( N \) as the quotient (integer division).
4. Repeat until \( N \) becomes 0.
5. The base-7 number is formed by reading the remainders **in reverse order**.

### **Why Use Base-7?**
- Base-7 can be useful in specific computing applications, such as encoding systems and numbering schemes.
- It is a good exercise for understanding number bases and conversion algorithms.
- Helps in improving problem-solving skills for programming contests and interviews.

### **Example Calculation**
Let's convert **100 (base-10) to base-7**:

| Step | N (Base 10) | N ÷ 7 | Quotient | Remainder |
|------|------------|-------|----------|-----------|
| 1    | 100        | 100 ÷ 7 | 14       | 2         |
| 2    | 14         | 14 ÷ 7  | 2        | 0         |
| 3    | 2          | 2 ÷ 7   | 0        | 2         |

**Final Base-7 Number**: **202** (read bottom to top)

---

## **C# Implementation**
Here’s a simple C# program that converts a base-10 number to base-7 **without using built-in functions like `Math.Abs`**:

```csharp
using System;

class Program
{
    static string ConvertToBase7(int number)
    {
        if (number == 0) return "0"; // Edge case for 0

        string result = "";
        bool isNegative = false;

        if (number < 0)
        {
            isNegative = true;
            number = -number; // Manually converting to positive
        }

        while (number > 0)
        {
            int remainder = number % 7;
            result = remainder + result; // Prepend remainder
            number /= 7;
        }

        if (isNegative)
        {
            result = "-" + result;
        }
        
        return result;
    }

    static void Main()
    {
        Console.Write("Enter a number in base-10: ");
        int input = int.Parse(Console.ReadLine());

        string base7 = ConvertToBase7(input);
        Console.WriteLine($"Base-7 equivalent: {base7}");
    }
}
```

---

## **How the Code Works**
1. **Handles Edge Cases:** If the number is `0`, it directly returns `"0"`.
2. **Manually Handles Negative Numbers:** Instead of `Math.Abs()`, we manually check if the number is negative and make it positive by multiplying by `-1`.
3. **Loops Until the Number Becomes Zero:** Extracts digits using `% 7` and builds the result in reverse.
4. **Manually Reconstructs the Negative Sign** if needed.
5. **Displays the Final Base-7 Equivalent.**

### **Breaking Down the Code**
- **`ConvertToBase7(int number)`**: This function takes a decimal number and converts it to base-7.
- **`if (number == 0) return "0";`**: Handles the special case where the input is 0.
- **Handling Negative Numbers Manually**: Uses an `if` check instead of `Math.Abs()`.
- **Building the Result**: Uses a `while` loop to calculate remainders and prepend them to the result string.
- **Final Output**: The function returns the final base-7 representation.

---

## **Example Runs**
```
Enter a number in base-10: 100
Base-7 equivalent: 202
```
```
Enter a number in base-10: 49
Base-7 equivalent: 100
```
```
Enter a number in base-10: -35
Base-7 equivalent: -50
```

---

## **Further Improvements**
- **Recursive Implementation**: The function can be rewritten recursively.
- **Supporting More Bases**: Generalizing the function to convert to any base.
- **User Interface**: Creating a graphical interface for easier input/output.

---

## **Conclusion**
This guide covers the basics of base conversion and provides a clear method for converting decimal numbers to base-7. The C# implementation ensures efficient and accurate conversion while handling edge cases without relying on built-in functions like `Math.Abs()`.


