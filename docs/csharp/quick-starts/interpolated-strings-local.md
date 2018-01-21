---
title: Quick Start - Interpolated strings - C# Guide
description: In this quick start about interpolated strings, you write C# code to incude the result of an expression in a larger string.
author: rpetrusha
ms.author: ronpet
ms.date: 01/11/2018
ms.topic: get-started-article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.custom: mvc
---
# Interpolated strings

This quick start teaches you how to use interpolated strings in C# to insert values into a single ouput string. You write C# code and see the results of compiling and running it. The quick start contains a series of lessons that insert values into strings, and format those values in different ways.

This quick start expects that you have a machine you can use for development. The .NET topic [Get Started in 10 minutes](https://www.microsoft.com/net/core) has instructions for setting up your local development environment on Mac, PC or Linux. A quick overview of the commands you'll use is in the [introduction to the local quick starts](local-environment.md) with links to more details. 

## Create an interpolated string

Create a directory named **interpolated-quickstart**. Make it the current directory and run the following command from a console window:

```console
dotnet new console -n interpolated -o .
```

This command creates a new .NET Core console application in the current directory.

Open **Program.cs** in your favorite editor, and replace the line `Console.WriteLine("Hello World!");` with the following code, where you replace `<name>` with your name:

```csharp
var name = "<name>";
Console.WriteLine($"Hello, {name}. It's a pleasure to meet you!");
```
Try this code by typing `dotnet run` in your console window. When you run the program, it displays a single string that includes your name in the greeting. The string included in the <xref:System.Console.WriteLine%2A> method call is an *interpolated string*. It's a kind of template that lets you construct a single string (called the *result string*) from a string that includes embedded code. Interpolated strings are particularly useful for inserting values into a string or concatenating (joining together) strings. 
    
This simple example contains the two elements that every interpolated string must have: 

- A string literal that begins with the `$` character before its opening quotation mark character. There can't be any spaces between the `$` symbol and the quotation mark character. (If you'd like to see what happens if you include one, insert a space after the `$` character, save the file, and run the program again by typing `dotnet run` in the console window. The C# compiler displays an error message, "error CS1056: Unexpected character '$'".) 

- One or more *interpolated expressions*. An interpolated expression is indicated by an opening and closing brace (`{` and `}`). You can put any C# expression that returns a value (including `null`) inside the braces. 

Let's try a few more interpolated string examples with some other data types.
    
## Include different data types

In the previous section, you used an interpolated string to insert one string inside of another. An interpolated string expression can be any data type, though. Let's try an interpolated string that has values of multiple data types. 
    
The following example includes interpolated expressions with a `Vegetable` object, a member of the `Unit` enumeration, a <xref:System.DateTime> value, and a <xref:System.Decimal> value. Replace all of the C# code in your editor with the following code, and then use the `console run` command to run it:

```csharp
using System;

public class Vegetable
{
   public Vegetable(string name) => Name = name;
   
   public string Name { get; }
   
   public override string ToString() => Name;
}

public class Example
{
   public enum Unit { item, pound, ounce, dozen };
   
   public static void Main()
   {
      var item = new Vegetable("eggplant");
      var date = DateTime.Now;
      var price = 1.99m;
      var unit = Unit.item;
      Console.WriteLine($"On {date}, the price of {item} was {price} per {unit}.");
   }
}
```
    
Note that the second interpolated expression includes the `item` object in the result string that's displayed to the console, and in this case the string "eggplant" is inserted into the result string. That's because, when the type of an interpolated expression is not a string, the C# compiler does the following:

- If the interpolated expression is `null`, the interpolated expression returns an empty string ("", or <xref:System.String.Empty?displayProperty=nameWithType>).

- If the interpolated expression is not `null`, the `ToString` method of the type of the interpolated expression is called. You can test this by commenting out the definition of the `Vegetable.ToString` method in the example by putting a comment symbol (`//`) in front of it. In the output, the string "eggplant" is replaced by the type name, "Vegetable", which is the default behavior of the <xref:System.Object.ToString?displayProperty=nameWithType> method.   

In the output from this example, the date is too precise (the price of eggplant does not vary by the second), and the price value doesn't indicate a unit of currency. In the next section, you'll learn how to fix those issues by controlling the format of strings returned by interpolated expressions.

## Control the formatting of interpolated expressions

In the previous section, two poorly formatted strings were inserted into the result string. One was a date and time value for which only the date was appropriate. The second was a price that did not indicate its unit of currency. Both issues are easy to address. Interpolated expressions can include *format strings* that control the formatting of particular types. Modify the call to `Console.WriteLine` from the previous example to include the format specifier for the date and price fields as shown in the following line:

```csharp
Console.WriteLine($"On {date:d}, the price of {item} was {price:C2} per {unit}.");
```
    
You specify a format string by following the interpolated expression with a colon and the format string. "d" is a [standard date and time format string](../../standard/base-types/standard-date-and-time-format-strings.md#the-short-date-d-format-specifier) that represents the short date format. "C2" is a  [standard numeric format string](../../standard/base-types/standard-numeric-format-strings.md#the-currency-c-format-specifier) that represents a number as a currency value with two digits after the decimal point.

A number of types in the .NET Standard libraries support a predefined set of format strings. These include all the numeric types and the date and time types. For a complete list of types that support format strings, see [Format Strings and .NET Class Library Types](../../standard/base-types/formatting-types.md#stringRef) in the [Formatting Types in .NET](../../standard/base-types/formatting-types.md) article. Any type can support a set of format strings, and you can also develop custom formatting extensions that provide custom formatting for existing types. For information on custom formatting by providing an <xref:System.ICustomFormatter> implementation, see [Custom Formatting with ICustomFormatter](../../standard/base-types/formatting-types.md#custom-formatting-with-icustomformatter) in the [Formatting Types in .NET](../../standard/base-types/formatting-types.md) article.

Try modifying the the format strings in your text editor and, each time you make a change, rerun the program to see how the changes affect the formatting of the date and time and the numeric value. Change the "d" in `{date:d}` to "t" (to display the short time format), "y" (to display the year and month), and "yyyy" (to display the year as a four-digit number). Change the "C2" in `{price:C2}` to "e" (for exponential notation) and "F3" (for a numeric value with three digits after the decimal point).

In addition to controlling formatting, you can also control the field width and alignment of the strings returned by an interpolated expression. In the next section, you'll learn how to do this.

## Control the field width and alignment of interpolated expressions

Ordinarily, when the string returned by an interpolated expression is included in a result string, it has no leading or trailing spaces. Particularly for instances in which you are working with a set of data, interpolated expressions let you specify a field width and its alignment. To see this, replace all the code in your text editor with the following code, then type `console run` to execute the program:
    
```csharp
using System;
using System.Collections.Generic;

public class Example
{
   public static void Main()
   {
      var titles = new Dictionary<string, string>();
      titles.Add("Doyle, Arthur Conan", "Hound of the Baskervilles, The");
      titles.Add("London, Jack", "Call of the Wild, The");
      titles.Add("Shakespeare, William", "Tempest, The");

      Console.WriteLine("Author and Title List");
      Console.WriteLine($"\n{"Author",-25}    {"Title",30}\n");
      foreach (var title in titles)
         Console.WriteLine($"{title.Key,-25}     {title.Value,30}");
   }
}
```
    
The names of authors are left-aligned, and the titles they wrote are right-aligned. You specify the alignment by adding a comma (",") after the expression and designating the field width. If the field width is a positive number, the field is right-aligned:

```text
{expression, width}
```

If the field width is a negative number, the field is left-aligned:

```text
{expression, -width}
```

Try removing the negative signs from the `{"Author",-25}` and `{title.Key,-25}` interpolated expressions and run the example again, as the following code does:

```csharp
Console.WriteLine($"\n{"Author",-25}    {"Title",30}\n");
foreach (var title in titles)
   Console.WriteLine($"{title.Key,-25}     {title.Value,30}");
```

This time, the author information is right-aligned.

You can combine a field width and a format string in a single interpolated expression. The field width comes first, followed by a colon and the format string. Replace all of the code inside the `Main` method with the following code, which displays three formatted strings with defined field widths. Then run the program by entering the `dotnet run` command.

```csharp
Console.WriteLine($"{DateTime.Now,-20:d} Hour {DateTime.Now,-10:HH} {1063.342,15:N2} feet");
```
The output looks something like the following:

```console
1/11/2018            Hour 09                1,063.34 feet
```

You've completed the interpolated strings quick start. 
    
You can continue with the [Arrays and collections](arrays-and-collections.md) quick start in your own development environment.

You can learn more about working with interpolated strings in the [Interpolated Strings](../language-reference/keywords/interpolated-strings.md) topic in the C# Reference.

