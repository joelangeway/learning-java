#What Does All That Crap Mean?

One of the frustrating things about Java as a first language, is that you can not know the meaning of most of the words and symbols in your first program. This article seeks to remove the magic, the mystery, the faith, from your first Java programs. 

We'll use this as our example, first program:

    public class Program { 
      public static void main(String[] args) {
        System.out.println("Hello, World.");
      }
    }

##What does `class` mean?

A Java class is a category of Java objects. A Java object is always an instance of a Java class. The word instance here means a particular, uniquely identifiable thing of a category; in the Salesforce world, "instance" means instance of a Salesforce environment. Further in the SalesForce world, the term "Object" means something much more like "class" in the Java world. An "object" in Java is much more like a "record" in SalesForce. In Java, the class is the basic unit of delineation. Every declaration is either a class, a memeber of a class, or is local to a single method. Every distinct bit a data or state or variable belongs to an object which is an instance of a particular class.

###Classes and objects are like forms and copies of forms

In an office, there might be many different types of forms that get filled out and filed away. In a doctor's office these forms might have names like "New Patient" and "Office Visit". Each of these forms will have distinct fields to be filled in in each copy. Those two forms might correspond to Java classes like:

    class NewPatient {
      public Long patientNumber;
      public String firstName;
      public String lastName;
      public String insuranceAccountNumber;
    }

    class OfficeVisit {
      public Long patientNumber;
      public Date dateOfVisit;
      public String reasonForVisit;
    }

Each filled out copy of a form would correspond to an object of the corresponding class.

###Classes and objects are like DB tables and rows.

If something would be a row in a database table, it would probably be an object in Java. That object would probably be an instance of a class that defined a field for every column in the table. So rows in this SQL table:

    create table Users (
      user_id     integer,
      user_name   varchar(32)
    );

might correspond to object of this Java class:

    class User {
      public int user_id;
      public String user_name;
    }

##What do `{` and `}` mean?

The `{` and `}` symbols are used to group statements or group declarations into blocks. Statements are executable Java code like `System.out.println("Hello, World.");`. Declarations are class declarations like `public class Program { ...` or method declarations like `public static void main(...` or field declarations like `public int user_id;`.

A Java class is made up of "members". Every class declaration includes a block of member declarations that we often call the class body.

A Java method is made up of statements (see next paragraph). Every method declaration includes a block of statements that we call the method body. When we call a method, we execute those statements in the method body. We'll talk more about methods under "What is a method?".

The preceding paragraph is not totally correct. A Java method might not actually provide an implementation, but instead, simply state that classes which inherit from this class must implement this method. Java has a notion of "abstract" classes and "interfaces", which allow us to define common protocols for multiple classes to all behave as though they are the same kind of thing. We will talk more about this later.

## What does `public` mean?

The very first word of our program is `public`. When we declare a class or a field or a method, we must specify the "accessability" of the thing. Other options for accessability are `protected`, `private`, and `internal`. 

When we say that a class is public, that means that the class can be referenced by any Java code in any other class or package. In order for your program to be run, the Java "runtime", needs to find your class and call its `main` method. If your class is not `public` then the Java runtime won't be able to access it. For the same reason, the `main` method also needs to be public. The word runtime is also used in the phrase "at runtime" to mean "while your program is running."

### The Client Code

Most often, when we write Java code, our code will be integrated with a whole system that already exists. When our code gets called by outside code that we're integrating with, we call the code, "the client code". In the case of a simple command line program, the Java runtime is the client code. The runtime is the first example of one of these preexisting systems to integrate with. Other examples include Hadoop, Salesforce, and Tomcat.

### Possible Accessibility Levels

`public` means anybody can get to it. `protected` means only code in this class and in classes inheriting from this class can get to it. `private` means nobody outside this class can get to it. `internal` is the default if you omit anything and means that code inside the same package can access it, but code outside the package can't, even if we inherit from the class. 

