# Java: 设计模式 Builder

## App


```java

package com.iluwatar.builder;
import com.iluwatar.builder.Hero.Builder;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
/**
 *
 * The intention of the Builder pattern is to find a solution to the telescoping constructor
 * anti-pattern. The telescoping constructor anti-pattern occurs when the increase of object
 * constructor parameter combination leads to an exponential list of constructors. Instead of using
 * numerous constructors, the builder pattern uses another object, a builder, that receives each
 * initialization parameter step by step and then returns the resulting constructed object at once.
 * <p>
 * The Builder pattern has another benefit. It can be used for objects that contain flat data (html
 * code, SQL query, X.509 certificate...), that is to say, data that can't be easily edited. This
 * type of data cannot be edited step by step and must be edited at once. The best way to construct
 * such an object is to use a builder class.
 * <p>
 * In this example we have the Builder pattern variation as described by Joshua Bloch in Effective
 * Java 2nd Edition.
 * <p>
 * We want to build {@link Hero} objects, but its construction is complex because of the many
 * parameters needed. To aid the user we introduce {@link Builder} class. {@link Hero.Builder}
 * takes the minimum parameters to build {@link Hero} object in its constructor. After that
 * additional configuration for the {@link Hero} object can be done using the fluent
 * {@link Builder} interface. When configuration is ready the build method is called to receive
 * the final {@link Hero} object.
 *
 */
public class App {
  private static final Logger LOGGER = LoggerFactory.getLogger(App.class);
  /**
   * Program entry point
   *
   * @param args command line args
   */
  public static void main(String[] args) {
    Hero mage =
        new Hero.Builder(Profession.MAGE, "Riobard").withHairColor(HairColor.BLACK)
            .withWeapon(Weapon.DAGGER).build();
    LOGGER.info(mage.toString());
    Hero warrior =
        new Hero.Builder(Profession.WARRIOR, "Amberjill").withHairColor(HairColor.BLOND)
            .withHairType(HairType.LONG_CURLY).withArmor(Armor.CHAIN_MAIL).withWeapon(Weapon.SWORD)
            .build();
    LOGGER.info(warrior.toString());
    Hero thief =
        new Hero.Builder(Profession.THIEF, "Desmond").withHairType(HairType.BALD)
            .withWeapon(Weapon.BOW).build();
    LOGGER.info(thief.toString());
  }
}
```

## Armor


```java

package com.iluwatar.builder;
/**
 *
 * Armor enumeration
 *
 */
public enum Armor {
  CLOTHES("clothes"), LEATHER("leather"), CHAIN_MAIL("chain mail"), PLATE_MAIL("plate mail");
  private String title;
  Armor(String title) {
    this.title = title;
  }
  @Override
  public String toString() {
    return title;
  }
}
```

## HairColor


```java

package com.iluwatar.builder;
/**
 *
 * HairColor enumeration
 *
 */
public enum HairColor {
  WHITE, BLOND, RED, BROWN, BLACK;
  @Override
  public String toString() {
    return name().toLowerCase();
  }
}
```

## HairType


```java

package com.iluwatar.builder;
/**
 *
 * HairType enumeration
 *
 */
public enum HairType {
  BALD("bald"), SHORT("short"), CURLY("curly"), LONG_STRAIGHT("long straight"), LONG_CURLY(
      "long curly");
  private String title;
  HairType(String title) {
    this.title = title;
  }
  @Override
  public String toString() {
    return title;
  }
}
```

## Hero


```java

package com.iluwatar.builder;
/**
 *
 * Hero, the class with many parameters.
 *
 */
public final class Hero {
  private final Profession profession;
  private final String name;
  private final HairType hairType;
  private final HairColor hairColor;
  private final Armor armor;
  private final Weapon weapon;
  private Hero(Builder builder) {
    this.profession = builder.profession;
    this.name = builder.name;
    this.hairColor = builder.hairColor;
    this.hairType = builder.hairType;
    this.weapon = builder.weapon;
    this.armor = builder.armor;
  }
  public Profession getProfession() {
    return profession;
  }
  public String getName() {
    return name;
  }
  public HairType getHairType() {
    return hairType;
  }
  public HairColor getHairColor() {
    return hairColor;
  }
  public Armor getArmor() {
    return armor;
  }
  public Weapon getWeapon() {
    return weapon;
  }
  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append("This is a ")
            .append(profession)
            .append(" named ")
            .append(name);
    if (hairColor != null || hairType != null) {
      sb.append(" with ");
      if (hairColor != null) {
        sb.append(hairColor).append(' ');
      }
      if (hairType != null) {
        sb.append(hairType).append(' ');
      }
      sb.append(hairType != HairType.BALD ? "hair" : "head");
    }
    if (armor != null) {
      sb.append(" wearing ").append(armor);
    }
    if (weapon != null) {
      sb.append(" and wielding a ").append(weapon);
    }
    sb.append('.');
    return sb.toString();
  }
  /**
   *
   * The builder class.
   *
   */
  public static class Builder {
    private final Profession profession;
    private final String name;
    private HairType hairType;
    private HairColor hairColor;
    private Armor armor;
    private Weapon weapon;
    /**
     * Constructor
     */
    public Builder(Profession profession, String name) {
      if (profession == null || name == null) {
        throw new IllegalArgumentException("profession and name can not be null");
      }
      this.profession = profession;
      this.name = name;
    }
    public Builder withHairType(HairType hairType) {
      this.hairType = hairType;
      return this;
    }
    public Builder withHairColor(HairColor hairColor) {
      this.hairColor = hairColor;
      return this;
    }
    public Builder withArmor(Armor armor) {
      this.armor = armor;
      return this;
    }
    public Builder withWeapon(Weapon weapon) {
      this.weapon = weapon;
      return this;
    }
    public Hero build() {
      return new Hero(this);
    }
  }
}
```

## Profession


```java

package com.iluwatar.builder;
/**
 *
 * Profession enumeration
 *
 */
public enum Profession {
  WARRIOR, THIEF, MAGE, PRIEST;
  @Override
  public String toString() {
    return name().toLowerCase();
  }
}
```

## Weapon


```java

package com.iluwatar.builder;
/**
 *
 * Weapon enumeration
 *
 */
public enum Weapon {
  DAGGER, SWORD, AXE, WARHAMMER, BOW;
  @Override
  public String toString() {
    return name().toLowerCase();
  }
}
```


