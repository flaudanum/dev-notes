<h1>C#</h1>

<h2>Table of Content</h2>

- [Compilation in Visual Studio](#compilation-in-visual-studio)
- [Compilation in the Developer PowerShell for Visual Studio 2019](#compilation-in-the-developer-powershell-for-visual-studio-2019)
- [Passing parameters](#passing-parameters)
- [Nullable types](#nullable-types)
- [Glossary](#glossary)

## Compilation in Visual Studio

Created a new project in Visual Studio *tutorial-project* with a source file `HelloWorld.cs`.

```C#
using System;

namespace tutorial_project
{
    class HelloWorld
    {
        static void Main(string[] args)
        {
            // My first program
            Console.WriteLine("Hello World!");
        }
    }
}
```

Built the project for *release*:

```
1>------ Build started: Project: tutorial-project, Configuration: Release Any CPU ------
1>tutorial-project -> C:\Users\frederic.Laudarin\source\repos\tutorial-project\tutorial-project\bin\Release\netcoreapp3.1\tutorial-project.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

The following folders were created `bin\Release\netcoreapp3.1` with the following content:

```
tutorial-project.deps.json
tutorial-project.dll
tutorial-project.exe
tutorial-project.pdb
tutorial-project.runtimeconfig.dev.json
tutorial-project.runtimeconfig.json
```

Only the following files are required for running the application:

```
tutorial-project.dll
tutorial-project.exe
tutorial-project.runtimeconfig.json
```

## Compilation in the Developer PowerShell for Visual Studio 2019

In the Windows menu go to the *Visual Studio 2019* folder and start **Developer PowerShell for VS 2019**. This special PowerShell terminal is set with the proper environment variables so that the C# compiler's CLI `csc` is available.

```PowerShell
PS > csc .\HelloWorld.cs
Compilateur Microsoft (R) Visual C# version 3.7.0-6.20459.4 (7ee7c540)
Copyright (C) Microsoft Corporation. Tous droits réservés.

PS > Get-ChildItem


    Répertoire : C:\Users\frederic.Laudarin\Documents\DEV


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         9/30/2020   5:06 PM            267 HelloWorld.cs
-a----         9/30/2020   5:17 PM           3584 HelloWorld.exe

PS > .\HelloWorld.exe
Hello World!
```

## Passing parameters

By default, parameters are passed **by value**.

```C#
r = obj.func(x, y);
```

You can declare reference parameters using the `ref` keyword in the method definition and call.

```C#
class MyClass {
      public void method(ref int x, ref int y) {
          ...
      }
      static void Main(string[] args) {
         MyClass obj = new MyClass();

         ... // declaration and assignments of a and b
         
         /* calling a function to swap the values */
         obj.method(ref a, ref b);
      }
   }
}
```

A return statement can be used for returning **only one** value from a function. However, using output parameters, you can return values from a function by declaring parameters with the keyword `out`.

```C#
class NumberManipulator {
      public void getValue(out int x ) {
         int temp = 5;
         x = temp;
     }
}
```

## Nullable types

Contrary to Java, C# variable do not accept a `null` value by default. C# makes it possible by declaring nullable types:

```
< data_type> ? <variable_name> = null;
```

*e.g.*

```C#
int? num1 = null;
int? num2 = 45;
double? num3 = new double?();
double? num4 = 3.14157;
bool? boolval = new bool?();
```

The null coalescing operator `??` is used with the nullable value types and reference types for converting an operand to the type of another nullable (or not) value type operand, where an implicit conversion is possible.

```C#
double? num1 = null;
double num3;
num3 = num1 ?? 5.34;
```

## Glossary

**Assembly**: An Assembly is a basic unit of application deployment and versioning. An Assembly is also called the building block of a .Net application. An Assembly is either a .exe or .dll file.

**Internal** (*specifier*): Internal access specifier allows a class to expose its member variables and member functions to other functions and objects in the current *assembly*.