We use `public` to expose those bits of our code that the system we're integrating with will need to interact with. It is generally assumed that when a method is `public`, it is going to get called at some point. It is very common that a class will have many non-public methods that do the actual detailed work, but only a few public methods that expose a sort of black box of functionality for other code to make use of. If methods are `public` that don't need to be, it will confuse programmers trying to make use of your class.

We use `protected` to hide the details of how our class works from client code that just wants a black box, while also leaving our class open to be fixed or extended later if some client wants to tweak it by inheriting from it.

There is almost never a good reason to use `private`. Some books use it casually as an easier to explain example. When something is `private`, that generally means it is only visible inside the one class it is declared in. It can never be extended or fixed without changing the source code of that very class. 

There is almost never a good reason to use `internal`. Some books use it casually as an example because it is the default that you get when you omit any accessibility from the declaration. When something is `internal`, that generally means it is only visible inside the original source package that it is declared in. It can be extended or fixed by contorting the client code to use the same package names as the source, but that is often terribly confusing.

### Inheritance

When you're just starting out, it may be confusing to think about fixing the behavior of a class without changing the class source file at all. This is the great strength of Object Oriented Programming. Often times, a class will be used in many, many places, and changing the class will be a horrendous risk because there are too many places that a tiny change might cause an error, or the class will come from some library and changing the source code of the class would be either impossible or impractical. But, if we can make a new class, that is just like the original class, but for one little difference, and which we can use anywhere we'd use the original class, that would be much better. Inheritance lets us do that. It is related to that "abstract" and "interface" stuff briefly mentioned above. We'll talk about inheritance in detail later. For now, just know that `protected` is generally a better choice than `private` or `internal`.

##Why does a class have a name?

Java programs typically consist of many classes. We refer to those classes by name. Our sample program *is* a class called "Program". Because executable Java code always lives in methods, and methods always live in classes, in order run any code, we need a class. We give the class a name, and when we run the java command at the command line to execute our program, we tell it what class we want to execute the main() method of by name.

In order to be able to find classes, Java requires that class names match the name of the files they live in. This limits us to one class per file.

##What is a method?

In our sample program, `main` is a method. A method can be like a function in math. It is a sequence of instructions. The actual "code" that runs in a program, is always within a method. All our sample program does is print the string, "Hello, World.", so it only has that one line, one instruction, inside of its one method. A method is always a member of a class. 

##What does `static` mean?

When something is declared `static`, that means that the thing declared is for the whole class and is the same for every object of that class and it exists and is valid before any objects of the class are even created. That means that the Java runtime doesn't have to even create an object of the class `Program` in order to run our program. The runtime can call the `main` method on the class and no objects ever need enter into it, as far as the programmer cares.

There is no word which is the opposite of `static` in Java. You achieve the opposite effect of `static` by merely omitting the word `static`. We said above that every distinct bit a data or state or variable belongs to an object. Well, every class is also a sort of psuedo-object of its own special class. So when the runtime calls the main method in our sample program, it's calling a method on an object, but the object *is* the class. 

We use static when we have to face up to the fact that something in our program doesn't always correspond to a particular object instance. There can only be one entry point of the program, one main method. You will almost never want to use the word static, except to define methods which are pure functions like `Math.sin`. 

##What does void mean?

When we declare a method, the "return type" is stated before the name of the method. The return type, is the type of the value returned by a method. A method often acts like a function and returns a value after it is run, which is substituted in its place in an expression, like `Math.sin` in the expression `1 + Math.sin(0.1)`. The return type of `Math.sin` is `double`. The return type of the main method is `void`, meaning "not applicable." Void is not a type. It is an token meaning that there will be no value returned from this method.

##What's with the dots, the `.`'s in `System.out.println` ?

The dot is used to refer to members. `System.out` refers to the static field named "out" of the "System" class. `System.out.println` refers to the method named "println" of the PrintStream class, calling that method on the object refered to by `System.out`.

