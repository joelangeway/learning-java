#How do we take take programs apart?

In this article we'll review a simple program that can be implemented somewhat messily and we'll make a cleaner, much easier to reason about version, by taking the program apart, decomposing it into methods.

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

State, is the space of possible positions of all the moving parts of the machine. When we talk about "a state", we mean a particular configuration of positions of those moving parts, or the particular value stored within each variable.

###OK, how do we get rid of state?

functions! more coming soon...

The final version will look something like this:

    public class BottlesOfBeer {
      
      protected static String btlstr(int n) {
        if(n == 0) {
          return "no more bottles";
        } else if(n == 1) {
          return "1 bottle";
        } else {
          return "" + n + " bottles";
        }
      }

      protected static String lineOfSong(int nBeers) {
        String nBottlesStr = btlstr(nBeers);
        String nBottlesMinusOneStr = btlstr(nBeers - 1);
        String line = "" + 
          nBottlesStr + " of beer on the wall, " +
          nBottlesStr + " of beer, " +
          "you take one down and pass it around you get " +
          nBottlesMinusOneStr + " of beer on the wall.";
        return line;
      }

      public static void main(String[] args) {
        for(int nBeers = 99; nBeers >= 1; nBeers = nBeers - 1 ) {
          String line = lineOfSong(nBeers);
          System.out.println(line);
        }
      }
    }
