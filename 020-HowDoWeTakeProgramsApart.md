#How do we take take programs apart?

In this article we'll review a simple program that can be implemented somewhat messily and we'll make a cleaner, much easier to reason about version, by taking the program apart, decomposing it into methods.

This does not imply that programs that can be taken apart are wrong. Often "done" is better than "perfect". These techniques may help you write your own programs though and give you mental model that will help read you read complicated programs.

Version 1:

    public class BottlesOfBeer {
      public static void main(String[] args) {
        String word = "bottles";
        int beerNum = 99;
        while(beerNum > 0) {
          System.out.print(
            "" + beerNum + " " + word + " of beer on the wall, " + beerNum + 
            " " + word + " of beer. You take one down, you pass it around, you get "
          );
          beerNum = beerNum - 1;
          if(beerNum == 1) {
            word = "bottle";
          }

          if(beerNum > 0) {
            System.out.println("" + beerNum + " " + word + " of beer on the wall.");
          } else {
            System.out.println("no more bottles of beer on the wall.");
          }
        }
      }
    }

##State

This program is full of state. That is an unprecise, sort of colloquial way to say, it has variables that change value thoughout. Every one of these variables, is an unknown thing we have to think about to know that a program is correct.

A program is a sort of machine. Variables that change are moving parts of that machine. Moving parts make machines fragile and dangerous. We should use as few as possible.

State is the space of possible positions of all the moving parts of the machine. When we talk about "a state", we mean a particular configuration of positions of those moving parts, or the particular value stored within each variable.

##Tell Me What A Variable Is Already!

OK, we're finally ready.

A variable is a bit of state, a moving part in the machine of the program. 

We use multiple variables to represent the state of the program, so that each variable can be a precisely defineable part of the whole state. Such a definition might be, "`beerNum` records the number of beers that are currently on the wall."

The word "currently" is important in that definition. It implies that the number of beers on the wall is not constant, but changes over time.

###The variables in the program

The variables in this program are `word` and `beerNum`. Let's talk about `word` first. 

When you sing the song "99 Bottles of Beer on the Wall," do you think about when to sing "bottle" vs "bottles" as a part of the song? You don't. What word you choose depends entirely on the number. This is true in our program too, but that is not obvious.

When our program starts, the value of `word` is set to "bottles", which will be correct for most of the song. In every iteration of the loop, we must check if the number of bottles left is equal to 1, no longer plural, and then correct the value if "word" if so. Following the working of this machine is hard. Let's make the relationship explicate with a method:

Version 2:

    public class BottlesOfBeer {
      private static String wordForNumberOfBottle(int nBottles) {
        if(nBottles == 1) {
          return "bottle";
        } else {
          return "bottles";
        }
      }
      public static void main(String[] args) {
        String word;
        int beerNum = 99;
        word = wordForNumberOfBottle(beerNum);
        while(beerNum > 0) {
          System.out.print(
            beerNum + " " + word + " of beer on the wall, " + 
            beerNum + " " + word + " of beer. " + 
            "You take one down, you pass it around, you get "
          );
          beerNum = beerNum - 1;
          
          word = wordForNumberOfBottle(beerNum);

          if(beerNum > 0) {
            System.out.println(beerNum + " " + word + " of beer on the wall.");
          } else {
            System.out.println("no more bottles of beer on the wall.");
          }
        }
      }
    }

Now we can see that the variable `word` is only ever set by calling a method that depends on the number of bottles remaining on the wall. It may not be super clear immediately, but this is the sort of thing that another human reading your code will check for. When a variable only every gets set one way, it is much easier to understand what it represents.

This is still harder to follow than it should be because of the variable, if we just called the method in all the places that need the value, we'd remove one whole moving part from the machine.

Version 3:

    public class BottlesOfBeer {
      private static String wordForNumberOfBottle(int nBottles) {
        if(nBottles == 1) {
          return "bottle";
        } else {
          return "bottles";
        }
      }
      public static void main(String[] args) {
        int beerNum = 99;
        while(beerNum > 0) {
          System.out.print(
            beerNum + " " + wordForNumberOfBottle(beerNum) + 
            " of beer on the wall, " + 
            beerNum + " " + wordForNumberOfBottle(beerNum) + 
            " of beer. You take one down, you pass it around, you get "
          );

          beerNum = beerNum - 1;
          
          if(beerNum > 0) {
            System.out.println(
              beerNum + " " + wordForNumberOfBottle(beerNum) + 
              " of beer on the wall."
            );
          } else {
            System.out.println("no more bottles of beer on the wall.");
          }
        }
      }
    }

