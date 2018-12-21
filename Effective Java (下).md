# Effective Java (下)

- [Item 47: Know and use the libraries](#item-47-know-and-use-the-libraries)
- [Item 48: Avoid float and double if exact answers are required](#item-48-avoid-float-and-double-if-exact-answers-are-required)
- [Item 49: Prefer primitive types to boxed primitives](#item-49-prefer-primitive-types-to-boxed-primitives)
- [Item 50: Avoid strings where other types are more appropriate](#item-50-avoid-strings-where-other-types-are-more-appropriate)
- [Item 51: Beware the performance of string concatenation](#item-51-beware-the-performance-of-string-concatenation)
- [Item 52: Refer to objects by their interfaces](#item-52-refer-to-objects-by-their-interfaces)
- [Item 53: Prefer interfaces to reflection](#item-53-prefer-interfaces-to-reflection)
- [Item 54: Use native methods judiciously](#item-54-use-native-methods-judiciously)
- [Item 55: Optimize judiciously](#item-55-optimize-judiciously)
- [Item 56: Adhere to generally accepted naming conventions](#item-56-adhere-to-generally-accepted-naming-conventions)
- [Item 57: Use exceptions only for exceptional conditions](#item-57-use-exceptions-only-for-exceptional-conditions)
- [Item 58: Use checked exceptions for recoverable conditions and runtime exceptions for programming errors](#item-58-use-checked-exceptions-for-recoverable-conditions-and-runtime-exceptions-for-programming-errors)
- [Item 59: Avoid unnecessary use of checked exceptions](#item-59-avoid-unnecessary-use-of-checked-exceptions)
- [Item 60: Favor the use of standard exceptions](#item-60-favor-the-use-of-standard-exceptions)
- [Item 61: Throw exceptions appropriate to the abstraction](#item-61-throw-exceptions-appropriate-to-the-abstraction)
- [Item 62: Document all exceptions thrown by each method](#item-62-document-all-exceptions-thrown-by-each-method)
- [Item 63: Include failure-capture information in detail messages](#item-63-include-failure-capture-information-in-detail-messages)
- [Item 64: Strive for failure atomicity](#item-64-strive-for-failure-atomicity)
- [Item 65: Don’t ignore exceptions](#item-65-dont-ignore-exceptions)
- [Item 66: Synchronize access to shared mutable data](#item-66-synchronize-access-to-shared-mutable-data)
- [Item 67: Avoid excessive synchronization](#item-67-avoid-excessive-synchronization)
- [Item 68: Prefer executors and tasks to threads](#item-68-prefer-executors-and-tasks-to-threads)
- [Item 69: Prefer concurrency utilities to wait and notify](#item-69-prefer-concurrency-utilities-to-wait-and-notify)
- [Item 70: Document thread safety](#item-70-document-thread-safety)
- [Item 71: Use lazy initialization judiciously](#item-71-use-lazy-initialization-judiciously)
- [Item 72: Don’t depend on the thread scheduler](#item-72-dont-depend-on-the-thread-scheduler)
- [Item 73: Avoid thread groups](#item-73-avoid-thread-groups)
- [Item 74: Implement Serializable judiciously](#item-74-implement-serializable-judiciously)
- [Item 75: Consider using a custom serialized form](#item-75-consider-using-a-custom-serialized-form)
- [Item 76: Write readObject methods defensively](#item-76-write-readobject-methods-defensively)
- [Item 78: Consider serialization proxies instead of serialized instances](#item-78-consider-serialization-proxies-instead-of-serialized-instances)
 
## Item 47: Know and use the libraries

```java
private static final Random rnd = new Random();

// Common but deeply flawed!
static int random(int n) {
    return Math.abs(rnd.nextInt()) % n;
}
```

By using a standard library, you take advantage of the knowledge of the experts who wrote it and the experience of those who used it before you.

Numerous features are added to the libraries in every major release, and it pays to keep abreast of these additions.

The libraries are too big to study all the documentation [JavaSE6], but every programmer should be familiar with the contents of java.lang, java.util, and, to a lesser extent, java.io. Knowledge of other libraries can be acquired on an as-needed basis.

## Item 48: Avoid float and double if exact answers are required

The float and double types are particularly ill-suited for monetary calculations because it is impossible to represent 0.1 (or any other negative power of ten) as a float or double exactly.

```java
// Broken - uses floating point for monetary calculation!
public static void main(String[] args) {
    double funds = 1.00;
    int itemsBought = 0;
    for (double price = .10; funds >= price; price += .10) {
        funds -= price;
        itemsBought++;
    }
    System.out.println(itemsBought + " items bought.");
    System.out.println("Change: $" + funds);
}
```

The right way to solve this problem is to use BigDecimal, int, or long for monetary calculations.

## Item 49: Prefer primitive types to boxed primitives

```java
// Broken comparator - can you spot the flaw?
Comparator<Integer> naturalOrder = new Comparator<Integer>() {
    public int compare(Integer first, Integer second) {
        return first < second ? -1 : (first == second ? 0 : 1);
    }
};
```

Applying the  `==`  operator to boxed primitives is almost always wrong.

In nearly every case when you mix primitives and boxed primitives in a single operation, the boxed primitive is auto-unboxed, and this case is no exception.

```java
// Hideously slow program! Can you spot the object creation?
public static void main(String[] args) {
    Long sum = 0L;
    for (long i = 0; i < Integer.MAX_VALUE; i++) {
        sum += i;
    }
    System.out.println(sum);
}
```

Autoboxing reduces the verbosity, but not the danger, of using boxed primitives.

When your program does mixed-type computations involving boxed and unboxed primitives, it does unboxing, and when your program does unboxing, it can throw a NullPointerException

## Item 50: Avoid strings where other types are more appropriate

Strings are poor substitutes for other value types.

Strings are poor substitutes for enum types.

Strings are poor substitutes for aggregate types. 

Strings are poor substitutes for capabilities.

```java
// Broken - inappropriate use of string as capability!
public class ThreadLocal {
    private ThreadLocal() { } // Noninstantiable

    // Sets the current thread's value for the named variable.
    public static void set(String key, Object value);

    // Returns the current thread's value for the named variable.
    public static Object get(String key);
}
```

## Item 51: Beware the performance of string concatenation

Using the string concatenation operator repeatedly to concatenate n strings requires time quadratic in n.

```java
// Inappropriate use of string concatenation - Performs horribly!
public String statement() {
    String result = "";
    for (int i = 0; i < numItems(); i++)
        result += lineForItem(i);  // String concatenation
    return result;
}
```

To achieve acceptable performance, use a StringBuilder in place of a String to store the statement under construction. 

```java
public String statement() {
    StringBuilder b = new StringBuilder(numItems() * LINE_WIDTH);
    for (int i = 0; i < numItems(); i++)
        b.append(lineForItem(i));
    return b.toString();
}
```

## Item 52: Refer to objects by their interfaces

If appropriate interface types exist, then parameters, return values, variables, and fields should all be declared using interface types.

```java
    // Good - uses interface as type
    List<Subscriber> subscribers = new Vector<Subscriber>();
```

```java
 // Bad - uses class as type!
    Vector<Subscriber> subscribers = new Vector<Subscriber>();
```

If you get into the habit of using interfaces as types, your program will be much more flexible.

It is entirely appropriate to refer to an object by a class rather than an interface if no appropriate interface exists.

## Item 53: Prefer interfaces to reflection

- You lose all the benefits of compile-time type checking, including exception checking. If a program attempts to invoke a nonexistent or inaccessible method reflectively, it will fail at runtime unless you’ve taken special precautions.
- The code required to perform reflective access is clumsy and verbose.
- Performance suffers. 

As a rule, objects should not be accessed reflectively in normal applications at runtime.

You can obtain many of the benefits of reflection while incurring few of its costs by using it only in a very limited form.

If this is the case, you can create instances reflectively and access them normally via their interface or superclass. 

```java
// Reflective instantiation with interface access
public static void main(String[] args) {
    // Translate the class name into a Class object
    Class<?> cl = null;
    try {
        cl = Class.forName(args[0]);
    } catch(ClassNotFoundException e) {
        System.err.println("Class not found.");
        System.exit(1);
    }

    // Instantiate the class
    Set<String> s = null;
    try {
        s = (Set<String>) cl.newInstance();
    } catch(IllegalAccessException e) {
        System.err.println("Class not accessible.");
        System.exit(1);
    } catch(InstantiationException e) {
        System.err.println("Class not instantiable.");
        System.exit(1);
    }

    // Exercise the set
    s.addAll(Arrays.asList(args).subList(1, args.length));
    System.out.println(s);
}
```

## Item 54: Use native methods judiciously

It is rarely advisable to use native methods for improved performance.

## Item 55: Optimize judiciously

Strive to write good programs rather than fast ones.

Strive to avoid design decisions that limit performance.

Consider the performance consequences of your API design decisions.

It is a very bad idea to warp an API to achieve good performance.

He could have added one more: measure performance before and after each attempted optimization.

## Item 56: Adhere to generally accepted naming conventions

## Item 57: Use exceptions only for exceptional conditions

```java
// Horrible abuse of exceptions. Don't ever do this!
try {
    int i = 0;
    while(true)
        range[i++].climb();
} catch(ArrayIndexOutOfBoundsException e) {
}
```

The moral of this story is simple: exceptions are, as their name implies, to be used only for exceptional conditions; they should never be used for ordinary control flow.

A well-designed API must not force its clients to use exceptions for ordinary control flow.

```java
// Do not use this hideous code for iteration over a collection!
try {
    Iterator<Foo> i = collection.iterator();
    while(true) {
        Foo foo = i.next();
        ...
    }
} catch (NoSuchElementException e) {
}
```

## Item 58: Use checked exceptions for recoverable conditions and runtime exceptions for programming errors

The cardinal rule in deciding whether to use a checked or an unchecked exception is this: use checked exceptions for conditions from which the caller can reasonably be expected to recover.

Use runtime exceptions to indicate programming errors.

Therefore, all of the unchecked throwables you implement should subclass RuntimeException (directly or indirectly).

## Item 59: Avoid unnecessary use of checked exceptions

```java
// Invocation with checked exception
try {
    obj.action(args);
} catch(TheCheckedException e) {
    // Handle exceptional condition
    ...
}
```

```java
// Invocation with state-testing method and unchecked exception
if (obj.actionPermitted(args)) {
    obj.action(args);
} else {
    // Handle exceptional condition
    ...
}
```

## Item 60: Favor the use of standard exceptions

## Item 61: Throw exceptions appropriate to the abstraction

To avoid this problem, higher layers should catch lower-level exceptions and, in their place, throw exceptions that can be explained in terms of the higher-level abstraction. 

```java
// Exception Translation
try {
    // Use lower-level abstraction to do our bidding
    ...
} catch(LowerLevelException e) {
    throw new HigherLevelException(...);
}
```

```java
// Exception Chaining
try {
    ...  // Use lower-level abstraction to do our bidding
} catch (LowerLevelException cause) {
    throw new HigherLevelException(cause);
}
```

```java
// Exception with chaining-aware constructor
class HigherLevelException extends Exception {
    HigherLevelException(Throwable cause) {
        super(cause);
    }
}
```

While exception translation is superior to mindless propagation of exceptions from lower layers, it should not be overused.

## Item 62: Document all exceptions thrown by each method

Always declare checked exceptions individually, and document precisely the conditions under which each one is thrown using the Javadoc @throws tag.

Use the Javadoc @throws tag to document each unchecked exception that a method can throw, but do not use the throws keyword to include unchecked exceptions in the method declaration.

If an exception is thrown by many methods in a class for the same reason, it is acceptable to document the exception in the class’s documentation comment rather than documenting it individually for each method.

## Item 63: Include failure-capture information in detail messages

To capture the failure, the detail message of an exception should contain the values of all parameters and fields that “contributed to the exception.

```java
/**
 * Construct an IndexOutOfBoundsException.
 *
 * @param lowerBound the lowest legal index value.
 * @param upperBound the highest legal index value plus one.
 * @param index      the actual index value.
 */
public IndexOutOfBoundsException(int lowerBound, int upperBound,
                                 int index) {
    // Generate a detail message that captures the failure
    super("Lower bound: "    + lowerBound +
          ", Upper bound: "  + upperBound +
          ", Index: "        + index);

    // Save failure information for programmatic access
    this.lowerBound = lowerBound;
    this.upperBound = upperBound;
    this.index = index;
}
```

## Item 64: Strive for failure atomicity

Generally speaking, a failed method invocation should leave the object in the state that it was in prior to the invocation.

```java
public Object pop() {
    if (size == 0)
        throw new EmptyStackException();
    Object result = elements[--size];
    elements[size] = null; // Eliminate obsolete reference
    return result;
}
```

## Item 65: Don’t ignore exceptions

```java
// Empty catch block ignores exception - Highly suspect!
try {
    ...
} catch (SomeException e) {
}
```

An empty catch block defeats the purpose of exceptions, which is to force you to handle exceptional conditions.

At the very least, the catch block should contain a comment explaining why it is appropriate to ignore the exception.

## Item 66: Synchronize access to shared mutable data

Synchronization is required for reliable communication between threads as well as for mutual exclusion.

Do not use Thread.stop. 

```java
// Broken! - How long would you expect this program to run?
public class StopThread {
    private static boolean stopRequested;

    public static void main(String[] args)
            throws InterruptedException {
        Thread backgroundThread = new Thread(new Runnable() {
            public void run() {
                int i = 0;
                while (!stopRequested)
                    i++;
            }
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        stopRequested = true;
    }
}
```

```java
// Properly synchronized cooperative thread termination
public class StopThread {
    private static boolean stopRequested;
    private static synchronized void requestStop() {
        stopRequested = true;
    }
    private static synchronized boolean stopRequested() {
        return stopRequested;
    }

    public static void main(String[] args)
            throws InterruptedException {
        Thread backgroundThread = new Thread(new Runnable() {
            public void run() {
                int i = 0;
                while (!stopRequested())
                    i++;
            }
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        requestStop();
    }
}
```

In fact, synchronization has no effect unless both read and write operations are synchronized.

```java
// Cooperative thread termination with a volatile field
public class StopThread {
    private static volatile boolean stopRequested;

    public static void main(String[] args)
            throws InterruptedException {
        Thread backgroundThread = new Thread(new Runnable() {
            public void run() {
                int i = 0;
                while (!stopRequested)
                    i++;
            }
        });
        backgroundThread.start();

        TimeUnit.SECONDS.sleep(1);
        stopRequested = true;
    }
}
```

```java
// Broken - requires synchronization!
private static volatile int nextSerialNumber = 0;

public static int generateSerialNumber() {
    return nextSerialNumber++;
}
```

Either share immutable data, or don’t share at all. In other words, confine mutable data to a single thread.

In summary, when multiple threads share mutable data, each thread that reads or writes the data must perform synchronization.

## Item 67: Avoid excessive synchronization

To avoid liveness and safety failures, never cede control to the client within a synchronized method or block.

```java
// Broken - invokes alien method from synchronized block!
public class ObservableSet<E> extends ForwardingSet<E> {
    public ObservableSet(Set<E> set) { super(set); }

    private final List<SetObserver<E>> observers =
        new ArrayList<SetObserver<E>>();

    public void addObserver(SetObserver<E> observer) {
        synchronized(observers) {
            observers.add(observer);
        }
    }
    public boolean removeObserver(SetObserver<E> observer) {
        synchronized(observers) {
            return observers.remove(observer);
        }
    }
    private void notifyElementAdded(E element) {
        synchronized(observers) {
            for (SetObserver<E> observer : observers)
                observer.added(this, element);
        }
    }
    @Override public boolean add(E element) {
        boolean added = super.add(element);
        if (added)
            notifyElementAdded(element);
        return added;
    }

    @Override public boolean addAll(Collection<? extends E> c) {
        boolean result = false;
        for (E element : c)
            result |= add(element);  // calls notifyElementAdded
        return result;
    }
}
```

```java
// Observer that uses a background thread needlessly
set.addObserver(new SetObserver<Integer>() {
    public void added(final ObservableSet<Integer> s, Integer e) {
        System.out.println(e);
        if (e == 23) {
            ExecutorService executor =
                Executors.newSingleThreadExecutor();
            final SetObserver<Integer> observer = this;
            try {
                executor.submit(new Runnable() {
                    public void run() {
                        s.removeObserver(observer);
                    }
                }).get();
            } catch (ExecutionException ex) {
                throw new AssertionError(ex.getCause());
            } catch (InterruptedException ex) {
                throw new AssertionError(ex);
            } finally {
                executor.shutdown();
            }
        }
    }
});
```

```java
// Alien method moved outside of synchronized block - open calls
private void notifyElementAdded(E element) {
    List<SetObserver<E>> snapshot = null;
    synchronized(observers) {
        snapshot = new ArrayList<SetObserver<E>>(observers);
    }
    for (SetObserver<E> observer : snapshot)
        observer.added(this, element);
}
```

```java
// Thread-safe observable set with CopyOnWriteArrayList
private final List<SetObserver<E>> observers =
    new CopyOnWriteArrayList<SetObserver<E>>();

public void addObserver(SetObserver<E> observer) {
    observers.add(observer);
}
public boolean removeObserver(SetObserver<E> observer) {
    return observers.remove(observer);
}
private void notifyElementAdded(E element) {
    for (SetObserver<E> observer : observers)
        observer.added(this, element);
}
```

As a rule, you should do as little work as possible inside synchronized regions.

## Item 68: Prefer executors and tasks to threads

## Item 69: Prefer concurrency utilities to wait and notify

Given the difficulty of using wait and notify correctly, you should use the higher-level concurrency utilities instead.

Therefore, it is impossible to exclude concurrent activity from a concurrent collection; locking it will have no effect but to slow the program.

```java
// Concurrent canonicalizing map atop ConcurrentMap - not optimal
private static final ConcurrentMap<String, String> map =
    new ConcurrentHashMap<String, String>();

public static String intern(String s) {
    String previousValue = map.putIfAbsent(s, s);
    return previousValue == null ? s : previousValue;
}
```

```java
// Concurrent canonicalizing map atop ConcurrentMap - faster!
public static String intern(String s) {
    String result = map.get(s);
    if (result == null) {
        result = map.putIfAbsent(s, s);
        if (result == null)
            result = s;
    }
    return result;
}
```

```java
// Simple framework for timing concurrent execution
public static long time(Executor executor, int concurrency,
        final Runnable action) throws InterruptedException {
    final CountDownLatch ready = new CountDownLatch(concurrency);
    final CountDownLatch start = new CountDownLatch(1);
    final CountDownLatch done = new CountDownLatch(concurrency);
    for (int i = 0; i < concurrency; i++) {
        executor.execute(new Runnable() {
            public void run() {
                ready.countDown(); // Tell timer we're ready
                try {
                    start.await(); // Wait till peers are ready
                    action.run();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                } finally {
                    done.countDown();  // Tell timer we're done
                }
            }
        });
    }
    ready.await();    // Wait for all workers to be ready
    long startNanos = System.nanoTime();
    start.countDown(); // And they're off!
    done.await();     // Wait for all workers to finish
    return System.nanoTime() - startNanos;
}
```

For interval timing, always use System.nanoTime in preference to System.currentTimeMillis.

```java
// The standard idiom for using the wait method
synchronized (obj) {
    while (<condition does not hold>)
        obj.wait(); // (Releases lock, and reacquires on wakeup)

    ... // Perform action appropriate to condition
}
```

Always use the wait loop idiom to invoke the wait method; never invoke it outside of a loop.

There is seldom, if ever, a reason to use wait and notify in new code.

## Item 70: Document thread safety

The presence of the synchronized modifier in a method declaration is an implementation detail, not a part of its exported API.

To enable safe concurrent use, a class must clearly document what level of thread safety it supports.

```java
// Private lock object idiom - thwarts denial-of-service attack
private final Object lock = new Object();

public void foo() {
    synchronized(lock) {
        ...
    }
}
```

## Item 71: Use lazy initialization judiciously

Under most circumstances, normal initialization is preferable to lazy initialization.

```java
// Normal initialization of an instance field
private final FieldType field = computeFieldValue();
```

If you use lazy initialization to break an initialization circularity, use a synchronized accessor, as it is the simplest, clearest alternative:

```java
// Lazy initialization of instance field - synchronized accessor
private FieldType field;

synchronized FieldType getField() {
    if (field == null)
        field = computeFieldValue();
    return field;
}
```

If you need to use lazy initialization for performance on a static field, use the lazy initialization holder class idiom. 

```java
// Lazy initialization holder class idiom for static fields
private static class FieldHolder {
    static final FieldType field = computeFieldValue();
}
static FieldType getField() { return FieldHolder.field; }
```

If you need to use lazy initialization for performance on an instance field, use the double-check idiom. 

```java
// Double-check idiom for lazy initialization of instance fields
private volatile FieldType field;
FieldType getField() {
    FieldType result = field;
    if (result == null) {  // First check (no locking)
        synchronized(this) {
            result = field;
            if (result == null)  // Second check (with locking)
                field = result = computeFieldValue();
        }
    }
    return result;
}
```

```java
// Single-check idiom - can cause repeated initialization!
private volatile FieldType field;

private FieldType getField() {
    FieldType result = field;
    if (result == null)
        field = result = computeFieldValue();
    return result;
}
```

## Item 72: Don’t depend on the thread scheduler

Any program that relies on the thread scheduler for correctness or performance is likely to be nonportable.

Threads should not run if they aren’t doing useful work.

```java
// Awful CountDownLatch implementation - busy-waits incessantly!
public class SlowCountDownLatch {
    private int count;
    public SlowCountDownLatch(int count) {
        if (count < 0)
            throw new IllegalArgumentException(count + " < 0");
        this.count = count;
    }

    public void await() {
        while (true) {
            synchronized(this) {
                if (count == 0) return;
            }
        }
    }
    public synchronized void countDown() {
        if (count != 0)
            count--;
    }
}
```

When faced with a program that barely works because some threads aren’t getting enough CPU time relative to others, resist the temptation to “fix” the program by putting in calls to Thread.yield.

The same yield invocations that improve performance on one JVM implementation might make it worse on a second and have no effect on a third. Thread.yield has no testable semantics.

Thread priorities are among the least portable features of the Java platform.

## Item 73: Avoid thread groups

While these problems could have been fixed with the addition of new methods, they haven’t, because there is no real need: thread groups are obsolete.

## Item 74: Implement Serializable judiciously

A major cost of implementing Serializable is that it decreases the flexibility to change a class’s implementation once it has been released.

A second cost of implementing Serializable is that it increases the likelihood of bugs and security holes.

A third cost of implementing Serializable is that it increases the testing burden associated with releasing a new version of a class.

Implementing the Serializable interface is not a decision to be undertaken lightly.

Classes designed for inheritance should rarely implement Serializable, and interfaces should rarely extend it.

```java
// readObjectNoData for stateful extendable serializable classes
private void readObjectNoData() throws InvalidObjectException {
    throw new InvalidObjectException("Stream data required");
}
```

Therefore, you should consider providing a parameterless constructor on nonserializable classes designed for inheritance.

```java
// Nonserializable stateful class allowing serializable subclass
public abstract class AbstractFoo {
    private int x, y;  // Our state

    // This enum and field are used to track initialization
    private enum State { NEW, INITIALIZING, INITIALIZED };
    private final AtomicReference<State> init =
        new AtomicReference<State>(State.NEW);

    public AbstractFoo(int x, int y) { initialize(x, y); }

    // This constructor and the following method allow
    // subclass's readObject method to initialize our state.
    protected AbstractFoo() { }

    protected final void initialize(int x, int y) {
        if (!init.compareAndSet(State.NEW, State.INITIALIZING))
            throw new IllegalStateException(
                "Already initialized");
        this.x = x;
        this.y = y;
        ... // Do anything else the original constructor did
        init.set(State.INITIALIZED);
    }

     // These methods provide access to internal state so it can
     // be manually serialized by subclass's writeObject method.
    protected final int getX() { checkInit(); return x; }
    protected final int getY() { checkInit(); return y; }
    // Must call from all public and protected instance methods
    private void checkInit() {
        if (init.get() != State.INITIALIZED)
            throw new IllegalStateException("Uninitialized");
    }
    ...// Remainder omitted
}
```

```java
// Serializable subclass of nonserializable stateful class
public class Foo extends AbstractFoo implements Serializable {
    private void readObject(ObjectInputStream s)
            throws IOException, ClassNotFoundException {
        s.defaultReadObject();

        // Manually deserialize and initialize superclass state
        int x = s.readInt();
        int y = s.readInt();
        initialize(x, y);
    }
    private void writeObject(ObjectOutputStream s)
            throws IOException {
        s.defaultWriteObject();

        // Manually serialize superclass state
        s.writeInt(getX());
        s.writeInt(getY());
    }

    // Constructor does not use the fancy mechanism
    public Foo(int x, int y) { super(x, y); }

    private static final long serialVersionUID = 1856835860954L;
}
```

Therefore, the default serialized form of an inner class is ill-defined.

## Item 75: Consider using a custom serialized form

Do not accept the default serialized form without first considering whether it is appropriate.

The default serialized form is likely to be appropriate if an object’s physical representation is identical to its logical content.

```java
// Good candidate for default serialized form
public class Name implements Serializable {
    /**
     * Last name. Must be non-null.
     * @serial
     */
    private final String lastName;

    /**
     * First name. Must be non-null.
     * @serial
     */
    private final String firstName;

    /**
     * Middle name, or null if there is none.
     * @serial
     */
    private final String middleName;

    ... // Remainder omitted
}
```

Even if you decide that the default serialized form is appropriate, you often must provide a readObject method to ensure invariants and security.

```java
// Awful candidate for default serialized form
public final class StringList implements Serializable {
    private int size = 0;
    private Entry head = null;

    private static class Entry implements Serializable {
        String data;
        Entry  next;
        Entry  previous;
    }

    ... // Remainder omitted
}
```

Using the default serialized form when an object’s physical representation differs substantially from its logical data content has four disadvantages:

- It permanently ties the exported API to the current internal representation.
- It can consume excessive space.
- It can consume excessive time.
- It can cause stack overflows.

```java
// StringList with a reasonable custom serialized form
public final class StringList implements Serializable {
    private transient int size   = 0;
    private transient Entry head = null;

    // No longer Serializable!
    private static class Entry {
        String data;
        Entry  next;
        Entry  previous;
    }

    // Appends the specified string to the list
    public final void add(String s) { ... }

    /**
     * Serialize this {@code StringList} instance.
     *
     * @serialData The size of the list (the number of strings
     * it contains) is emitted ({@code int}), followed by all of
     * its elements (each a {@code String}), in the proper
     * sequence.
     */
    private void writeObject(ObjectOutputStream s)
            throws IOException {
        s.defaultWriteObject();
        s.writeInt(size);

        // Write out all elements in the proper order.
        for (Entry e = head; e != null; e = e.next)
            s.writeObject(e.data);
    }

    private void readObject(ObjectInputStream s)
            throws IOException, ClassNotFoundException {
        s.defaultReadObject();
        int numElements = s.readInt();

        // Read in all elements and insert them in list
        for (int i = 0; i < numElements; i++)
            add((String) s.readObject());
    }

    ... // Remainder omitted
}
```

If all instance fields are transient, it is technically permissible to dispense with invoking defaultWriteObject and defaultReadObject, but it is not recommended.

Before deciding to make a field nontransient, convince yourself that its value is part of the logical state of the object.

Whether or not you use the default serialized form, you must impose any synchronization on object serialization that you would impose on any other method that reads the entire state of the object.

```java
// writeObject for synchronized class with default serialized form
private synchronized void writeObject(ObjectOutputStream s)
        throws IOException {
    s.defaultWriteObject();
}
```

Regardless of what serialized form you choose, declare an explicit serial version UID in every serializable class you write.

## Item 76: Write readObject methods defensively

```java
// Immutable class that uses defensive copying
public final class Period {
    private final Date start;
    private final Date end;

    /**
     * @param  start the beginning of the period
     * @param  end the end of the period; must not precede start
     * @throws IllegalArgumentException if start is after end
     * @throws NullPointerException if start or end is null
     */
    public Period(Date start, Date end) {
        this.start = new Date(start.getTime());
        this.end   = new Date(end.getTime());
        if (this.start.compareTo(this.end) > 0)
            throw new IllegalArgumentException(
                          start + " after " + end);
    }

    public Date start () { return new Date(start.getTime()); }

    public Date end ()   { return new Date(end.getTime()); }

    public String toString() { return start + " - " + end; }

    ... // Remainder omitted
}
```

```java
public class BogusPeriod {
    // Byte stream could not have come from real Period instance!
    private static final byte[] serializedForm = new byte[] {
     (byte)0xac, (byte)0xed, 0x00, 0x05, 0x73, 0x72, 0x00, 0x06,
     0x50, 0x65, 0x72, 0x69, 0x6f, 0x64, 0x40, 0x7e, (byte)0xf8,
     0x2b, 0x4f, 0x46, (byte)0xc0, (byte)0xf4, 0x02, 0x00, 0x02,
     0x4c, 0x00, 0x03, 0x65, 0x6e, 0x64, 0x74, 0x00, 0x10, 0x4c,
     0x6a, 0x61, 0x76, 0x61, 0x2f, 0x75, 0x74, 0x69, 0x6c, 0x2f,
     0x44, 0x61, 0x74, 0x65, 0x3b, 0x4c, 0x00, 0x05, 0x73, 0x74,
     0x61, 0x72, 0x74, 0x71, 0x00, 0x7e, 0x00, 0x01, 0x78, 0x70,
     0x73, 0x72, 0x00, 0x0e, 0x6a, 0x61, 0x76, 0x61, 0x2e, 0x75,
     0x74, 0x69, 0x6c, 0x2e, 0x44, 0x61, 0x74, 0x65, 0x68, 0x6a,
     (byte)0x81, 0x01, 0x4b, 0x59, 0x74, 0x19, 0x03, 0x00, 0x00,
     0x78, 0x70, 0x77, 0x08, 0x00, 0x00, 0x00, 0x66, (byte)0xdf,
     0x6e, 0x1e, 0x00, 0x78, 0x73, 0x71, 0x00, 0x7e, 0x00, 0x03,
     0x77, 0x08, 0x00, 0x00, 0x00, (byte)0xd5, 0x17, 0x69, 0x22,
     0x00, 0x78 };

    public static void main(String[] args) {
        Period p = (Period) deserialize(serializedForm);
        System.out.println(p);
    }

    // Returns the object with the specified serialized form
    private static Object deserialize(byte[] sf) {
        try {
            InputStream is = new ByteArrayInputStream(sf);
            ObjectInputStream ois = new ObjectInputStream(is);
            return ois.readObject();
        } catch (Exception e) {
            throw new IllegalArgumentException(e);
        }
    }
}
```

```java
// readObject method with validity checking
private void readObject(ObjectInputStream s)
        throws IOException, ClassNotFoundException {
    s.defaultReadObject();

    // Check that our invariants are satisfied
    if (start.compareTo(end) > 0)
        throw new InvalidObjectException(start +" after "+ end);
}
```

```java
public class MutablePeriod {
    // A period instance
    public final Period period;

    // period's start field, to which we shouldn't have access
    public final Date start;

    // period's end field, to which we shouldn't have access
    public final Date end;
    public MutablePeriod() {
        try {
            ByteArrayOutputStream bos =
                new ByteArrayOutputStream();
            ObjectOutputStream out =
                new ObjectOutputStream(bos);

            // Serialize a valid Period instance
            out.writeObject(new Period(new Date(), new Date()));

            /*
             * Append rogue "previous object refs" for internal
             * Date fields in Period. For details, see "Java
             * Object Serialization Specification," Section 6.4.
             */
            byte[] ref = { 0x71, 0, 0x7e, 0, 5 }; // Ref #5
            bos.write(ref); // The start field
            ref[4] = 4;     // Ref # 4
            bos.write(ref); // The end field

            // Deserialize Period and "stolen" Date references
            ObjectInputStream in = new ObjectInputStream(
            new ByteArrayInputStream(bos.toByteArray()));
            period = (Period) in.readObject();
            start  = (Date)   in.readObject();
            end    = (Date)   in.readObject();
        } catch (Exception e) {
            throw new AssertionError(e);
        }
    }
}
```

```java
public static void main(String[] args) {
    MutablePeriod mp = new MutablePeriod();
    Period p = mp.period;
    Date pEnd = mp.end;

    // Let's turn back the clock
    pEnd.setYear(78);
    System.out.println(p);

    // Bring back the 60s!
    pEnd.setYear(69);
    System.out.println(p);
}
```

When an object is deserialized, it is critical to defensively copy any field containing an object reference that a client must not possess. 

```java
// readObject method with defensive copying and validity checking
private void readObject(ObjectInputStream s)
        throws IOException, ClassNotFoundException {
    s.defaultReadObject();

    // Defensively copy our mutable components
    start = new Date(start.getTime());
    end   = new Date(end.getTime());

    // Check that our invariants are satisfied
    if (start.compareTo(end) > 0)
        throw new InvalidObjectException(start +" after "+ end);
}
```

Do not use the writeUnshared and readUnshared methods. 

Item 77: For instance control, prefer enum types to readResolve

```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis() { ... }

    public void leaveTheBuilding() { ... }
}
```

```java
// readResolve for instance control - you can do better!
private Object readResolve() {
    // Return the one true Elvis and let the garbage collector
    // take care of the Elvis impersonator.
    return INSTANCE;
}
```

In fact, if you depend on readResolve for instance control, all instance fields with object reference types must be declared transient. 

```java
// Broken singleton - has nontransient object reference field!
public class Elvis implements Serializable {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis() { }

    private String[] favoriteSongs =
        { "Hound Dog", "Heartbreak Hotel" };
    public void printFavorites() {
        System.out.println(Arrays.toString(favoriteSongs));
    }

    private Object readResolve() {
        return INSTANCE;
    }
}
```

```java
public class ElvisStealer implements Serializable {
    static Elvis impersonator;
    private Elvis payload;

    private Object readResolve() {
        // Save a reference to the "unresolved" Elvis instance
        impersonator = payload;

        // Return object of correct type for favoriteSongs field
        return new String[] { "A Fool Such as I" };
    }
    private static final long serialVersionUID = 0;
}
```

```java
public class ElvisImpersonator {
    // Byte stream could not have come from real Elvis instance!
    private static final byte[] serializedForm = new byte[] {
     (byte)0xac, (byte)0xed, 0x00, 0x05, 0x73, 0x72, 0x00, 0x05,
     0x45, 0x6c, 0x76, 0x69, 0x73, (byte)0x84, (byte)0xe6,
     (byte)0x93, 0x33, (byte)0xc3, (byte)0xf4, (byte)0x8b,
     0x32, 0x02, 0x00, 0x01, 0x4c, 0x00, 0x0d, 0x66, 0x61, 0x76,
     0x6f, 0x72, 0x69, 0x74, 0x65, 0x53, 0x6f, 0x6e, 0x67, 0x73,
     0x74, 0x00, 0x12, 0x4c, 0x6a, 0x61, 0x76, 0x61, 0x2f, 0x6c,
     0x61, 0x6e, 0x67, 0x2f, 0x4f, 0x62, 0x6a, 0x65, 0x63, 0x74,
     0x3b, 0x78, 0x70, 0x73, 0x72, 0x00, 0x0c, 0x45, 0x6c, 0x76,
     0x69, 0x73, 0x53, 0x74, 0x65, 0x61, 0x6c, 0x65, 0x72, 0x00,
     0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x02, 0x00, 0x01,
     0x4c, 0x00, 0x07, 0x70, 0x61, 0x79, 0x6c, 0x6f, 0x61, 0x64,
     0x74, 0x00, 0x07, 0x4c, 0x45, 0x6c, 0x76, 0x69, 0x73, 0x3b,
     0x78, 0x70, 0x71, 0x00, 0x7e, 0x00, 0x02
    };
    public static void main(String[] args) {
        // Initializes ElvisStealer.impersonator and returns
        // the real Elvis (which is Elvis.INSTANCE)
        Elvis elvis = (Elvis) deserialize(serializedForm);
        Elvis impersonator = ElvisStealer.impersonator;

        elvis.printFavorites();
        impersonator.printFavorites();
    }
}
```

```java
// Enum singleton - the preferred approach
public enum Elvis {
    INSTANCE;
    private String[] favoriteSongs =
        { "Hound Dog", "Heartbreak Hotel" };
    public void printFavorites() {
        System.out.println(Arrays.toString(favoriteSongs));
    }
}
```

The accessibility of readResolve is significant.

## Item 78: Consider serialization proxies instead of serialized instances

```java
// Serialization proxy for Period class
private static class SerializationProxy implements Serializable {
    private final Date start;
    private final Date end;

    SerializationProxy(Period p) {
        this.start = p.start;
        this.end = p.end;
    }

    private static final long serialVersionUID =
        234098243823485285L; // Any number will do (Item 75)
}
```

```java
// writeReplace method for the serialization proxy pattern
private Object writeReplace() {
    return new SerializationProxy(this);
}
```

```java
// readObject method for the serialization proxy pattern
private void readObject(ObjectInputStream stream)
        throws InvalidObjectException {
    throw new InvalidObjectException("Proxy required");
}
```

```java
// readResolve method for Period.SerializationProxy
private Object readResolve() {
    return new Period(start, end);   // Uses public constructor
}
```

```java
// EnumSet's serialization proxy
private static class SerializationProxy <E extends Enum<E>>
        implements Serializable {
    // The element type of this enum set.
    private final Class<E> elementType;

    // The elements contained in this enum set.
    private final Enum[] elements;

    SerializationProxy(EnumSet<E> set) {
        elementType = set.elementType;
        elements = set.toArray(EMPTY_ENUM_ARRAY);  // (Item 43)
    }

    private Object readResolve() {
        EnumSet<E> result = EnumSet.noneOf(elementType);
        for (Enum e : elements)
            result.add((E)e);
        return result;
    }
    private static final long serialVersionUID =
        362491234563181265L;
}
```


