#Why did anyone ever use `++`?

It used to be that computers were slow, and compilers were not smart. Computers used to be so slow and compilers were so dumb that it made a difference whether you wrote in your program to add two numbers, or if you told it to just increment a number from some starting offset. If this were still true, and we needed to write a Java method that would copy int's from one array to another, it might look like this:

    public class ArrayUtils {
      public static void copyInts(int[] srcArray, int srcStart, int[] destArray, int destStart, int count) {
        int i = srcStart;
        int j = destStart;
        for( int k = 0 ; k < count ; k++) {
          destArray[ j++ ] = srcArray [ i++ ];
        }
      }
    }

This might be quicker because there are no add operations at all. When we say things like `srcArray[ i++ ]` this might be a pattern that the compiler recognizes and knows how to turn into a single instruction that means exactly: "I'm reading from a list, fetch one thing and move the pointer to the next thing."

This is no longer a good idea. Computers are fast. Compilers are smart. I would almost be willing to bet that the modern Java compiler would generate exactly the same machine instructions for this version:

    public class ArrayUtils {
      public static void copyInts(int[] srcArray, int srcStart, int[] destArray, int destStart, int count) {
        for( int k = 0 ; k < count ; k++) {
          destArray[ srcStart + k ] = srcArray [ destStart + k ];
        }
      }
    }

This version is much easier for most programmers to read. It has just one variable that changes value, `k`. For each `k`, it copies the corresponding element from `srcArray` to `destArray`.

A program is a sort of machine. If we think of variables that change value as moving parts, then everytime you use `++` or `--`, you're introducing another moving part to your program. Moving parts make machines fragile and dangerous. Use as few as possible.

Java inherited the `++` and `--` operators from C++, which inhereted them from C. C was invented in 1972 to run on a computer named the PDP-11. The computer you are reading this document on is leterally, at least, by any measure, one million times more powerful than that machine. That machine was not powerful enough to support a compiler that could "optimize" your program to run just as fast as if you wrote all those confusing tricks like `srcArray[ i++ ]` your self. But your computer is that powerful and the Java compiler is that smart.

So don't use `++` or `--` unless it is in a super clear spot, like on it's own line or as the update part of a for loop.