Is this better? Some would say no. Some complicated looking bits are longer. Someday you may need to make a program faster and you'll look on the multiple calls to this method as potentially wasted work. But, you should almost never care about making the code faster and we'll see how getting rid of that moving part made the machine simpler at the end.

##Special Cases

Do mothers with multiple children make them all differnt dinners? No they don't. Her children are very special but none of them are more special than the others. Your programs will feel like creations sometimes. They will fill you with pride. But they are not children..... dramatic pause....

NOTHING IS SPECIAL

Our program has a special case. When there are no more beers left, we do something totally different from when there are any beers left. How different is the thing we do? We merely change the phrase we use to describe the number of beers. We already have a method that chooses a word to go after the number, and in every case that we use it, the number appears before it. We can change that method to be responsable for the whole phrase, and then it can handle all the cases.

Version 4:

    public class BottlesOfBeer {
      private static String phraseForNumberOfBottles(int nBottles) {
        if(nBottles == 1) {
          return "one bottle";
        } else if(nBottles == 0) {
          return "no more bottles";
        } else {
          return nBottles + "bottles";
        }
      }
      public static void main(String[] args) {
        int beerNum = 99;
        while(beerNum > 0) {
          System.out.print(
            phraseForNumberOfBottles(beerNum) + 
            " of beer on the wall, " + beerNum + 
            phraseForNumberOfBottles(beerNum) + 
            " of beer. You take one down, you pass it around, you get "
          );

          beerNum = beerNum - 1;
          
          System.out.println(
            phraseForNumberOfBottles + 
            " of beer on the wall."
          );
        }
      }
    }

##Decompose Everything

Our main method still looks complicated. That loop has there giant bits where we're just concatenation strings and with all those quotes and plus signs it's hard to be sure we got it correct. Let's decompose both those bit into methods.

Version 5:

    public class BottlesOfBeer {
      private static String phraseForNumberOfBottles(int nBottles) {
        if(nBottles == 1) {
          return "one bottle";
        } else if(nBottles == 0) {
          return "no more bottles";
        } else {
          return nBottles + "bottles";
        }
      }
      private static String firstPartOfVerse(int nBottles) {
        return String.format(
          "%1$s of beer on the wall, %1$s of beer, ",
          phraseForNumberOfBottles(nBottles)
        );
      }

      private static String secondPartOfVerse(int nBottles) {
        return String.format(
          "You take one down, you pass it around, you get %1$s of beer on the wall.",
          phraseForNumberOfBottles(nBottles)
        );
      }

      public static void main(String[] args) {
        int beerNum = 99;
        while(beerNum > 0) {
          
          System.out.print(firstPartOfVerse(beerNum));

          beerNum = beerNum - 1;
          
          System.out.println(secondPartOfVerse(beerNum));

        }
      }
    }

We did two things here. Thing one: We decomposed the two complicated String building things into two different methods. Thing two: we replaced the big series of Strings added together with a call to [`String.format`](http://docs.oracle.com/javase/7/docs/api/java/lang/String.html#format(java.util.Locale,%20java.lang.String,%20java.lang.Object...)). 

###Format Strings

`String.format` is a static method of the String class, which you get for free in all your programs. Its first argument is a "[format string](http://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax)" which is a template or madlib that we'll fill in with the other arguments. A `%` in a format string marks a spot to be filled in by a subsequent argument. Typically, we'd just write `%s` to say, substitute here with the next argument that has not yet been used and render it as a String. We could use other letters to specify that we render a date in the default locale or a number in scientific notation with the correct number of significant digits.

When the `%` is followed by `1$`, that means that the argument just after the format string goes there, a `2$` would mean the one after that, and so on. If we wish to use the same argument twice, we must specify which arguments to substitute. 

Please, never write a format string that specifies the position sometimes but not all the time; no human has ever remembered what happens in that case.

##Why is Version 5 better than Version 1?

Just look at it! ...

The real explanation is coming soon.

TODO TODO TODO

##Loops are Evil

The only worse thing than a loop, is repeating yourself. But our loop over the number of beers, is especially bad. The body changes state in the middle. There is the part before we took a bottle down, and the part after. This is literally twice as hard to think about than if there was There are two ways we could 

TODO TODO TODO

