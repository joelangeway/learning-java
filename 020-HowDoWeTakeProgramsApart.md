#How do we take take programs apart?

In this article we'll review a simple program that can be implemented somewhat messily and we'll make a cleaner, much easier to reason about version, by taking the program apart, decomposing it into methods.

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

      public static String lineOfSong(int n) {
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
        for(int nBeers = 99; nBeers >= 1; nBeers-- ) {
          String line = lineOfSong(nBeers);
          System.out.println(line);
        }
      }
    }

More will be written here soon.
