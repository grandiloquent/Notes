# Java Threads and the Concurrency Utilities

- [Chapter 1 : Threads and Runnables](#chapter-1--threads-and-runnables)
- [Chapter 2 : Synchronization](#chapter-2--synchronization)
- [Chapter 3 : Waiting and Notification](#chapter-3--waiting-and-notification)
- [Chapter 4:Additional Thread Capabilities](#chapter-4additional-thread-capabilities)
- [Chapter 5: Concurrency Utilities and Executors](#chapter-5-concurrency-utilities-and-executors)
- [Chapter 6 : Synchronizers](#chapter-6--synchronizers)
- [Chapter 7 : The Locking Framework](#chapter-7--the-locking-framework)
- [Chapter 8 : Additional Concurrency Utilities](#chapter-8--additional-concurrency-utilities)

## Chapter 1 : Threads and Runnables

Java applications execute via threads, which are independent paths of execution through an application’s code. Each Java application has a default main thread that executes the main() method. The application can also create threads to perform time-intensive tasks in the background so that it remains responsive to its users. These threads execute code sequences encapsulated in objects that are known as runnables.

The Thread class provides a consistent interface to the underlying operating system’s threading architecture. (The operating system is typically responsible for creating and managing threads.) A single operating system thread is associated with a Thread object.

The Runnable interface supplies the code to be executed by the thread that’s associated with a Thread object. This code is located in Runnable’s void run() method—a thread receives no arguments and returns no value although it might throw an exception.

Except for the default main thread, threads are introduced to applications by creating the appropriate Thread and Runnable objects. Thread declares several constructors for initializing Thread objects. Several of these constructors require a Runnable object as an argument.

A Thread object associates state with a thread. This state consists of a name, an indication of whether the thread is alive or dead, the execution state of the thread (is it runnable?), the thread’s priority, and an indication of whether the thread is daemon or nondaemon.

After creating a Thread or Thread subclass object, you start the thread associated with this object by calling Thread’s void start() method. This method throws IllegalThreadStateException when the thread was previously started and is running or the thread has died.

Along with simple thread tasks for configuring a Thread object and starting the associated thread, the Thread class supports more advanced tasks, which include interrupting another thread, joining one thread to another thread, and causing a thread to go to sleep.

```java
public class ThreadDemo
{
   public static void main(String[] args)
   {
      boolean isDaemon = args.length != 0;
      Runnable r = new Runnable()
                   {
                      @Override
                      public void run()
                      {
                         Thread thd = Thread.currentThread();
                         while (true)
                            System.out.printf("%s is %salive and in %s " +
                                              "state%n",
                                              thd.getName(), 
                                              thd.isAlive() ? "" : "not ", 
                                              thd.getState());
                      }
                   };
      Thread t1 = new Thread(r, "thd1");
      if (isDaemon)
         t1.setDaemon(true);
      System.out.printf("%s is %salive and in %s state%n",
                        t1.getName(), 
                        t1.isAlive() ? "" : "not ", 
                        t1.getState());
      Thread t2 = new Thread(r);
      t2.setName("thd2");
      if (isDaemon)
         t2.setDaemon(true);
      System.out.printf("%s is %salive and in %s state%n",
                        t2.getName(), 
                        t2.isAlive() ? "" : "not ", 
                        t2.getState());
      t1.start();
      t2.start();
   }
}
```

```java
public class ThreadDemo
{
   public static void main(String[] args)
   {
      Runnable r = new Runnable()
                   {
                      @Override
                      public void run()
                      {
                         String name = Thread.currentThread().getName();
                         int count = 0;
                         while (!Thread.interrupted())
                            System.out.println(name + ": " + count++);
                      }
                   };
      Thread thdA = new Thread(r);
      Thread thdB = new Thread(r);
      thdA.start();
      thdB.start();
      while (true)
      {
         double n = Math.random();
         if (n >= 0.49999999 && n <= 0.50000001)
            break;
      }
      thdA.interrupt();
      thdB.interrupt();
   }
}
```

```java
import java.math.BigDecimal;

public class ThreadDemo
{
   // constant used in pi computation

   private static final BigDecimal FOUR = BigDecimal.valueOf(4);

   // rounding mode to use during pi computation

   private static final int roundingMode = BigDecimal.ROUND_HALF_EVEN;

   private static BigDecimal result;

   public static void main(String[] args)
   {
      Runnable r = () -> 
                   {
                      result = computePi(50000);
                   };
      Thread t = new Thread(r);
      t.start();
      try
      {
         t.join();
      }
      catch (InterruptedException ie)
      {
         // Should never arrive here because interrupt() is never
         // called.
      }
      System.out.println(result);
   }

   /*
     * Compute the value of pi to the specified number of digits after the
     * decimal point. The value is computed using Machin's formula:
     *
     * pi/4 = 4*arctan(1/5)-arctan(1/239)
     *
     * and a power series expansion of arctan(x) to sufficient precision.
     */

    public static BigDecimal computePi(int digits)
    {
       int scale = digits + 5;
       BigDecimal arctan1_5 = arctan(5, scale);
       BigDecimal arctan1_239 = arctan(239, scale);
       BigDecimal pi = arctan1_5.multiply(FOUR).
                       subtract(arctan1_239).multiply(FOUR);
       return pi.setScale(digits, BigDecimal.ROUND_HALF_UP);
    }

    /*
     * Compute the value, in radians, of the arctangent of the inverse of 
     * the supplied integer to the specified number of digits after the 
     * decimal point. The value is computed using the power series 
     * expansion for the arc tangent:
     *
     * arctan(x) = x-(x^3)/3+(x^5)/5-(x^7)/7+(x^9)/9 ...
     */

    public static BigDecimal arctan(int inverseX, int scale) 
    {
       BigDecimal result, numer, term;
       BigDecimal invX = BigDecimal.valueOf(inverseX);
       BigDecimal invX2 = BigDecimal.valueOf(inverseX * inverseX);
       numer = BigDecimal.ONE.divide(invX, scale, roundingMode);
       result = numer;
       int i = 1;
       do
       {
          numer = numer.divide(invX2, scale, roundingMode);
          int denom = 2 * i + 1;
          term = numer.divide(BigDecimal.valueOf(denom), scale, 
                              roundingMode);
          if ((i % 2) != 0)
             result = result.subtract(term);
          else
             result = result.add(term);
          i++;
       }
       while (term.compareTo(BigDecimal.ZERO) != 0);
       return result;
    }
}
```

```java
public class ThreadDemo
{
   public static void main(String[] args)
   {
      Runnable r = new Runnable()
                   {
                      @Override
                      public void run()
                      {
                         String name = Thread.currentThread().getName();
                         int count = 0;
                         while (!Thread.interrupted())
                            System.out.println(name + ": " + count++);
                      }
                   };
      Thread thdA = new Thread(r);
      Thread thdB = new Thread(r);
      thdA.start();
      thdB.start();
      try
      {
         Thread.sleep(2000);
      }
      catch (InterruptedException ie)
      {
      }
      thdA.interrupt();
      thdB.interrupt();
   }
}
```

## Chapter 2 : Synchronization

Developing multithreaded applications is much easier when threads don’t interact, typically via shared variables. When interaction occurs, race conditions, data races, and cached variable problems can arise that make an application thread-unsafe.

You can use synchronization to solve race conditions, data races, and cached variable problems. Synchronization is a JVM feature that ensures that two or more concurrent threads don’t simultaneously execute a critical section that must be accessed in a serial manner.

Liveness refers to something beneficial happening eventually. A liveness failure occurs when an application reaches a state in which it can make no further progress. Multithreaded applications face the liveness challenges of deadlock, livelock, and starvation.

Synchronization exhibits two properties: mutual exclusion and visibility. The synchronized keyword is associated with both properties. Java also provides a weaker form of synchronization involving visibility only, and associates only this property with the volatile keyword.

When a field variable is declared volatile, it cannot also be declared final. However, this isn’t a problem because Java also lets you safely access a final field without the need for synchronization. You will often use final to help ensure thread safety in the context of an immutable class.

```java
public class DeadlockDemo
{
   private final Object lock1 = new Object();
   private final Object lock2 = new Object();

   public void instanceMethod1()
   {
      synchronized(lock1)
      {
         synchronized(lock2)
         {
            System.out.println("first thread in instanceMethod1");
            // critical section guarded first by
            // lock1 and then by lock2
         }
      }
   }

   public void instanceMethod2()
   {
      synchronized(lock2)
      {
         synchronized(lock1)
         {
            System.out.println("second thread in instanceMethod2");
            // critical section guarded first by
            // lock2 and then by lock1
         }
      }
   }

   public static void main(String[] args)
   {
      final DeadlockDemo dld = new DeadlockDemo();
      Runnable r1 = new Runnable()
                    {
                       @Override
                       public void run()
                       {
                          while(true)
                          {
                             dld.instanceMethod1();
                             try
                             {
                                Thread.sleep(50);
                             }
                             catch (InterruptedException ie)
                             {
                             }
                          }
                       }
                    };
      Thread thdA = new Thread(r1);
      Runnable r2 = new Runnable()
                    {
                       @Override
                       public void run()
                       {
                          while(true)
                          {
                             dld.instanceMethod2();
                             try
                             {
                                Thread.sleep(50);
                             }
                             catch (InterruptedException ie)
                             {
                             }
                          }
                        }
                    };
      Thread thdB = new Thread(r2);
      thdA.start();
      thdB.start();
   }
}
```

```java
public class ID
{
   private int counter; // initialized to 0 by default

   public int getID()
   {
      return counter++;
   }

   public static void main(String[] args)
   {
      ID id = new ID();
      System.out.println(id.getID());
   }
}
```

```java
public class ID
{
   private int counter; // initialized to 0 by default

   public synchronized int getID()
   {
      return counter++;
   }

   public static void main(String[] args)
   {
      ID id = new ID();
      System.out.println(id.getID());
   }
}
```

```java
public class ID
{
   private static int counter; // initialized to 0 by default

   public static int getID()
   {
      return counter++;
   }

   public static void main(String[] args)
   {
      System.out.println(ID.getID());
   }
}
```

```java
public class ID
{
   private static int counter; // initialized to 0 by default

   public static synchronized int getID()
   {
      return counter++;
   }

   public static void main(String[] args)
   {
      System.out.println(ID.getID());
   }
}
```

```java
import java.math.BigDecimal;

public class JoinDemo
{
   // constant used in pi computation

   private static final BigDecimal FOUR = BigDecimal.valueOf(4);

   // rounding mode to use during pi computation

   private static final int roundingMode = BigDecimal.ROUND_HALF_EVEN;

   private static BigDecimal result;

   public static void main(String[] args)
   {
      Runnable r = () -> 
                   {
                      synchronized(FOUR)
                      {
                         result = computePi(50000);
                      }
                   };
      Thread t = new Thread(r);
      t.start();
      try
      {
         t.join();
      }
      catch (InterruptedException ie)
      {
         // Should never arrive here because interrupt() is never
         // called.
      }
      synchronized(FOUR)
      {
         System.out.println(result);
      }
   }

   /*
     * Compute the value of pi to the specified number of digits after the
     * decimal point. The value is computed using Machin's formula:
     *
     * pi/4 = 4*arctan(1/5)-arctan(1/239)
     *
     * and a power series expansion of arctan(x) to sufficient precision.
     */

    public static BigDecimal computePi(int digits)
    {
       int scale = digits + 5;
       BigDecimal arctan1_5 = arctan(5, scale);
       BigDecimal arctan1_239 = arctan(239, scale);
       BigDecimal pi = arctan1_5.multiply(FOUR).
                       subtract(arctan1_239).multiply(FOUR);
       return pi.setScale(digits, BigDecimal.ROUND_HALF_UP);
    }

    /*
     * Compute the value, in radians, of the arctangent of the inverse of 
     * the supplied integer to the specified number of digits after the 
     * decimal point. The value is computed using the power series 
     * expansion for the arc tangent:
     *
     * arctan(x) = x-(x^3)/3+(x^5)/5-(x^7)/7+(x^9)/9 ...
     */

    public static BigDecimal arctan(int inverseX, int scale) 
    {
       BigDecimal result, numer, term;
       BigDecimal invX = BigDecimal.valueOf(inverseX);
       BigDecimal invX2 = BigDecimal.valueOf(inverseX * inverseX);
       numer = BigDecimal.ONE.divide(invX, scale, roundingMode);
       result = numer;
       int i = 1;
       do
       {
          numer = numer.divide(invX2, scale, roundingMode);
          int denom = 2 * i + 1;
          term = numer.divide(BigDecimal.valueOf(denom), scale, 
                              roundingMode);
          if ((i % 2) != 0)
             result = result.subtract(term);
          else
             result = result.add(term);
          i++;
       }
       while (term.compareTo(BigDecimal.ZERO) != 0);
       return result;
    }
}
```

```java
import java.math.BigDecimal;

public class JoinDemo
{
   // constant used in pi computation

   private static final BigDecimal FOUR = BigDecimal.valueOf(4);

   // rounding mode to use during pi computation

   private static final int roundingMode = BigDecimal.ROUND_HALF_EVEN;

   private static volatile BigDecimal result;

   public static void main(String[] args)
   {
      Runnable r = () -> 
                   {
                      result = computePi(50000);
                   };
      Thread t = new Thread(r);
      t.start();
      try
      {
         t.join();
      }
      catch (InterruptedException ie)
      {
         // Should never arrive here because interrupt() is never
         // called.
      }
      System.out.println(result);
   }

   /*
     * Compute the value of pi to the specified number of digits after the
     * decimal point. The value is computed using Machin's formula:
     *
     * pi/4 = 4*arctan(1/5)-arctan(1/239)
     *
     * and a power series expansion of arctan(x) to sufficient precision.
     */

    public static BigDecimal computePi(int digits)
    {
       int scale = digits + 5;
       BigDecimal arctan1_5 = arctan(5, scale);
       BigDecimal arctan1_239 = arctan(239, scale);
       BigDecimal pi = arctan1_5.multiply(FOUR).
                       subtract(arctan1_239).multiply(FOUR);
       return pi.setScale(digits, BigDecimal.ROUND_HALF_UP);
    }

    /*
     * Compute the value, in radians, of the arctangent of the inverse of 
     * the supplied integer to the specified number of digits after the 
     * decimal point. The value is computed using the power series 
     * expansion for the arc tangent:
     *
     * arctan(x) = x-(x^3)/3+(x^5)/5-(x^7)/7+(x^9)/9 ...
     */

    public static BigDecimal arctan(int inverseX, int scale) 
    {
       BigDecimal result, numer, term;
       BigDecimal invX = BigDecimal.valueOf(inverseX);
       BigDecimal invX2 = BigDecimal.valueOf(inverseX * inverseX);
       numer = BigDecimal.ONE.divide(invX, scale, roundingMode);
       result = numer;
       int i = 1;
       do
       {
          numer = numer.divide(invX2, scale, roundingMode);
          int denom = 2 * i + 1;
          term = numer.divide(BigDecimal.valueOf(denom), scale, 
                              roundingMode);
          if ((i % 2) != 0)
             result = result.subtract(term);
          else
             result = result.add(term);
          i++;
       }
       while (term.compareTo(BigDecimal.ZERO) != 0);
       return result;
    }
}
```

```java
public class ThreadStopping
{
   public static void main(String[] args)
   {
      class StoppableThread extends Thread
      {
         private boolean stopped; // defaults to false

         @Override
         public void run()
         {
            while(!stopped)
              System.out.println("running");
         }

         void stopThread()
         {
            stopped = true;
         }
      }
      StoppableThread thd = new StoppableThread();
      thd.start();
      try
      {
         Thread.sleep(1000); // sleep for 1 second
      }
      catch (InterruptedException ie)
      {
      }
      thd.stopThread();
   }
}
```

```java
public class ThreadStopping
{
   public static void main(String[] args)
   {
      class StoppableThread extends Thread
      {
         private boolean stopped; // defaults to false

         @Override
         public void run()
         {
            synchronized(this)
            {
               while(!stopped)
                 System.out.println("running");
            }
         }

         synchronized void stopThread()
         {
            stopped = true;
         }
      }
      StoppableThread thd = new StoppableThread();
      thd.start();
      try
      {
         Thread.sleep(1000); // sleep for 1 second
      }
      catch (InterruptedException ie)
      {
      }
      thd.stopThread();
   }
}
```

```java
public class ThreadStopping
{
   public static void main(String[] args)
   {
      class StoppableThread extends Thread
      {
         private boolean stopped; // defaults to false

         @Override
         public void run()
         {
            boolean _stopped = false;
            while (!_stopped)
            {
               synchronized(this)
               {
                  _stopped = stopped;
               }
               System.out.println("running");
            }
         }

         synchronized void stopThread()
         {
            stopped = true;
         }
      }
      StoppableThread thd = new StoppableThread();
      thd.start();
      try
      {
         Thread.sleep(1000); // sleep for 1 second
      }
      catch (InterruptedException ie)
      {
      }
      thd.stopThread();
   }
}
```

```java
public class ThreadStopping
{
   public static void main(String[] args)
   {
      class StoppableThread extends Thread
      {
         private volatile boolean stopped; // defaults to false

         @Override
         public void run()
         {
            while(!stopped)
              System.out.println("running");
         }

         void stopThread()
         {
            stopped = true;
         }
      }
      StoppableThread thd = new StoppableThread();
      thd.start();
      try
      {
         Thread.sleep(1000); // sleep for 1 second
      }
      catch (InterruptedException ie)
      {
      }
      thd.stopThread();
   }
}
```

```java
import java.util.Set;
import java.util.TreeSet;

public final class Planets 
{
   private final Set<String> planets = new TreeSet<>();

   public Planets() 
   {
      planets.add("Mercury");
      planets.add("Venus");
      planets.add("Earth");
      planets.add("Mars");
      planets.add("Jupiter");
      planets.add("Saturn");
      planets.add("Uranus");
      planets.add("Neptune");
   }

   public boolean isPlanet(String planetName) 
   {
      return planets.contains(planetName);
   }
}
```

```java
public class UsePlanets
{
   public static void main(String[] args)
   {
      Planets planets = new Planets();
      System.out.println(planets.isPlanet("Earth"));
      System.out.println(planets.isPlanet("Vulcan"));
   }
}
```

## Chapter 3 : Waiting and Notification

Java provides an API that supports communication between threads. This API consists of Object’s three wait() methods, one notify() method, and one notifyAll() method. The wait() methods wait for a condition to exist; notify() and notifyAll() notify waiting threads when the condition exists.

The wait(), notify(), and notifyAll() methods are called from within a synchronized block that synchronizes on the same object as the object on which they are called. Because of spurious wakeups, wait() is called from a while loop that reexecutes wait() while the condition doesn’t hold.

A classic example of thread communication involving conditions is the relationship between a producer thread and a consumer thread. The producer thread produces data items to be consumed by the consumer thread. Each produced data item is stored in a shared variable.

To overcome problems such as consuming a data item that hasn’t been produced, the producer thread must wait until it’s notified that the previously produced data item has been consumed, and the consumer thread must wait until it’s notified that a new data item has been produced.

```java
public class PC
{
   public static void main(String[] args)
   {
      Shared s = new Shared();
      new Producer(s).start();
      new Consumer(s).start();
   }
}

class Shared
{
   private char c;
   private volatile boolean writeable = true;

   synchronized void setSharedChar(char c)
   {
      while (!writeable)
         try
         {
            wait();
         }
         catch (InterruptedException ie) 
         {
         }
      this.c = c;
      writeable = false;
      notify();
   }

   synchronized char getSharedChar()
   {
      while (writeable)
         try
         {
            wait();
         }
         catch (InterruptedException ie) 
         {
         }
      writeable = true;
      notify();
      return c;
   }
}

class Producer extends Thread
{
   private final Shared s;

   Producer(Shared s)
   {
      this.s = s;
   }

   @Override
   public void run()
   {
      for (char ch = 'A'; ch <= 'Z'; ch++)
      {
         s.setSharedChar(ch);
         System.out.println(ch + " produced by producer.");
      }
   }
}
class Consumer extends Thread
{
   private final Shared s;

   Consumer(Shared s)
   {
      this.s = s;
   }

   @Override
   public void run()
   {
      char ch;
      do
      {
         ch = s.getSharedChar();
         System.out.println(ch + " consumed by consumer.");
      }
      while (ch != 'Z');
   }
}
```

```java
public class PC
{
   public static void main(String[] args)
   {
      Shared s = new Shared();
      new Producer(s).start();
      new Consumer(s).start();
   }
}

class Shared
{
   private char c;
   private volatile boolean writeable = true;

   synchronized void setSharedChar(char c)
   {
      while (!writeable)
         try
         {
            wait();
         }
         catch (InterruptedException ie) 
         {
         }
      this.c = c;
      writeable = false;
      notify();
   }

   synchronized char getSharedChar()
   {
      while (writeable)
         try
         {
            wait();
         }
         catch (InterruptedException ie) 
         {
         }
      writeable = true;
      notify();
      return c;
   }
}

class Producer extends Thread
{
   private final Shared s;

   Producer(Shared s)
   {
      this.s = s;
   }

   @Override
   public void run()
   {
      for (char ch = 'A'; ch <= 'Z'; ch++)
      {
         synchronized(s)
         {
            s.setSharedChar(ch);
            System.out.println(ch + " produced by producer.");
         }
      }
   }
}
class Consumer extends Thread
{
   private final Shared s;

   Consumer(Shared s)
   {
      this.s = s;
   }

   @Override
   public void run()
   {
      char ch;
      do
      {
         synchronized(s)
         {
            ch = s.getSharedChar();
            System.out.println(ch + " consumed by consumer.");
         }
      }
      while (ch != 'Z');
   }
}
```

## Chapter 4:Additional Thread Capabilities

The ThreadGroup class describes a thread group, which stores a set of threads. It simplifies thread management by applying method calls to all contained threads. You should avoid using thread groups because the most useful methods are deprecated and because of a race condition.

The ThreadLocal class describes a thread-local variable, which lets you associate per-thread data (such as a user ID) with a thread. It provides a separate storage slot to each thread that accesses the variable. Think of a thread-local variable as a multislot variable in which each thread can store a different value in the same variable. Each thread sees only its value and is unaware of other threads having their own values in this variable. Values stored in thread-local variables are not related. A parent thread can use the InheritableThreadLocal class to pass a value to a child thread.

It’s often necessary to schedule a task for one-shot execution or for repeated execution at regular intervals. Java 1.3 introduced the Timer Framework, which consists of Timer and TimerTask classes, to facilitate working with threads in a timer context.

```java
public class ExceptionThread
{
   public static void main(String[] args)
   {
      Runnable r = new Runnable()
                   {
                      @Override
                      public void run()
                      {
                         int x = 1 / 0; // Line 10
                      }
                   };
      Thread thd = new Thread(r);
      thd.start();
   }
}
```

```java
public class ExceptionThread
{
   public static void main(String[] args)
   {
      Runnable r = new Runnable()
                   {
                      @Override
                      public void run()
                      {
                         int x = 1 / 0;
                      }
                   };
      Thread thd = new Thread(r);
      Thread.UncaughtExceptionHandler uceh;
      uceh = new Thread.UncaughtExceptionHandler()
             {
                @Override
                public void uncaughtException(Thread t, Throwable e)
                {
                   System.out.println("Caught throwable " + e + 
                                      " for thread " + t);
                }
             };
      thd.setUncaughtExceptionHandler(uceh);
      uceh = new Thread.UncaughtExceptionHandler()
             {
                @Override
                public void uncaughtException(Thread t, Throwable e)
                {
                   System.out.println("Default uncaught exception handler");
                   System.out.println("Caught throwable " + e + 
                                      " for thread " + t);
                }
             };
      thd.setDefaultUncaughtExceptionHandler(uceh);
      thd.start();
   }
}
```

```java
public class InheritableThreadLocalDemo
{
   private static final InheritableThreadLocal<Integer> intVal =
      new InheritableThreadLocal<Integer>();

   public static void main(String[] args)
   {
      Runnable rP = () ->
                    {
                       intVal.set(new Integer(10));
                       Runnable rC = () ->
                                     {
                                        Thread thd = Thread.currentThread();
                                        String name = thd.getName();
                                        System.out.printf("%s %d%n", name,
                                                          intVal.get());
                                     };
                       Thread thdChild = new Thread(rC);
                       thdChild.setName("Child");
                       thdChild.start();
                    };
      new Thread(rP).start();
   }
}
```

```java
public class ThreadLocalDemo
{
   private static volatile ThreadLocal<String> userID =
      new ThreadLocal<String>();

   public static void main(String[] args)
   {
      Runnable r = new Runnable()
                   {
                      @Override
                      public void run()
                      {
                         String name = Thread.currentThread().getName();
                         if (name.equals("A"))
                            userID.set("foxtrot");
                         else
                            userID.set("charlie");
                         System.out.println(name + " " + userID.get());
                      }
                   };
      Thread thdA = new Thread(r);
      thdA.setName("A");
      Thread thdB = new Thread(r);
      thdB.setName("B");
      thdA.start();
      thdB.start();
   }
}
```

```java
import java.util.Timer;
import java.util.TimerTask;

public class TimerDemo
{
   public static void main(String[] args)
   {
      TimerTask task = new TimerTask()
                       {
                          @Override
                          public void run()
                          {
                             System.out.println("alarm going off");
                             System.exit(0);
                          }
                       };
      Timer timer = new Timer();
      timer.schedule(task, 2000); // Execute one-shot timer task after
                                  // 2-second delay.
   }
}
```

```java
import java.util.Timer;
import java.util.TimerTask;

public class TimerDemo
{
   public static void main(String[] args)
   {
      TimerTask task = new TimerTask()
                       {
                          @Override
                          public void run()
                          {
                             System.out.println(System.currentTimeMillis());
                          }
                       };
      Timer timer = new Timer();
      timer.schedule(task, 0, 1000);
   }
}
```

## Chapter 5: Concurrency Utilities and Executors

Java’s low-level thread capabilities let you create multithreaded applications that offer better performance and responsiveness over their single-threaded counterparts. However, performance issues that affect an application’s scalability and other problems resulted in Java 5’s introduction of the concurrency utilities.

The concurrency utilities organize various types into three packages: java.util.concurrent, java.util.concurrent.atomic, and java.util.concurrent.locks. Basic types for executors, thread pools, concurrent hashmaps, and other high-level concurrency constructs are stored in java.util.concurrent; classes that support lock-free, thread-safe programming on single variables are stored in java.util.concurrent.atomic; and types for locking and waiting on conditions are stored in java.util.concurrent.locks.

An executor decouples task submission from task-execution mechanics and is described by the Executor, ExecutorService, and ScheduledExecutorService interfaces. You obtain an executor by calling one of the utility methods in the Executors class. Executors are associated with callables and futures.

```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class CalculateE
{
   final static int LASTITER = 17;

   public static void main(String[] args)
   {
      ExecutorService executor = Executors.newFixedThreadPool(1);
      Callable<BigDecimal> callable;
      callable = new Callable<BigDecimal>()
                 {
                    @Override
                    public BigDecimal call()
                    {
                       MathContext mc = 
                         new MathContext(100, RoundingMode.HALF_UP);
                       BigDecimal result = BigDecimal.ZERO;
                       for (int i = 0; i <= LASTITER; i++)
                       {
                          BigDecimal factorial = 
                             factorial(new BigDecimal(i));
                          BigDecimal res = BigDecimal.ONE.divide(factorial, 
                                                                 mc);
                          result = result.add(res);
                       }
                       return result;
                    }

                    public BigDecimal factorial(BigDecimal n)
                    {
                       if (n.equals(BigDecimal.ZERO))
                          return BigDecimal.ONE;
                       else
                          return n.multiply(factorial(n.
                                   subtract(BigDecimal.ONE)));
                    }
                 };
      Future<BigDecimal> taskFuture = executor.submit(callable);
      try
      {
         while (!taskFuture.isDone())
            System.out.println("waiting");
         System.out.println(taskFuture.get());
      }
      catch(ExecutionException ee)
      {
         System.err.println("task threw an exception");
         System.err.println(ee);
      }
      catch(InterruptedException ie)
      {
         System.err.println("interrupted while waiting");
      }
      executor.shutdownNow();
   }
}
```

## Chapter 6 : Synchronizers

Java provides the synchronized keyword for synchronizing thread access to critical sections. Because it can be difficult to correctly write synchronized code that’s based on synchronized, high-level synchronizers are included in the concurrency utilities.

A countdown latch causes one or more threads to wait at a “gate” until another thread opens this gate, at which point these other threads can continue. It consists of a count and operations for “causing a thread to wait until the count reaches zero” and “decrementing the count.”

A cyclic barrier lets a set of threads wait for each other to reach a common barrier point. The barrier is cyclic because it can be reused after the waiting threads are released. This synchronizer is useful in applications involving a fixed-size party of threads that must occasionally wait for each other.

An exchanger provides a synchronization point where threads can swap objects. Each thread presents some object on entry to the exchanger’s exchange() method, matches with a partner thread, and receives its partner’s object on return.

A semaphore maintains a set of permits for restricting the number of threads that can access a limited resource. A thread attempting to acquire a permit when no permits are available blocks until some other thread releases a permit.

A phaser is a more flexible cyclic barrier. Like a cyclic barrier, a phaser lets a group of threads wait on a barrier; these threads continue after the last thread arrives. A phaser also offers the equivalent of a barrier action. Unlike a cyclic barrier, which coordinates a fixed number of threads, a phaser can coordinate a variable number of threads, which can register at any time. To implement this capability, a phaser uses phases and phase numbers.

```java
import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class CountDownLatchDemo
{
   final static int NTHREADS = 3;

   public static void main(String[] args)
   {
      final CountDownLatch startSignal = new CountDownLatch(1);
      final CountDownLatch doneSignal = new CountDownLatch(NTHREADS);
      Runnable r = new Runnable()
                   {
                      @Override
                      public void run()
                      {
                         try
                         {
                            report("entered run()");
                            startSignal.await();  // wait until told to ...
                            report("doing work"); // ... proceed
                            Thread.sleep((int) (Math.random() * 1000));
                            doneSignal.countDown(); // reduce count on which
                                                    // main thread is ...
                         }                          // waiting
                         catch (InterruptedException ie)
                         {
                            System.err.println(ie);
                         }
                      }

                      void report(String s)
                      {
                         System.out.println(System.currentTimeMillis() + 
                                            ": " + Thread.currentThread() + 
                                            ": " + s);
                      }
                   };
      ExecutorService executor = Executors.newFixedThreadPool(NTHREADS);
      for (int i = 0; i < NTHREADS; i++)
         executor.execute(r);
      try
      {
         System.out.println("main thread doing something");
         Thread.sleep(1000); // sleep for 1 second
         startSignal.countDown(); // let all threads proceed
         System.out.println("main thread doing something else");
         doneSignal.await(); // wait for all threads to finish
         executor.shutdownNow();
      }
      catch (InterruptedException ie)
      {
         System.err.println(ie);
      }
   }
}
```

```java
import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class CyclicBarrierDemo
{
   public static void main(String[] args)
   {
      float[][] matrix = new float[3][3];
      int counter = 0;
      for (int row = 0; row < matrix.length; row++)
         for (int col = 0; col < matrix[0].length; col++)
         matrix[row][col] = counter++;
      dump(matrix);
      System.out.println();
      Solver solver = new Solver(matrix);
      System.out.println();
      dump(matrix);
   }

   static void dump(float[][] matrix)
   {
      for (int row = 0; row < matrix.length; row++)
      {
         for (int col = 0; col < matrix[0].length; col++)
            System.out.print(matrix[row][col] + " ");
         System.out.println();
      }
   }
}

class Solver 
{
   final int N;
   final float[][] data;
   final CyclicBarrier barrier;

   class Worker implements Runnable 
   {
      int myRow;
      boolean done = false;

      Worker(int row) 
      { 
         myRow = row; 
      }

      boolean done()
      {
         return done;
      }

      void processRow(int myRow)
      {
         System.out.println("Processing row: " + myRow);
         for (int i = 0; i < N; i++)
            data[myRow][i] *= 10;
         done = true;
      }

      @Override
      public void run() 
      {
         while (!done()) 
         {
            processRow(myRow);

            try 
            {
               barrier.await();
            } 
            catch (InterruptedException ie) 
            {
               return;
            } 
            catch (BrokenBarrierException bbe) 
            {
               return;
            }
         }
      }
   }

   public Solver(float[][] matrix) 
   {
      data = matrix;
      N = matrix.length;
      barrier = new CyclicBarrier(N,
                                  new Runnable() 
                                  {
                                     @Override
                                     public void run() 
                                     {
                                        mergeRows();
                                     }
                                  });
      for (int i = 0; i < N; ++i)
         new Thread(new Worker(i)).start();

      waitUntilDone();
   }

   void mergeRows()
   {
      System.out.println("merging");
      synchronized("abc")
      {
         "abc".notify();
      }
   }

   void waitUntilDone()
   {
      synchronized("abc")
      {
         try
         {
            System.out.println("main thread waiting");
            "abc".wait();
            System.out.println("main thread notified");
         }
         catch (InterruptedException ie)
         {
            System.out.println("main thread interrupted");
         }
      }
   }
}
```

```java
import java.util.ArrayList;
import java.util.List;

import java.util.concurrent.Exchanger;

public class ExchangerDemo
{
   final static Exchanger<DataBuffer> exchanger = 
      new Exchanger<DataBuffer>();

   final static DataBuffer initialEmptyBuffer = new DataBuffer();
   final static DataBuffer initialFullBuffer = new DataBuffer("I");

   public static void main(String[] args)
   {
      class FillingLoop implements Runnable 
      {
         int count = 0;

         @Override
         public void run() 
         {
            DataBuffer currentBuffer = initialEmptyBuffer;
            try 
            {
               while (true) 
               {
                  addToBuffer(currentBuffer);
                  if (currentBuffer.isFull())
                  {
                     System.out.println("filling thread wants to exchange");
                     currentBuffer = exchanger.exchange(currentBuffer);
                     System.out.println("filling thread receives exchange");
                  }
               }
            } 
            catch (InterruptedException ie) 
            { 
               System.out.println("filling thread interrupted");
            }
         }

         void addToBuffer(DataBuffer buffer)
         {
            String item = "NI" + count++;
            System.out.println("Adding: " + item);
            buffer.add(item);
         }
      }

      class EmptyingLoop implements Runnable 
      {
         @Override
         public void run() 
         {
            DataBuffer currentBuffer = initialFullBuffer;
            try 
            {
               while (true) 
               {
                  takeFromBuffer(currentBuffer);
                  if (currentBuffer.isEmpty())
                  {
                     System.out.println("emptying thread wants to " +
                                        "exchange");
                     currentBuffer = exchanger.exchange(currentBuffer);
                     System.out.println("emptying thread receives " +
                                        "exchange");
                  }
               }
            } 
            catch (InterruptedException ie) 
            { 
               System.out.println("emptying thread interrupted");
            }
         }

         void takeFromBuffer(DataBuffer buffer)
         {
            System.out.println("taking: " + buffer.remove());
         }
      }
      new Thread(new EmptyingLoop()).start();
      new Thread(new FillingLoop()).start();
   }
}

class DataBuffer
{
   private final static int MAXITEMS = 10;

   private final List<String> items = new ArrayList<>();

   DataBuffer()
   {
   }

   DataBuffer(String prefix)
   {
      for (int i = 0; i < MAXITEMS; i++)
      {
         String item = prefix + i;
         System.out.printf("Adding %s%n", item);
         items.add(item);
      }
   }

   synchronized void add(String s)
   {
      if (!isFull())
         items.add(s);
   }

   synchronized boolean isEmpty()
   {
      return items.size() == 0;
   }

   synchronized boolean isFull()
   {
      return items.size() == MAXITEMS;
   }

   synchronized String remove()
   {
      if (!isEmpty())
         return items.remove(0);
      return null;
   }
}
```

```java
import java.util.ArrayList;
import java.util.List;

import java.util.concurrent.Executors;
import java.util.concurrent.Phaser;

public class PhaserDemo
{
   public static void main(String[] args)
   {
      List<Runnable> tasks = new ArrayList<>();
      tasks.add(() -> System.out.printf("%s running at %d%n", 
                                        Thread.currentThread().getName(),
                                        System.currentTimeMillis()));
      tasks.add(() -> System.out.printf("%s running at %d%n", 
                                        Thread.currentThread().getName(),
                                        System.currentTimeMillis()));
      runTasks(tasks);
   }

   static void runTasks(List<Runnable> tasks) 
   {
      final Phaser phaser = new Phaser(1); // "1" (register self)
      // create and start threads
      for (final Runnable task: tasks) 
      {
         phaser.register();
         Runnable r = () ->
                      {
                         try 
                         { 
                            Thread.sleep(50 + (int) (Math.random() * 300));
                         } 
                         catch (InterruptedException ie) 
                         {
                            System.out.println("interrupted thread");
                         }
                         phaser.arriveAndAwaitAdvance(); // await the ... 
                                                         // creation of ...
                                                         // all tasks
                         task.run();
                      };
         Executors.newSingleThreadExecutor().execute(r);
      }
      // allow threads to start and deregister self
      phaser.arriveAndDeregister();
   }
}
```

```java
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Semaphore;

public class SemaphoreDemo
{
   public static void main(String[] args)
   {
      final Pool pool = new Pool();
      Runnable r = new Runnable()
                   {
                      @Override
                      public void run()
                      {
                         String name = Thread.currentThread().getName();
                         try
                         {
                            while (true)
                            {
                               String item;
                               System.out.println(name + " acquiring " + 
                                                  (item = pool.getItem()));
                               Thread.sleep(200 + 
                                            (int) (Math.random() * 100));
                               System.out.println(name + " putting back " + 
                                                  item);
                               pool.putItem(item);
                            }
                         }
                         catch (InterruptedException ie)
                         {
                            System.out.println(name + "interrupted");
                         }
                      }
                   };
      ExecutorService[] executors = 
         new ExecutorService[Pool.MAX_AVAILABLE + 1];
      for (int i = 0; i < executors.length; i++)
      {
         executors[i] = Executors.newSingleThreadExecutor();
         executors[i].execute(r);
      }
   }
}

final class Pool 
{
   public static final int MAX_AVAILABLE = 10;

   private final Semaphore available = new Semaphore(MAX_AVAILABLE, true);

   private final String[] items;

   private final boolean[] used = new boolean[MAX_AVAILABLE];

   Pool()
   {
      items = new String[MAX_AVAILABLE];
      for (int i = 0; i < items.length; i++)
         items[i] = "I" + i;
   }

   String getItem() throws InterruptedException 
   {
      available.acquire();
      return getNextAvailableItem();
   }

   void putItem(String item) 
   {
      if (markAsUnused(item))
         available.release();
   }

   private synchronized String getNextAvailableItem() 
   {
      for (int i = 0; i < MAX_AVAILABLE; ++i) 
      {
         if (!used[i]) 
         {
            used[i] = true;
            return items[i];
         }
      }
      return null; // not reached
   }

   private synchronized boolean markAsUnused(String item) 
   {
      for (int i = 0; i < MAX_AVAILABLE; ++i) 
      {
         if (item == items[i]) 
         {
            if (used[i]) 
            {
               used[i] = false;
               return true;
            } 
            else
               return false;
         }
      }
      return false;
   }
}
```

## Chapter 7 : The Locking Framework

The java.util.concurrent.locks package provides a framework of interfaces and classes for locking and waiting for conditions in a manner that’s distinct from an object’s intrinsic lock-based synchronization and Object’s wait/notification mechanism. The concurrency utilities include a locking framework that improves on intrinsic synchronization and wait/notification by offering lock polling, timed waits, and more.

The Locking Framework includes the often-used Lock, ReentrantLock, Condition, ReadWriteLock, and ReentrantReadWriteLock types, which I explored in this chapter. I also briefly introduced you to the StampedLock class, which was introduced in Java 8.

```java
import java.util.HashMap;
import java.util.Map;

import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReadWriteLock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class Dictionary
{
   public static void main(String[] args)
   {
      final String[] words =
      {
         "hypocalcemia",
         "prolixity",
         "assiduous",
         "indefatigable",
         "castellan"
      };

      final String[] definitions =
      {
         "a deficiency of calcium in the blood",
         "unduly prolonged or drawn out",
         "showing great care, attention, and effort",
         "able to work or continue for a lengthy time without tiring",
         "the govenor or warden of a castle or fort"
      };

      final Map<String, String> dictionary = new HashMap<String, String>();

      ReadWriteLock rwl = new ReentrantReadWriteLock(true);
      final Lock rlock = rwl.readLock();
      final Lock wlock = rwl.writeLock();

      Runnable writer = () ->
                        {
                           for (int i = 0; i < words.length; i++)
                           {
                              wlock.lock(); 
                              try
                              {
                                 dictionary.put(words[i], 
                                                definitions[i]);
                                 System.out.println("writer storing " + 
                                                    words[i] + " entry");
                              }
                              finally
                              {
                                 wlock.unlock();
                              }

                              try
                              {
                                 Thread.sleep(1);
                              }
                              catch (InterruptedException ie)
                              {
                                 System.err.println("writer " + 
                                                    "interrupted");
                              }
                           }
                        };
      ExecutorService es = Executors.newFixedThreadPool(1);
      es.submit(writer);

      Runnable reader = () ->
                        {
                           while (true)
                           {
                              rlock.lock();
                              try
                              {
                                 int i = (int) (Math.random() * 
                                                words.length);
                                 System.out.println("reader accessing " + 
                                                    words[i] + ": " +
                                                    dictionary.get(words[i]) 
                                                    + " entry");
                              }
                              finally
                              {
                                 rlock.unlock();
                              }
                           }
                        };
      es = Executors.newFixedThreadPool(1);
      es.submit(reader);
   }
}
```

```java
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class PC
{
   public static void main(String[] args)
   {
      Shared s = new Shared();
      new Producer(s).start();
      new Consumer(s).start();
   }
}

class Shared
{
   private char c;

   private volatile boolean available;

   private final Lock lock;

   private final Condition condition;

   Shared()
   {
      available = false;
      lock = new ReentrantLock();
      condition = lock.newCondition();
   }

   Lock getLock()
   {
      return lock;
   }

   char getSharedChar()
   {
      lock.lock();
      try
      {
         while (!available)
            try
            {
               condition.await();
            }
            catch (InterruptedException ie)
            {
               ie.printStackTrace();
            }
         available = false;
         condition.signal();
      }
      finally
      {
         lock.unlock();
         return c;
      }
   }

   void setSharedChar(char c)
   {
      lock.lock();
      try
      {
         while (available)
            try
            {
               condition.await();
            }
            catch (InterruptedException ie)
            {
               ie.printStackTrace();
            }
         this.c = c;
         available = true;
         condition.signal();
      }
      finally
      {
         lock.unlock();
      }
   }
}
class Producer extends Thread
{
   private final Lock l;

   private final Shared s;

   Producer(Shared s)
   {
      this.s = s;
      l = s.getLock();
   }

   @Override
   public void run()
   {
      for (char ch = 'A'; ch <= 'Z'; ch++)
      {
         l.lock();
         s.setSharedChar(ch);
         System.out.println(ch + " produced by producer.");
         l.unlock();
      }
   }
}

class Consumer extends Thread
{
   private final Lock l;

   private final Shared s;

   Consumer(Shared s)
   {
      this.s = s;
      l = s.getLock();
   }

   @Override
   public void run()
   {
      char ch;
      do
      {
         l.lock();
         ch = s.getSharedChar();
         System.out.println(ch + " consumed by consumer.");
         l.unlock();
      }
      while (ch != 'Z');
   }
}
```

```java
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.TimeUnit;

import java.util.concurrent.locks.ReentrantLock;

public class RLDemo
{
   public static void main(String[] args)
   {
      ExecutorService executor = Executors.newFixedThreadPool(2);
      final ReentrantLock lock = new ReentrantLock();

      class Worker implements Runnable
      {
         private final String name;

         Worker(String name)
         {
            this.name = name;
         }

         @Override
         public void run()
         {
           lock.lock();
           try
           {
              if (lock.isHeldByCurrentThread())
                System.out.printf("Thread %s entered critical section.%n", 
                                  name);
              System.out.printf("Thread %s performing work.%n", name);
              try
              {
                 Thread.sleep(2000);
              }
              catch (InterruptedException ie)
              {
                 ie.printStackTrace();
              }
              System.out.printf("Thread %s finished working.%n", name);
           }
           finally
           {
              lock.unlock(); 
           }
         }
      }
      executor.execute(new Worker("ThdA"));
      executor.execute(new Worker("ThdB"));
      try
      {
         executor.awaitTermination(5, TimeUnit.SECONDS);
      }
      catch (InterruptedException ie)
      {
         ie.printStackTrace();
      }
      executor.shutdownNow();
   }
}
```

## Chapter 8 : Additional Concurrency Utilities

This chapter completed my tour of the concurrency utilities by introducing concurrent collections, atomic variables, the Fork/Join Framework, and completion services.

A concurrent collection is a concurrency performant and highly-scalable collections-oriented type that is stored in the java.util.concurrent package. It overcomes the ConcurrentModificationException and performance problems of thread-safe collections.

An atomic variable is an instance of a class that encapsulates a single variable and supports lock-free, thread-safe operations on that variable, for example, AtomicInteger.

The Fork/Join Framework consists of a special executor service and thread pool. The executor service makes a task available to the framework, and this task is broken down into smaller tasks that are forked (executed by different threads) from the pool. A task waits until it’s joined (its subtasks finish).

A completion service is an implementation of the CompletionService<V> interface that decouples the production of new asynchronous tasks (a producer) from the consumption of the results of completed tasks (a consumer). V is the type of a task result.

```java
import java.math.BigDecimal;
import java.math.MathContext;
import java.math.RoundingMode;

import java.util.concurrent.Callable;
import java.util.concurrent.CompletionService;
import java.util.concurrent.ExecutorCompletionService;
import java.util.concurrent.Executors;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Future;

public class CSDemo
{
   public static void main(String[] args) throws Exception
   {
      ExecutorService es = Executors.newFixedThreadPool(10);
      CompletionService<BigDecimal> cs = 
         new ExecutorCompletionService<BigDecimal>(es);
      cs.submit(new CalculateE(17));
      cs.submit(new CalculateE(170));
      Future<BigDecimal> result = cs.take();
      System.out.println(result.get());
      System.out.println();
      result = cs.take();
      System.out.println(result.get());
      es.shutdown();
   }
}

class CalculateE implements Callable<BigDecimal>
{
   final int lastIter;

   public CalculateE(int lastIter)
   {
      this.lastIter = lastIter;
   }

   @Override
   public BigDecimal call()
   {
      MathContext mc = new MathContext(100, RoundingMode.HALF_UP);
      BigDecimal result = BigDecimal.ZERO;
      for (int i = 0; i <= lastIter; i++)
      {
         BigDecimal factorial = factorial(new BigDecimal(i));
         BigDecimal res = BigDecimal.ONE.divide(factorial, mc);
         result = result.add(res);
      }
      return result;
   }

   private BigDecimal factorial(BigDecimal n)
   {
      if (n.equals(BigDecimal.ZERO))
         return BigDecimal.ONE;
      else
         return n.multiply(factorial(n.subtract(BigDecimal.ONE)));
   }
}
```

```java
class ID
{
   private static volatile long nextID = 1;

   static synchronized long getNextID()
   {
      return nextID++;
   }
}
```

```java
import java.util.concurrent.atomic.AtomicLong;

class ID
{
   private static AtomicLong nextID = new AtomicLong(1);

   static long getNextID()
   {
      return nextID.getAndIncrement();
   }
}
```

```java
public class MatMult
{
   public static void main(String[] args)
   {
      Matrix a = new Matrix(1, 3);
      a.setValue(0, 0, 1); // | 1 2 3 |
      a.setValue(0, 1, 2);
      a.setValue(0, 2, 3);
      dump(a);
      Matrix b = new Matrix(3, 2);
      b.setValue(0, 0, 4); // | 4 7 |
      b.setValue(1, 0, 5); // | 5 8 |
      b.setValue(2, 0, 6); // | 6 9 |
      b.setValue(0, 1, 7);
      b.setValue(1, 1, 8);
      b.setValue(2, 1, 9);
      dump(b);
      dump(multiply(a, b));
   }

   public static void dump(Matrix m)
   {
      for (int i = 0; i < m.getRows(); i++)
      {
         for (int j = 0; j < m.getCols(); j++)
            System.out.printf("%d ", m.getValue(i, j));
         System.out.println();
      }
      System.out.println();
   }

   public static Matrix multiply(Matrix a, Matrix b)
   {
      if (a.getCols() != b.getRows())
         throw new IllegalArgumentException("rows/columns mismatch");
      Matrix result = new Matrix(a.getRows(), b.getCols());
      for (int i = 0; i < a.getRows(); i++)
         for (int j = 0; j < b.getCols(); j++)
            for (int k = 0; k < a.getCols(); k++)
               result.setValue(i, j, result.getValue(i, j) + 
                               a.getValue(i, k) * b.getValue(k, j));
      return result;
   }
}
```

```java
public class Matrix
{
   private final int[][] matrix;

   public Matrix(int nrows, int ncols)
   {
      matrix = new int[nrows][ncols];
   }

   public int getCols()
   {
      return matrix[0].length;
   }

   public int getRows()
   {
      return matrix.length;
   }

   public int getValue(int row, int col)
   {
      return matrix[row][col];
   }

   public void setValue(int row, int col, int value)
   {
      matrix[row][col] = value;
   }
}
```

```java
import java.util.ArrayList;
import java.util.List;

import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveAction;

public class MatMult extends RecursiveAction
{
   private final Matrix a, b, c;
   private final int row;

   public MatMult(Matrix a, Matrix b, Matrix c)
   {
      this(a, b, c, -1);
   }

   public MatMult(Matrix a, Matrix b, Matrix c, int row)
   {
      if (a.getCols() != b.getRows())
         throw new IllegalArgumentException("rows/columns mismatch");
      this.a = a;
      this.b = b;
      this.c = c;
      this.row = row;
   }

   @Override
   public void compute()
   {
      if (row == -1)
      {
         List<MatMult> tasks = new ArrayList<>();
         for (int row = 0; row < a.getRows(); row++)
            tasks.add(new MatMult(a, b, c, row));
         invokeAll(tasks);
      }
      else
         multiplyRowByColumn(a, b, c, row);
   }

   public static void multiplyRowByColumn(Matrix a, Matrix b, Matrix c, 
                                          int row)
   {
      for (int j = 0; j < b.getCols(); j++)
         for (int k = 0; k < a.getCols(); k++)
            c.setValue(row, j, c.getValue(row, j) + 
                       a.getValue(row, k) * b.getValue(k, j));
   }

   public static void dump(Matrix m)
   {
      for (int i = 0; i < m.getRows(); i++)
      {
         for (int j = 0; j < m.getCols(); j++)
            System.out.print(m.getValue(i, j) + " ");
         System.out.println();
      }
      System.out.println();
   }

   public static void main(String[] args)
   {
      Matrix a = new Matrix(2, 3);
      a.setValue(0, 0, 1); // | 1 2 3 |
      a.setValue(0, 1, 2); // | 4 5 6 |
      a.setValue(0, 2, 3);
      a.setValue(1, 0, 4);
      a.setValue(1, 1, 5);
      a.setValue(1, 2, 6);
      dump(a);
      Matrix b = new Matrix(3, 2);
      b.setValue(0, 0, 7); // | 7 1 |
      b.setValue(1, 0, 8); // | 8 2 |
      b.setValue(2, 0, 9); // | 9 3 |
      b.setValue(0, 1, 1);
      b.setValue(1, 1, 2);
      b.setValue(2, 1, 3);
      dump(b);
      Matrix c = new Matrix(2, 2);
      ForkJoinPool pool = new ForkJoinPool();
      pool.invoke(new MatMult(a, b, c));
      dump(c);
   }
}
```

```java
public class Matrix
{
   private final int[][] matrix;

   public Matrix(int nrows, int ncols)
   {
      matrix = new int[nrows][ncols];
   }

   public int getCols()
   {
      return matrix[0].length;
   }

   public int getRows()
   {
      return matrix.length;
   }

   public int getValue(int row, int col)
   {
      return matrix[row][col];
   }

   public void setValue(int row, int col, int value)
   {
      matrix[row][col] = value;
   }
}
```

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class PC
{
   public static void main(String[] args)
   {
      final BlockingQueue<Character> bq;
      bq = new ArrayBlockingQueue<Character>(26);
      final ExecutorService executor = Executors.newFixedThreadPool(2);
      Runnable producer = () ->
                          {
                             for (char ch = 'A'; ch <= 'Z'; ch++)
                             {
                                try
                                {
                                   bq.put(ch);
                                   System.out.printf("%c produced by " +
                                                     "producer.%n", ch);
                                }
                                catch (InterruptedException ie)
                                {
                                }
                             }
                          };
      executor.execute(producer);
      Runnable consumer = () ->
                          {
                             char ch = '\0';
                             do
                             {
                                try
                                {
                                   ch = bq.take();
                                   System.out.printf("%c consumed by " +
                                                     "consumer.%n", ch);
                                }
                                catch (InterruptedException ie)
                                {
                                }
                             }
                             while (ch != 'Z');
                             executor.shutdownNow();
                          };
      executor.execute(consumer);
   }
}
```
