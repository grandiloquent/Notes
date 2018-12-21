# Effective Java (上)

- [Item 1: Consider static factory methods instead of constructors](#item-1-consider-static-factory-methods-instead-of-constructors)
- [Item 2: Consider a builder when faced with many constructor parameters](#item-2-consider-a-builder-when-faced-with-many-constructor-parameters)
- [Item 3: Enforce the singleton property with a private constructor or an enum type](#item-3-enforce-the-singleton-property-with-a-private-constructor-or-an-enum-type)
- [Item 4: Enforce noninstantiability with a private constructor](#item-4-enforce-noninstantiability-with-a-private-constructor)
- [Item 5: Avoid creating unnecessary objects](#item-5-avoid-creating-unnecessary-objects)
- [Item 6: Eliminate obsolete object references](#item-6-eliminate-obsolete-object-references)
- [Item 7: Avoid finalizers](#item-7-avoid-finalizers)
- [Item 8: Obey the general contract when overriding equals](#item-8-obey-the-general-contract-when-overriding-equals)
- [Item 9: Always override hashCode when you override equals](#item-9-always-override-hashcode-when-you-override-equals)
- [Item 10: Always override toString](#item-10-always-override-tostring)
- [Item 11: Override clone judiciously](#item-11-override-clone-judiciously)
- [Item 12: Consider implementing Comparable](#item-12-consider-implementing-comparable)
- [Item 13: Minimize the accessibility of classes and members](#item-13-minimize-the-accessibility-of-classes-and-members)
- [Item 14: In public classes, use accessor methods, not public fields](#item-14-in-public-classes-use-accessor-methods-not-public-fields)
- [Item 15: Minimize mutability](#item-15-minimize-mutability)
- [Item 16: Favor composition over inheritance](#item-16-favor-composition-over-inheritance)
- [Item 17: Design and document for inheritance or else prohibit it](#item-17-design-and-document-for-inheritance-or-else-prohibit-it)
- [Item 18: Prefer interfaces to abstract classes](#item-18-prefer-interfaces-to-abstract-classes)
- [Item 19: Use interfaces only to define types](#item-19-use-interfaces-only-to-define-types)
- [Item 20: Prefer class hierarchies to tagged classes](#item-20-prefer-class-hierarchies-to-tagged-classes)

## Item 1: Consider static factory methods instead of constructors

One advantage of static factory methods is that, unlike constructors, they have names. 

A second advantage of static factory methods is that, unlike constructors, they are not required to create a new object each time they’re invoked.

A third advantage of static factory methods is that, unlike constructors, they can return an object of any subtype of their return type. 

```java

// Service interface
public interface Service {
    ... // Service-specific methods go here
}

// Service provider interface
public interface Provider {
    Service newService();
}

// Noninstantiable class for service registration and access
public class Services {
    private Services() { }  // Prevents instantiation (Item 4)

    // Maps service names to services
    private static final Map<String, Provider> providers =
        new ConcurrentHashMap<String, Provider>();
    public static final String DEFAULT_PROVIDER_NAME = "<def>";

    // Provider registration API
    public static void registerDefaultProvider(Provider p) {
        registerProvider(DEFAULT_PROVIDER_NAME, p);
    }
    public static void registerProvider(String name, Provider p){
        providers.put(name, p);
    }

    // Service access API
    public static Service newInstance() {
        return newInstance(DEFAULT_PROVIDER_NAME);
    }
    public static Service newInstance(String name) {
        Provider p = providers.get(name);
        if (p == null)
            throw new IllegalArgumentException(
                "No provider registered with name: " + name);
        return p.newService();
    }
}
```

A fourth advantage of static factory methods is that they reduce the verbosity of creating parameterized type instances.

The main disadvantage of providing only static factory methods is that classes without public or protected constructors cannot be subclassed. 

A second disadvantage of static factory methods is that they are not readily distinguishable from other static methods.

## Item 2: Consider a builder when faced with many constructor parameters

```java
// Telescoping constructor pattern - does not scale well!
public class NutritionFacts {
    private final int servingSize;  // (mL)            required
    private final int servings;     // (per container) required
    private final int calories;     //                 optional
    private final int fat;          // (g)             optional
    private final int sodium;       // (mg)            optional
    private final int carbohydrate; // (g)             optional

    public NutritionFacts(int servingSize, int servings) {
        this(servingSize, servings, 0);
    }

    public NutritionFacts(int servingSize, int servings,
            int calories) {
        this(servingSize, servings, calories, 0);
    }

    public NutritionFacts(int servingSize, int servings,
            int calories, int fat) {
        this(servingSize, servings, calories, fat, 0);
    }

    public NutritionFacts(int servingSize, int servings,
            int calories, int fat, int sodium) {
        this(servingSize, servings, calories, fat, sodium, 0);
    }

    public NutritionFacts(int servingSize, int servings,
           int calories, int fat, int sodium, int carbohydrate) {
        this.servingSize  = servingSize;
        this.servings     = servings;
        this.calories     = calories;
        this.fat          = fat;
        this.sodium       = sodium;
        this.carbohydrate = carbohydrate;
    }
}
```

In short, the telescoping constructor pattern works, but it is hard to write client code when there are many parameters, and harder still to read it.

```java
// JavaBeans Pattern - allows inconsistency, mandates mutability
public class NutritionFacts {
    // Parameters initialized to default values (if any)
    private int servingSize  = -1; // Required; no default value
    private int servings     = -1;  //     "     "      "      "
    private int calories     = 0;
    private int fat          = 0;
    private int sodium       = 0;
    private int carbohydrate = 0;

    public NutritionFacts() { }
    // Setters
    public void setServingSize(int val)  { servingSize = val; }
    public void setServings(int val)     { servings = val; }
    public void setCalories(int val)     { calories = val; }
    public void setFat(int val)          { fat = val; }
    public void setSodium(int val)       { sodium = val; }
    public void setCarbohydrate(int val) { carbohydrate = val; }
}
```

```java
// Builder Pattern
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        // Required parameters
        private final int servingSize;
        private final int servings;

        // Optional parameters - initialized to default values
        private int calories      = 0;
        private int fat           = 0;
        private int carbohydrate  = 0;
        private int sodium        = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings    = servings;
        }

        public Builder calories(int val)
            { calories = val;      return this; }
        public Builder fat(int val)
            { fat = val;           return this; }
        public Builder carbohydrate(int val)
            { carbohydrate = val;  return this; }
        public Builder sodium(int val)
            { sodium = val;        return this; }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize  = builder.servingSize;
        servings     = builder.servings;
        calories     = builder.calories;
        fat          = builder.fat;
        sodium       = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }
}
```

## Item 3: Enforce the singleton property with a private constructor or an enum type

Making a class a singleton can make it difficult to test its clients, as it’s impossible to substitute a mock implementation for a singleton unless it implements an interface that serves as its type.

```java
// Singleton with static factory
public class Elvis {
    private static final Elvis INSTANCE = new Elvis();
    private Elvis() { ... }
    public static Elvis getInstance() { return INSTANCE; }

    public void leaveTheBuilding() { ... }
}
```

```java
// Enum singleton - the preferred approach
public enum Elvis {
    INSTANCE;

    public void leaveTheBuilding() { ... }
}
```

While this approach has yet to be widely adopted, a single-element enum type is the best way to implement a singleton.

## Item 4: Enforce noninstantiability with a private constructor

Attempting to enforce noninstantiability by making a class abstract does not work. 

A default constructor is generated only if a class contains no explicit constructors, so a class can be made noninstantiable by including a private constructor:

```java
// Noninstantiable utility class
public class UtilityClass {
    // Suppress default constructor for noninstantiability
    private UtilityClass() {
        throw new AssertionError();
    }
    ...  // Remainder omitted
}
```

## Item 5: Avoid creating unnecessary objects

```java
String s = new String("stringette");  // DON'T DO THIS!
```

```java
public class Person {
    private final Date birthDate;

    // Other fields, methods, and constructor omitted
    // DON'T DO THIS!
    public boolean isBabyBoomer() {
        // Unnecessary allocation of expensive object
        Calendar gmtCal =
            Calendar.getInstance(TimeZone.getTimeZone("GMT"));
        gmtCal.set(1946, Calendar.JANUARY, 1, 0, 0, 0);
        Date boomStart = gmtCal.getTime();
        gmtCal.set(1965, Calendar.JANUARY, 1, 0, 0, 0);
        Date boomEnd = gmtCal.getTime();
        return birthDate.compareTo(boomStart) >= 0 &&
               birthDate.compareTo(boomEnd)   <  0;
    }
}
```

```java
public class Person {
    private final Date birthDate;
    // Other fields, methods, and constructor omitted

    /**
     * The starting and ending dates of the baby boom.
     */
    private static final Date BOOM_START;
    private static final Date BOOM_END;

    static {
        Calendar gmtCal =
            Calendar.getInstance(TimeZone.getTimeZone("GMT"));
        gmtCal.set(1946, Calendar.JANUARY, 1, 0, 0, 0);
        BOOM_START = gmtCal.getTime();
        gmtCal.set(1965, Calendar.JANUARY, 1, 0, 0, 0);
        BOOM_END = gmtCal.getTime();
    }

    public boolean isBabyBoomer() {
        return birthDate.compareTo(BOOM_START) >= 0 &&
               birthDate.compareTo(BOOM_END)   <  0;
    }
}
```

```java
// Hideously slow program! Can you spot the object creation?
public static void main(String[] args) {
    Long sum = 0L;
    for (long i = 0; i <= Integer.MAX_VALUE; i++) {
        sum += i;
    }
    System.out.println(sum);
}
```

The lesson is clear: prefer primitives to boxed primitives, and watch out for unintentional autoboxing.

## Item 6: Eliminate obsolete object references

```java
// Can you spot the "memory leak"?
public class Stack {
    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack() {
        elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e) {
        ensureCapacity();
        elements[size++] = e;
    }

    public Object pop() {
        if (size == 0)
            throw new EmptyStackException();
        return elements[--size];
    }

    /**
     * Ensure space for at least one more element, roughly
     * doubling the capacity each time the array needs to grow.
     */
    private void ensureCapacity() {
        if (elements.length == size)
            elements = Arrays.copyOf(elements, 2 * size + 1);
    }
}
```

```java
public Object pop() {
    if (size == 0)
        throw new EmptyStackException();
    Object result = elements[--size];
    elements[size] = null; // Eliminate obsolete reference
    return result;
}
```

Nulling out object references should be the exception rather than the norm. 

Generally speaking, whenever a class manages its own memory, the programmer should be alert for memory leaks. 

Another common source of memory leaks is caches. 

A third common source of memory leaks is listeners and other callbacks.

## Item 7: Avoid finalizers

Finalizers are unpredictable, often dangerous, and generally unnecessary. 

This means that you should never do anything time-critical in a finalizer. 

As a consequence, you should never depend on a finalizer to update critical persistent state. 

Oh, and one more thing: there is a severe performance penalty for using finalizers. 

Just provide an explicit termination method, and require clients of the class to invoke this method on each instance when it is no longer needed.

Explicit termination methods are typically used in combination with the try-finally construct to ensure termination. 

```java
// try-finally block guarantees execution of termination method
Foo foo = new Foo(...);
try {
    // Do what must be done with foo
    ...
} finally {
    foo.terminate();  // Explicit termination method
}
```

But the finalizer should log a warning if it finds that the resource has not been terminated, 

```java
// Finalizer Guardian idiom
public class Foo {
   // Sole purpose of this object is to finalize outer Foo object
   private final Object finalizerGuardian = new Object() {
      @Override protected void finalize() throws Throwable {
         ... // Finalize outer Foo object
      }
   };
   ...  // Remainder omitted
}
```

## Item 8: Obey the general contract when overriding equals

Each instance of the class is inherently unique.

You don’t care whether the class provides a “logical equality” test.

A superclass has already overridden equals, and the superclass behavior is appropriate for this class. 

The class is private or package-private, and you are certain that its equals method will never be invoked. 

```java
@Override public boolean equals(Object o) {
    throw new AssertionError(); // Method is never called
}
```

```java
// Broken - violates symmetry!
public final class CaseInsensitiveString {
    private final String s;

    public CaseInsensitiveString(String s) {
        if (s == null)
            throw new NullPointerException();
        this.s = s;
    }

    // Broken - violates symmetry!
    @Override public boolean equals(Object o) {
        if (o instanceof CaseInsensitiveString)
            return s.equalsIgnoreCase(
                ((CaseInsensitiveString) o).s);
        if (o instanceof String)  // One-way interoperability!
            return s.equalsIgnoreCase((String) o);
        return false;
    }
    ...  // Remainder omitted
}
```

Once you’ve violated the equals contract, you simply don’t know how other objects will behave when confronted with your object.

There is no way to extend an instantiable class and add a value component while preserving the equals contract, unless you are willing to forgo the benefits of object-oriented abstraction.

Putting it all together, here’s a recipe for a high-quality equals method:

1. Use the == operator to check if the argument is a reference to this object.
2. Use the instanceof operator to check if the argument has the correct type. 
3. Cast the argument to the correct type. 
4. For each “significant” field in the class, check if that field of the argument matches the corresponding field of this object. 
5. When you are finished writing your equals method, ask yourself three questions: Is it symmetric? Is it transitive? Is it consistent?

Always override hashCode when you override equals.

Don’t try to be too clever. 

Don’t substitute another type for Object in the equals declaration.

## Item 9: Always override hashCode when you override equals

The key provision that is violated when you fail to override hashCode is the second one: equal objects must have equal hash codes. 

```java
// The worst possible legal hash function - never use!
@Override public int hashCode() { return 42; }
```

```java
    @Override public int hashCode() {
        int result = 17;
        result = 31 * result + areaCode;
        result = 31 * result + prefix;
        result = 31 * result + lineNumber;
        return result;
    }
```

```java
// Lazily initialized, cached hashCode
private volatile int hashCode;  // (See Item 71)

@Override public int hashCode() {
    int result = hashCode;
    if (result == 0) {
        result = 17;
        result = 31 * result + areaCode;
        result = 31 * result + prefix;
        result = 31 * result + lineNumber;
        hashCode = result;
    }
    return result;
}
```

Do not be tempted to exclude significant parts of an object from the hash code computation to improve performance.

## Item 10: Always override toString

While it isn’t as important as obeying the equals and hashCode contracts, providing a good toString implementation makes your class much more pleasant to use. 

When practical, the toString method should return all of the interesting information contained in the object, as in the phone number example just shown.

Whether or not you decide to specify the format, you should clearly document your intentions.

Whether or not you specify the format, provide programmatic access to all of the information contained in the value returned by toString. 

## Item 11: Override clone judiciously

Therefore, if you override the clone method in a nonfinal class, you should return an object obtained by invoking super.clone.

In practice, a class that implements Cloneable is expected to provide a properly functioning public clone method.

The general principle at play here is never make the client do anything the library can do for the client.

In effect, the clone method functions as another constructor; you must ensure that it does no harm to the original object and that it properly establishes invariants on the clone. 

This is a fundamental problem: the clone architecture is incompatible with normal use of final fields referring to mutable objects, except in cases where the mutable objects may be safely shared between an object and its clone. 

Otherwise, you are better off providing an alternative means of object copying, or simply not providing the capability. 

A fine approach to object copying is to provide a copy constructor or copy factory. 

```java
public static Yum newInstance(Yum yum);
```

## Item 12: Consider implementing Comparable

```java
public int compareTo(PhoneNumber pn) {
    // Compare area codes
    if (areaCode < pn.areaCode)
        return -1;
    if (areaCode > pn.areaCode)
        return  1;
    // Area codes are equal, compare prefixes
    if (prefix < pn.prefix)
        return -1;
    if (prefix > pn.prefix)
        return  1;

    // Area codes and prefixes are equal, compare line numbers
    if (lineNumber < pn.lineNumber)
        return -1;
    if (lineNumber > pn.lineNumber)
        return  1;

    return 0;  // All fields are equal
}
```

## Item 13: Minimize the accessibility of classes and members

The rule of thumb is simple: make each class or member as inaccessible as possible

- private—. The member is accessible only from the top-level class where it is declared.
- package-private—. The member is accessible from any class in the package where it is declared. Technically known as default access, this is the access level you get if no access modifier is specified.
- protected—. The member is accessible from subclasses of the class where it is declared (subject to a few restrictions [JLS, 6.6.2]) and from any class in the package where it is declared.
- public—. The member is accessible from anywhere.

Instance fields should never be public. 

Also, you give up the ability to take any action when the field is modified, so classes with public mutable fields are not thread-safe. 

Note that a nonzero-length array is always mutable, so it is wrong for a class to have a public static final array field, or an accessor that returns such a field.

```java
// Potential security hole!
public static final Thing[] VALUES =  { ... };
```

## Item 14: In public classes, use accessor methods, not public fields

```java
// Degenerate classes like this should not be public!
class Point {
    public double x;
    public double y;
}
```

```java
// Encapsulation of data by accessor methods and mutators
class Point {
    private double x;
    private double y;

    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }
}
```

Certainly, the hard-liners are correct when it comes to public classes: if a class is accessible outside its package, provide accessor methods, to preserve the flexibility to change the class’s internal representation. 

However, if a class is package-private or is a private nested class, there is nothing inherently wrong with exposing its data fields—assuming they do an adequate job of describing the abstraction provided by the class. 

```java
// Public class with exposed immutable fields - questionable
public final class Time {
    private static final int HOURS_PER_DAY    = 24;
    private static final int MINUTES_PER_HOUR = 60;

    public final int hour;
    public final int minute;

    public Time(int hour, int minute) {
        if (hour < 0 || hour >= HOURS_PER_DAY)
           throw new IllegalArgumentException("Hour: " + hour);
        if (minute < 0 || minute >= MINUTES_PER_HOUR)
           throw new IllegalArgumentException("Min: " + minute);
        this.hour = hour;
        this.minute = minute;
    }
    ... // Remainder omitted
}
```

## Item 15: Minimize mutability

1. Don’t provide any methods that modify the object’s state (known as mutators).
2. Ensure that the class can’t be extended.
3. Make all fields final. 
4. Make all fields private. 
5. Ensure exclusive access to any mutable components.

```java
public final class Complex {
    private final double re;
    private final double im;

    public Complex(double re, double im) {
        this.re = re;
        this.im = im;
    }

    // Accessors with no corresponding mutators
    public double realPart()      { return re; }
    public double imaginaryPart() { return im; }

    public Complex add(Complex c) {
        return new Complex(re + c.re, im + c.im);
    }

    public Complex subtract(Complex c) {
        return new Complex(re - c.re, im - c.im);
    }

    public Complex multiply(Complex c) {
        return new Complex(re * c.re - im * c.im,
                           re * c.im + im * c.re);
    }

    public Complex divide(Complex c) {
        double tmp = c.re * c.re + c.im * c.im;
        return new Complex((re * c.re + im * c.im) / tmp,
                           (im * c.re - re * c.im) / tmp);
    }

    @Override public boolean equals(Object o) {
       if (o == this)
           return true;
       if (!(o instanceof Complex))
           return false;
       Complex c = (Complex) o;

       // See page 43 to find out why we use compare instead of ==
       return Double.compare(re, c.re) == 0 &&
              Double.compare(im, c.im) == 0;
    }

    @Override public int hashCode() {
        int result = 17 + hashDouble(re);
        result = 31 * result + hashDouble(im);
        return result;
    }

    private static int hashDouble(double val) {
        long longBits = Double.doubleToLongBits(val);
        return (int) (longBits ^ (longBits >>> 32));
    }

    @Override public String toString() {
        return "(" + re + " + " + im + "i)";
    }
}
```

Immutable objects are simple. 

Immutable objects are inherently thread-safe; they require no synchronization. 

Therefore, immutable objects can be shared freely. 

Not only can you share immutable objects, but you can share their internals.

Immutable objects make great building blocks for other objects,

The only real disadvantage of immutable classes is that they require a separate object for each distinct value.

```java
// Immutable class with static factories instead of constructors
public class Complex {
    private final double re;
    private final double im;

    private Complex(double re, double im) {
        this.re = re;
        this.im = im;
    }

    public static Complex valueOf(double re, double im) {
        return new Complex(re, im);
    }

    ... // Remainder unchanged
}
```

Classes should be immutable unless there’s a very good reason to make them mutable.

If a class cannot be made immutable, limit its mutability as much as possible. 

Therefore, make every field final unless there is a compelling reason to make it nonfinal.

## Item 16: Favor composition over inheritance

Unlike method invocation, inheritance violates encapsulation. 

```java
// Broken - Inappropriate use of inheritance!
public class InstrumentedHashSet<E> extends HashSet<E> {
    // The number of attempted element insertions
    private int addCount = 0;

    public InstrumentedHashSet() {
    }

    public InstrumentedHashSet(int initCap, float loadFactor) {
        super(initCap, loadFactor);
    }
    @Override public boolean add(E e) {
        addCount++;
        return super.add(e);
    }
    @Override public boolean addAll(Collection<? extends E> c) {
        addCount += c.size();
        return super.addAll(c);
    }
    public int getAddCount() {
        return addCount;
    }
}
```

```java
// Wrapper class - uses composition in place of inheritance
public class InstrumentedSet<E> extends ForwardingSet<E> {
    private int addCount = 0;

    public InstrumentedSet(Set<E> s) {
        super(s);
    }

    @Override public boolean add(E e) {
        addCount++;
        return super.add(e);
    }
    @Override public boolean addAll(Collection<? extends E> c) {
        addCount += c.size();
        return super.addAll(c);
    }
    public int getAddCount() {
        return addCount;
    }
}

// Reusable forwarding class
public class ForwardingSet<E> implements Set<E> {
    private final Set<E> s;
    public ForwardingSet(Set<E> s) { this.s = s; }

    public void clear()               { s.clear();            }
    public boolean contains(Object o) { return s.contains(o); }
    public boolean isEmpty()          { return s.isEmpty();   }
    public int size()                 { return s.size();      }
    public Iterator<E> iterator()     { return s.iterator();  }
    public boolean add(E e)           { return s.add(e);      }
    public boolean remove(Object o)   { return s.remove(o);   }
    public boolean containsAll(Collection<?> c)
                                   { return s.containsAll(c); }
    public boolean addAll(Collection<? extends E> c)
                                   { return s.addAll(c);      }
    public boolean removeAll(Collection<?> c)
                                   { return s.removeAll(c);   }
    public boolean retainAll(Collection<?> c)
                                   { return s.retainAll(c);   }
    public Object[] toArray()          { return s.toArray();  }
    public <T> T[] toArray(T[] a)      { return s.toArray(a); }
    @Override public boolean equals(Object o)
                                       { return s.equals(o);  }
    @Override public int hashCode()    { return s.hashCode(); }
    @Override public String toString() { return s.toString(); }
}
```

## Item 17: Design and document for inheritance or else prohibit it

In other words, the class must document its self-use of overridable methods.

To allow programmers to write efficient subclasses without undue pain, a class may have to provide hooks into its internal workings in the form of judiciously chosen protected methods or, in rare instances, protected fields.

The only way to test a class designed for inheritance is to write subclasses. 

Therefore, you must test your class by writing subclasses before you release it.

Constructors must not invoke overridable methods, directly or indirectly.

```java
public class Super {
    // Broken - constructor invokes an overridable method
    public Super() {
        overrideMe();
    }
    public void overrideMe() {
    }
}
```

If you do decide to implement Cloneable or Serializable in a class designed for inheritance, you should be aware that because the clone and readObject methods behave a lot like constructors, a similar restriction applies: neither clone nor readObject may invoke an overridable method, directly or indirectly. 

By now it should be apparent that designing a class for inheritance places substantial limitations on the class.

The best solution to this problem is to prohibit subclassing in classes that are not designed and documented to be safely subclassed.

## Item 18: Prefer interfaces to abstract classes

Existing classes can be easily retrofitted to implement a new interface. 

Interfaces are ideal for defining mixins. 

Interfaces allow the construction of nonhierarchical type frameworks. 

Interfaces enable safe, powerful functionality enhancements via the wrapper class idiom.

You can combine the virtues of interfaces and abstract classes by providing an abstract skeletal implementation class to go with each nontrivial interface that you export. 

```java
// Concrete implementation built atop skeletal implementation
static List<Integer> intArrayAsList(final int[] a) {
    if (a == null)
        throw new NullPointerException();

    return new AbstractList<Integer>() {
        public Integer get(int i) {
            return a[i];  // Autoboxing (Item 5)
        }

        @Override public Integer set(int i, Integer val) {
            int oldVal = a[i];
            a[i] = val;     // Auto-unboxing
            return oldVal;  // Autoboxing
        }

        public int size() {
            return a.length;
        }
    };
}
```

```java
// Skeletal Implementation
public abstract class AbstractMapEntry<K,V>
        implements Map.Entry<K,V> {
    // Primitive operations
    public abstract K getKey();
    public abstract V getValue();

    // Entries in modifiable maps must override this method
    public V setValue(V value) {
        throw new UnsupportedOperationException();
    }

    // Implements the general contract of Map.Entry.equals
    @Override public boolean equals(Object o) {
        if (o == this)
            return true;
        if (!(o instanceof Map.Entry))
            return false;
        Map.Entry<?,?> arg = (Map.Entry) o;
        return equals(getKey(),   arg.getKey()) &&
               equals(getValue(), arg.getValue());
    }
    private static boolean equals(Object o1, Object o2) {
        return o1 == null ? o2 == null : o1.equals(o2);
    }

    // Implements the general contract of Map.Entry.hashCode
    @Override public int hashCode() {
        return hashCode(getKey()) ^ hashCode(getValue());
    }
    private static int hashCode(Object obj) {
        return obj == null ? 0 : obj.hashCode();
    }
}
```

Using abstract classes to define types that permit multiple implementations has one great advantage over using interfaces: It is far easier to evolve an abstract class than an interface.

Once an interface is released and widely implemented, it is almost impossible to change. 

## Item 19: Use interfaces only to define types

```java
 // Constant interface antipattern - do not use!
public interface PhysicalConstants {
    // Avogadro's number (1/mol)
    static final double AVOGADROS_NUMBER   = 6.02214199e23;

    // Boltzmann constant (J/K)
    static final double BOLTZMANN_CONSTANT = 1.3806503e-23;

    // Mass of the electron (kg)
    static final double ELECTRON_MASS      = 9.10938188e-31;
}
```

The constant interface pattern is a poor use of interfaces.

```java
// Constant utility class
package com.effectivejava.science;

public class PhysicalConstants {
  private PhysicalConstants() { }  // Prevents instantiation

  public static final double AVOGADROS_NUMBER   = 6.02214199e23;
  public static final double BOLTZMANN_CONSTANT = 1.3806503e-23;
  public static final double ELECTRON_MASS     = 9.10938188e-31;
}
```

```java
// Use of static import to avoid qualifying constants
import static com.effectivejava.science.PhysicalConstants.*;

public class Test {
    double atoms(double mols) {
        return AVOGADROS_NUMBER * mols;
    }
    ...
    // Many more uses of PhysicalConstants justify static import
}
```

## Item 20: Prefer class hierarchies to tagged classes

```java
// Tagged class - vastly inferior to a class hierarchy!
class Figure {
    enum Shape { RECTANGLE, CIRCLE };

    // Tag field - the shape of this figure
    final Shape shape;

    // These fields are used only if shape is RECTANGLE
    double length;
    double width;

    // This field is used only if shape is CIRCLE
    double radius;

    // Constructor for circle
    Figure(double radius) {
        shape = Shape.CIRCLE;
        this.radius = radius;
    }

    // Constructor for rectangle
    Figure(double length, double width) {
        shape = Shape.RECTANGLE;
        this.length = length;
        this.width = width;
    }

    double area() {
        switch(shape) {
          case RECTANGLE:
            return length * width;
          case CIRCLE:
            return Math.PI * (radius * radius);
          default:
            throw new AssertionError();
        }
    }
}
```

Finally, the data type of an instance gives no clue as to its flavor. In short, tagged classes are verbose, error-prone, and inefficient.

A tagged class is just a pallid imitation of a class hierarchy.

```java
// Class hierarchy replacement for a tagged class
abstract class Figure {
    abstract double area();
}

class Circle extends Figure {
    final double radius;

    Circle(double radius) { this.radius = radius; }

    double area() { return Math.PI * (radius * radius); }
}

class Rectangle extends Figure {
    final double length;
    final double width;

    Rectangle(double length, double width) {
        this.length = length;
        this.width  = width;
    }
    double area() { return length * width; }
}
```