##What does System mean?

"System" is the name of a class, [javadoc](http://docs.oracle.com/javase/8/docs/api/index.html?java/lang/System.html). Some classes are automatically availible in every Java program, like "Math". All the most basic, primitive stuff, like how to just print a result to the screen, is done with the System class.

##What does "out" main?

"out" is a static field of the System class. It is variable that refers to an object of the class [PrintStream ](http://docs.oracle.com/javase/8/docs/api/index.html?java/io/PrintStream.html). A PrintStream object is a destination that we can write text to. It might be the console; it might be a file; it might be an SMS message. It has methods to write Strings and all sorts of numbers and provides basic control of formatting.

##What does "println" mean?

"println" is a method of the PrintStream class. It takes a String as an argument, and writes the characters of that String to the text destination, followed by a new-line character. It's like it types out the string and then hits "enter".

###What's a String?

A string, in general, means a sequence of characters, of any length including zero. A character, in general, means a letter, numeral, symbol, punctuation mark, particular kind of white space, or some other grapheme.

In Java, String is a class for representing strings as objects. The String class has some methods for getting the length, changing to all upper or lower case, trimming leading and trailing whitespace, and lots of other stuff, [javadoc](http://docs.oracle.com/javase/8/docs/api/index.html?java/lang/String.html). You'll frequently find that you don't actually need to call any methods of String objects though.

##What do the `""`'s mean?

We use double-quotes to denote a literal String object. At the place where the string literal expression appears in your program, Java will substitute an object of the String class which represents those characters.

##What does the `(` and `)` mean?

Parenthesis in Java are used to make precedence clear in expression just like in math: `(2 + 3) * 4` is NOT the same as `2 + 3 * 4`, and to surround argument lists in method declarations and method calls. When we say `System.out.println("Hello, World.")`, we're "calling" the method "println" on the object refered to by `System.out` and passing it one argument, the object of the String class which we create with the String literal, "Hello, World.". If a method accepts multiple arguments, they are separated with commas.

##What is `(String[] args)`?

When we declare a method, we must give a list of the arguments or parameters that the method accepts. We give that list as comma separated pairs of type and name. The type `String[]` is pronounced "an array of String" or "array of Strings". 

The main method, always takes one argument, which is an array of Strings. You can give that argument whatever name you like. That array, will contain one String for every argument given to you program from the command line. 

Try this program:

    public class Arrrgs {
      public static void main(String[] args) {
        System.out.println("Arrrgument 1 was: " + args[0]);
        System.out.println("Arrrgument 2 was: " + args[1]);
      }
    }

and try running these commands after compiling:

    java Arrrgs arg1 args2
    java Arrrgs "arg1 stillarg1" arg2
    java Arrrgs oneArg # <-this breaks

##What is an "array"?

An array in general, is a fixed number of slots that we can store objects in. Creating an array of length N is like declaring N variables at once, of the same type. We don't have to know N when we write the program, our programs can determine how big arrays need to be just before creating them.

We use arrays when we have to represent a lot of values and don't want to give them all individual names, or when we need to represent a variable number of values. Command line arguments are like that. The user calling our class with the java command at the command like can give us any number of arguments. It our example HelloWorld program, the number of arguments will typically be zero. 

Details about arrays will be in another article.

##What's the `;` for?

Java methods are made up of statements and control structures. A statement, is either a declaration of a local variable, or a method call. Statements, end in `;`. It's like the period in English.

A control structure is one of those things with keywords like `if`, `else`, `while`, `for`... which cause the code in a method to execute in some sequence other than straight from top to bottom. Details about control structures will be in another article. 

#Variables

This article broke one of its promises and used the word "variable" all over the place without precisely defining it. In the [next article](https://github.com/joelangeway/learning-java/blob/master/020-HowDoWeTakeProgramsApart.md) we make up for this.
