#设计模式 
 
 通过正反例提问可以更快掌握
 2024-02-27 16:24
## Simple Factory模式
Simple Factory模式（简单工厂模式）是一种创建型设计模式，它提供了一个创建对象的实例并封装了实例化逻辑的方式，而不需要将对象的创建逻辑暴露给客户端。这种模式主要用于解耦对象的创建和使用，使得对象的创建更加灵活。
### 反例

在简单工厂模式的反例中，我们可能会看到对象的创建逻辑直接暴露在客户端代码中，或者在多个地方重复创建对象的实例。这会导致代码的耦合性增加，难以维护和扩展。
```java
// 产品类
class ProductA {
    public void use() {
        System.out.println("Using Product A");
    }
}

class ProductB {
    public void use() {
        System.out.println("Using Product B");
    }
}

// 客户端代码，直接创建对象实例
public class Client {
    public static void main(String[] args) {
        ProductA productA = new ProductA();
        ProductB productB = new ProductB();
        productA.use();
        productB.use();
    }
}
```

在这个反例中，客户端代码直接创建了 `ProductA` 和 `ProductB` 的实例。这种方式的缺点是，如果我们需要添加新的产品类，我们就需要在客户端代码中添加新的创建逻辑，这违反了开闭原则（对扩展开放，对修改封闭）。

### 正例

在简单工厂模式的正例中，我们创建一个工厂类来封装对象的创建逻辑。客户端代码通过工厂类来获取对象实例，而不需要知道具体的类名。

```java
// 产品接口
interface Product {
    void use();
}

// 产品类
class ProductA implements Product {
    @Override
    public void use() {
        System.out.println("Using Product A");
    }
}

class ProductB implements Product {
    @Override
    public void use() {
        System.out.println("Using Product B");
    }
}

// 工厂类
class SimpleFactory {
    public Product createProduct(String type) {
        if ("A".equals(type)) {
            return new ProductA();
        } else if ("B".equals(type)) {
            return new ProductB();
        } else {
            throw new IllegalArgumentException("Unknown product type");
        }
    }
}

// 客户端代码，通过工厂类创建对象实例
public class Client {
    public static void main(String[] args) {
        SimpleFactory factory = new SimpleFactory();
        Product productA = factory.createProduct("A");
        Product productB = factory.createProduct("B");
        productA.use();
        productB.use();
    }
}
```

在这个正例中，我们定义了一个 `Product` 接口和两个实现类 `ProductA` 和 `ProductB`。`SimpleFactory` 类负责根据传入的类型创建相应的产品实例。客户端代码通过调用 `SimpleFactory` 的 `createProduct` 方法来获取产品实例，而不需要知道具体的类名。这样，如果我们需要添加新的产品类，我们只需要在工厂类中添加新的逻辑，而不需要修改客户端代码，从而实现了代码的解耦。

 了解了，我们可以按照这个顺序来讨论设计模式。首先，我会给出一个模式的反例，并指出其中的问题，然后提供一个改进后的正例来展示如何正确应用该模式。

## 工厂方法模式（Factory Method Pattern）
 
 反例

在工厂方法模式的反例中，我们可能会看到工厂类直接实例化具体产品类，而不是定义一个创建对象的接口或抽象类。这会导致客户端代码与具体产品类紧密耦合，使得系统难以扩展和维护。

```java
// 产品接口
interface Product {
    void use();
}

// 具体产品类A
class ConcreteProductA implements Product {
    public void use() {
        System.out.println("Using ConcreteProductA");
    }
}

// 具体产品类B
class ConcreteProductB implements Product {
    public void use() {
        System.out.println("Using ConcreteProductB");
    }
}

// 工厂类，直接实例化具体产品类
class SimpleFactory {
    public Product createProduct(String type) {
        if ("A".equals(type)) {
            return new ConcreteProductA();
        } else if ("B".equals(type)) {
            return new ConcreteProductB();
        } else {
            throw new IllegalArgumentException("Unknown product type");
        }
    }
}

// 客户端代码，直接使用工厂类创建产品
public class Client {
    public static void main(String[] args) {
        SimpleFactory factory = new SimpleFactory();
        Product productA = factory.createProduct("A");
        Product productB = factory.createProduct("B");
        productA.use();
        productB.use();
    }
}
```

**问题**：在这个反例中，`SimpleFactory` 类直接实例化了`ConcreteProductA`和`ConcreteProductB`。如果我们需要添加新的产品类，就必须修改`SimpleFactory`类，这违反了开闭原则。此外，客户端代码依赖于具体的产品类，这使得代码难以维护。

### 工厂方法模式正例

在工厂方法模式的正例中，我们定义了一个创建对象的接口或抽象类，以及一个具体的工厂类。工厂类实现了创建对象的逻辑，而客户端代码通过接口或抽象类与具体的工厂类解耦。

```java
// 产品接口
interface Product {
    void use();
}

// 具体产品类A
class ConcreteProductA implements Product {
    public void use() {
        System.out.println("Using ConcreteProductA");
    }
}

// 具体产品类B
class ConcreteProductB implements Product {
    public void use() {
        System.out.println("Using ConcreteProductB");
    }
}

// 创建者接口
interface Creator {
    Product createProduct();
}

// 具体工厂类A
class ConcreteCreatorA implements Creator {
    public Product createProduct() {
        return new ConcreteProductA();
    }
}

// 具体工厂类B
class ConcreteCreatorB implements Creator {
    public Product createProduct() {
        return new ConcreteProductB();
    }
}

// 客户端代码，通过创建者接口与具体的工厂类解耦
public class Client {
    public static void main(String[] args) {
        Creator creatorA = new ConcreteCreatorA();
        Product productA = creatorA.createProduct();
        productA.use();

        Creator creatorB = new ConcreteCreatorB();
        Product productB = creatorB.createProduct();
        productB.use();
    }
}
```

**改进**：在这个正例中，我们定义了一个`Creator`接口，它包含一个`createProduct`方法。`ConcreteCreatorA`和`ConcreteCreatorB`是具体的工厂类，它们实现了`Creator`接口并提供了创建产品的逻辑。客户端代码通过`Creator`接口与具体的工厂类解耦，这样即使添加新的产品类，也不需要修改客户端代码。这种方式遵循了开闭原则，并且使得系统更加灵活和可维护。
## 单例模式（Singleton Pattern）

#### 反例

在单例模式的反例中，我们可能会看到全局变量被用来存储单例实例，这可能导致在多线程环境中出现同步问题，或者在不同的类加载器下产生多个实例。

```java
public class Singleton {
    private static Singleton instance;

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

    private Singleton() {
        // 私有构造器
    }
}
```

**问题**：在上述代码中，如果多个线程同时检查`instance`是否为`null`，并且同时进入`if`语句，那么可能会创建多个实例。这违反了单例模式的原则，即一个类只能有一个实例。

#### 正例

为了解决这个问题，我们可以使用双重检查锁定（Double-Check Locking）或者使用静态内部类来实现单例模式。

```java
public class Singleton {
    private static volatile Singleton instance;

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

    private Singleton() {
        // 私有构造器
    }
}
```

或者使用静态内部类：

```java
public class Singleton {
    private Singleton() {
        // 私有构造器
    }

    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

**改进**：在正例中，我们使用了`volatile`关键字来确保`instance`变量的读写操作在多线程环境中是原子的。对于静态内部类的方法，Java保证了`SingletonHolder`类只被加载一次，因此`INSTANCE`只会被创建一次，从而确保了单例的唯一性。这两种方法都解决了反例中的同步问题，并且遵循了单例模式的原则。

## 抽象工厂模式（Abstract Factory Pattern）反例
在抽象工厂模式的反例中，我们可能会看到工厂类负责创建多种不同类型的产品，而这些产品之间没有共同的接口或基类。这会导致工厂类变得庞大且难以维护，因为每次添加新的产品类型都需要修改工厂类。

```java
// 产品接口A
interface ProductA {
    void useA();
}

// 产品接口B
interface ProductB {
    void useB();
}

// 具体产品类A1
class ConcreteProductA1 implements ProductA {
    public void useA() {
        System.out.println("Using ConcreteProductA1");
    }
}

// 具体产品类B1
class ConcreteProductB1 implements ProductB {
    public void useB() {
        System.out.println("Using ConcreteProductB1");
    }
}

// 工厂类，负责创建多种产品
class SimpleFactory {
    public ProductA createProductA() {
        return new ConcreteProductA1();
    }

    public ProductB createProductB() {
        return new ConcreteProductB1();
    }
}

// 客户端代码，直接使用工厂类创建产品
public class Client {
    public static void main(String[] args) {
        SimpleFactory factory = new SimpleFactory();
        ProductA productA = factory.createProductA();
        ProductB productB = factory.createProductB();
        productA.useA();
        productB.useB();
    }
}
```

**问题**：在这个反例中，`SimpleFactory` 类负责创建两种不同类型的产品。随着产品种类的增加，`SimpleFactory` 类将变得越来越复杂，这违反了单一职责原则（SRP）。此外，如果需要添加新的产品类型，就必须修改工厂类，这违反了开闭原则。

### 抽象工厂模式正例

在抽象工厂模式的正例中，我们定义了一个抽象工厂接口，它包含创建相关产品族的方法。每个具体的工厂类实现这个接口，负责创建特定系列的产品。这样，当需要添加新的产品系列时，我们只需要添加新的工厂类和产品类，而不需要修改现有的工厂类。

```java
// 产品接口A
interface ProductA {
    void useA();
}

// 产品接口B
interface ProductB {
    void useB();
}

// 具体产品类A1
class ConcreteProductA1 implements ProductA {
    public void useA() {
        System.out.println("Using ConcreteProductA1");
    }
}

// 具体产品类B1
class ConcreteProductB1 implements ProductB {
    public void useB() {
        System.out.println("Using ConcreteProductB1");
    }
}

// 抽象工厂接口
interface AbstractFactory {
    ProductA createProductA();
    ProductB createProductB();
}

// 具体工厂类A
class ConcreteFactoryA implements AbstractFactory {
    public ProductA createProductA() {
        return new ConcreteProductA1();
    }

    public ProductB createProductB() {
        return new ConcreteProductB1();
    }
}

// 客户端代码，使用抽象工厂接口
public class Client {
    public static void main(String[] args) {
        AbstractFactory factory = new ConcreteFactoryA();
        ProductA productA = factory.createProductA();
        ProductB productB = factory.createProductB();
        productA.useA();
        productB.useB();
    }
}
```

**改进**：在这个正例中，我们定义了一个`AbstractFactory`接口，它规定了创建产品的方法。`ConcreteFactoryA`类实现了这个接口，负责创建`ConcreteProductA1`和`ConcreteProductB1`。客户端代码通过`AbstractFactory`接口与具体的工厂类解耦，这样即使添加新的产品系列，也不需要修改客户端代码。这种方式遵循了开闭原则，并且使得系统更加灵活和可扩展。

## 原型模式（Prototype Pattern）反例

在原型模式的反例中，我们可能会看到对象的复制是通过创建新实例来完成的，而不是通过复制现有对象的状态。这种方法可能会导致性能问题，尤其是在创建复杂对象或大量对象时。

```java
// 产品类
class Product {
    private int state;

    public Product(int state) {
        this.state = state;
    }

    // 复制构造器被省略，对象创建是通过 new 关键字完成的
    public Product() {
        // 默认构造器
    }

    public void setState(int state) {
        this.state = state;
    }

    public int getState() {
        return state;
    }

    // 克隆方法，简单地通过 new 创建新对象
    public Product clone() {
        return new Product(this.state);
    }
}

// 客户端代码，直接使用 clone() 方法复制对象
public class Client {
    public static void main(String[] args) {
        Product original = new Product(10);
        Product copy = original.clone();
        copy.setState(20); // 修改复制对象的状态，原始对象的状态不受影响
        System.out.println("Original state: " + original.getState());
        System.out.println("Copy state: " + copy.getState());
    }
}
```

**问题**：在这个反例中，`Product` 类的 `clone()` 方法简单地通过 `new` 关键字创建了一个新的对象。这种方法在对象状态简单时可能没有问题，但如果对象包含复杂的状态或资源（如文件句柄、数据库连接等），这种方法可能会导致资源浪费和性能问题。此外，如果对象的创建过程复杂或成本高昂，这种方法也不合适。

### 原型模式正例

在原型模式的正例中，我们使用一个原型对象来复制对象，而不是通过创建新实例。这通常通过实现 `java.lang.Cloneable` 接口并重写 `clone()` 方法来实现，确保对象的复制是深拷贝。

```java
// 产品类实现 Cloneable 接口
class Product implements Cloneable {
    private int state;

    public Product(int state) {
        this.state = state;
    }

    // 深拷贝实现
    @Override
    protected Object clone() throws CloneNotSupportedException {
        try {
            // 创建一个新的对象
            Product copy = (Product) super.clone();
            // 复制复杂状态或资源
            // ...
            return copy;
        } catch (CloneNotSupportedException e) {
            throw new AssertionError(); // 不应该发生，因为实现了 Cloneable
        }
    }

    public void setState(int state) {
        this.state = state;
    }

    public int getState() {
        return state;
    }
}

// 客户端代码，使用原型对象进行复制
public class Client {
    public static void main(String[] args) {
        Product original = new Product(10);
        Product copy = null;
        try {
            copy = (Product) original.clone(); // 深拷贝
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        copy.setState(20); // 修改复制对象的状态，原始对象的状态不受影响
        System.out.println("Original state: " + original.getState());
        System.out.println("Copy state: " + copy.getState());
    }
}
```

**改进**：在这个正例中，`Product` 类实现了 `Cloneable` 接口并重写了 `clone()` 方法以实现深拷贝。这意味着复制的对象将拥有原始对象的全部状态，而不仅仅是引用。这种方式适用于对象状态复杂或创建成本高的情况，可以提高性能并减少资源消耗。需要注意的是，深拷贝需要确保所有引用类型的状态也被正确复制。

 ### Builder模式（Builder Pattern）反例

在Builder模式的反例中，我们可能会看到对象的构建过程直接嵌入在类的构造器中，或者在类的方法中直接设置对象的各个部分。这种方式可能导致对象的构建过程难以理解和维护，尤其是在对象有大量组件和配置选项时。

```java
// 产品类，包含多个组件
class Product {
    private String partA;
    private String partB;
    private String partC;

    // 构造器，直接构建对象
    public Product(String partA, String partB, String partC) {
        this.partA = partA;
        this.partA = partA;
        this.partC = partC;
    }

    // 其他方法...
}

// 客户端代码，直接使用构造器创建对象
public class Client {
    public static void main(String[] args) {
        Product product = new Product("PartA", "PartB", "PartC");
        // 使用 product...
    }
}
```

**问题**：在这个反例中，`Product` 类的构造器负责接收所有组件并构建产品。这种方式在组件数量较少时可能看起来简单直接，但如果组件数量增加，构造器的参数列表会变得很长，难以管理和维护。此外，如果某些组件是可选的，构造器的复杂性会增加，因为需要处理默认值或空值。

### Builder模式正例

在Builder模式的正例中，我们引入了一个Builder类，它提供了设置对象各个组件的方法，并返回自身以支持链式调用。最终，Builder类提供了一个构建方法来创建最终的对象。

```java
// 产品接口
interface Product {
    void show();
}

// 具体产品类
class ConcreteProduct implements Product {
    private String partA;
    private String partB;
    private String partC;

    private ConcreteProduct(Builder builder) {
        this.partA = builder.partA;
        this.partB = builder.partB;
        this.partC = builder.partC;
    }

    @Override
    public void show() {
        System.out.println("PartA: " + partA + ", PartB: " + partB + ", PartC: " + partC);
    }

    // Builder内嵌类
    public static class Builder {
        private String partA;
        private String partB;
        private String partC;

        public Builder partA(String partA) {
            this.partA = partA;
            return this;
        }

        public Builder partB(String partB) {
            this.partB = partB;
            return this;
        }

        public Builder partC(String partC) {
            this.partC = partC;
            return this;
        }

        public ConcreteProduct build() {
            return new ConcreteProduct(this);
        }
    }
}

// 客户端代码，使用Builder构建对象
public class Client {
    public static void main(String[] args) {
        ConcreteProduct product = new ConcreteProduct.Builder()
            .partA("PartA")
            .partB("PartB")
            .partC("PartC")
            .build();
        product.show();
    }
}
```

## 代理模式（Proxy Pattern）反例

在代理模式的反例中，我们可能会看到客户端直接与实际对象交互，而不是通过代理。这可能导致实际对象的创建和使用没有适当的控制，例如，没有实现延迟加载、访问控制或增加额外的功能。

```java
// 真实主题类
class RealSubject {
    public void doSomething() {
        System.out.println("RealSubject is doing something.");
    }
}

// 客户端代码，直接使用真实主题
public class Client {
    public static void main(String[] args) {
        RealSubject real = new RealSubject();
        real.doSomething();
    }
}
```

**问题**：在这个反例中，客户端直接创建并使用`RealSubject`对象。这种方式没有提供任何额外的控制或功能，如访问控制、性能优化或对象的预加载。如果`RealSubject`的实例化或操作成本很高，或者需要在访问前进行某些检查，那么这种方式就不合适。

### 代理模式正例

在代理模式的正例中，我们引入了一个代理类，它在客户端和真实主题之间提供了一个中介。代理类可以控制对真实主题的访问，实现延迟加载、缓存、访问控制等。

```java
// 真实主题接口
interface Subject {
    void doSomething();
}

// 真实主题类
class RealSubject implements Subject {
    @Override
    public void doSomething() {
        System.out.println("RealSubject is doing something.");
    }
}

// 代理类
class ProxySubject implements Subject {
    private RealSubject realSubject;

    @Override
    public void doSomething() {
        if (realSubject == null) {
            // 延迟加载真实主题
            realSubject = new RealSubject();
        }
        realSubject.doSomething();
    }

    // 可以添加其他控制逻辑，如访问权限检查
}

// 客户端代码，通过代理与真实主题交互
public class Client {
    public static void main(String[] args) {
        Subject proxy = new ProxySubject();
        proxy.doSomething();
    }
}
```

**改进**：在这个正例中，`ProxySubject`类作为代理，它实现了与`RealSubject`相同的接口。客户端通过`ProxySubject`与真实主题交互。在`doSomething()`方法中，代理可以执行额外的逻辑，例如检查访问权限、实现延迟加载等。这种方式使得我们可以在不修改真实主题代码的情况下，增加额外的功能和控制。


**改进**：在这个正例中，`ConcreteProduct` 类的构建过程被封装在了`Builder`内嵌类中。客户端代码通过调用`Builder`的方法来设置产品的各种组件，最后通过`build()`方法创建最终的产品对象。这种方式使得构建过程更加清晰，易于理解和维护。同时，它允许灵活地设置组件，包括可选组件，因为Builder类可以为这些组件提供默认值。

 外观模式（Facade）、适配器模式（Adapter）、桥接模式（Bridge）和装饰器模式（Decorator）都是结构型设计模式，它们用于处理对象之间的结构关系。尽管它们都与对象的组合和接口转换有关，但它们的目的和应用场景有所不同。

**外观模式（Facade）**：
- 目的：提供一个统一的接口来访问子系统中的多个接口。它隐藏了子系统的复杂性，使得子系统更易于使用。
- 应用场景：当你需要简化复杂系统的接口，或者需要提供一个简单的入口点时使用外观模式。

**适配器模式（Adapter）**：
- 目的：允许不兼容的接口之间进行交互。它通过包装一个类的接口，使其与另一个期望的接口兼容。
- 应用场景：当你需要使用一个已经存在的类，但其接口不符合你的需求时，或者当你需要使原本因接口不兼容而无法一起工作的类可以协同工作时。

**桥接模式（Bridge）**：
- 目的：将抽象与实现分离，使得两者可以独立地变化。它允许在不修改现有代码的情况下，扩展或更换系统的实现。
- 应用场景：当你需要在运行时动态地改变对象的行为，或者当你需要在多个维度上扩展系统时。

**装饰器模式（Decorator）**：
- 目的：动态地给一个对象添加额外的职责，而不会影响从同一个类中派生的其他对象。它通过包装对象来提供额外的功能。
- 应用场景：当你需要给对象添加额外的功能，但又不想通过继承增加子类的数量时，或者当你需要灵活地组合对象的功能时。

**区别**：

- **外观模式**关注的是简化复杂系统的接口，它通常用于客户端与复杂系统之间的交互。
- **适配器模式**关注的是接口的兼容性，它通常用于系统之间的集成，使得原本不兼容的接口能够协同工作。
- **桥接模式**关注的是抽象与实现的分离，它允许在不同的层次上进行扩展，通常用于软件架构设计。
- **装饰器模式**关注的是对象功能的动态扩展，它通常用于在运行时增加对象的行为，而不是在编译时通过继承来实现。

总结来说，外观模式用于简化接口，适配器模式用于接口转换，桥接模式用于解耦抽象与实现，装饰器模式用于动态扩展对象功能。每种模式都有其特定的应用场景和解决的问题。

 外观模式（Facade Pattern）是一种设计模式，它提供了一个统一的接口来访问子系统中的一群接口。外观模式主要用于简化复杂子系统的访问，使得子系统更易于使用。它通过定义一个高层接口来隐藏子系统的复杂性，让子系统与外界的交互更加清晰。

**外观模式的主要角色包括：**

1. **Facade（外观）**：提供一个统一的接口，用于与复杂子系统进行交互。Facade 类通常知道如何调用子系统的哪些部分，以及如何将子系统的操作转换为客户端可以理解的形式。

2. **Subsystem（子系统）**：一系列类和接口，它们共同实现了特定的功能。子系统可以独立于外观模式存在，但客户端通常不直接与子系统交互。

**外观模式的优点：**

- **简化接口**：客户端通过一个简单的接口与复杂子系统交互，而不需要了解子系统的内部结构。
- **解耦**：外观模式将客户端与子系统解耦，子系统的修改不会影响到客户端。
- **易于维护**：子系统的复杂性被封装在外观类中，使得系统更易于维护和扩展。

**外观模式的缺点：**

- **增加抽象**：引入外观类可能会增加系统的抽象层次，这可能会导致系统的理解难度增加。
- **性能开销**：在某些情况下，外观模式可能会引入额外的性能开销，因为所有的请求都要通过外观类。

**外观模式的应用场景：**

- 当你想为复杂子系统提供一个简化的接口时。
- 当你需要减少客户端与复杂子系统之间的依赖时。
- 当你想为一个大型系统提供一个轻量级的访问子系统的方式时。

**示例：**

假设你有一个复杂的图形处理系统，它包含多个类，如 `Shape`、`Color`、`Renderer` 等。客户端代码需要与这些类交互以创建和渲染图形。使用外观模式，你可以创建一个 `GraphicsFacade` 类，它封装了所有与图形处理相关的操作。

```java
// 子系统接口
public class Shape {
    public void draw() {
        // 绘制图形
    }
}

public class Color {
    public void apply() {
        // 应用颜色
    }
}

public class Renderer {
    public void render(Shape shape, Color color) {
        // 渲染图形
    }
}

// 外观类
public class GraphicsFacade {
    private Renderer renderer;
    private Shape shape;
    private Color color;

    public GraphicsFacade() {
        this.renderer = new Renderer();
        this.shape = new Shape();
        this.color = new Color();
    }

    public void drawShape() {
        color.apply();
        renderer.render(shape, color);
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        GraphicsFacade facade = new GraphicsFacade();
        facade.drawShape();
    }
}
```

在这个例子中，`GraphicsFacade` 类提供了一个简单的接口 `drawShape()`，它封装了创建颜色、绘制形状和渲染图形的复杂过程。客户端代码只需要调用这个简单的方法，而不需要直接与子系统类交互。这样，外观模式简化了客户端的使用，并隐藏了子系统的复杂性。

 在实际生产中，代理模式的前置操作和后置操作通常与业务逻辑、性能优化、资源管理、安全性等方面相关。以下是一个更接近商业开发的例子，其中代理类负责连接池管理、日志记录和异常处理。

### 代理模式正例：数据库操作代理

假设我们有一个数据库操作类`DatabaseAccessor`，它负责执行数据库查询和更新。我们将创建一个代理类`DatabaseAccessorProxy`，它在执行数据库操作前后执行连接池管理、日志记录和异常处理。

```java
// 数据库操作接口
interface DatabaseAccessor {
    void executeQuery(String query);
    void executeUpdate(String update);
}

// 真实数据库操作类
class RealDatabaseAccessor implements DatabaseAccessor {
    @Override
    public void executeQuery(String query) {
        // 实际的数据库查询逻辑
        System.out.println("Executing query: " + query);
    }

    @Override
    public void executeUpdate(String update) {
        // 实际的数据库更新逻辑
        System.out.println("Executing update: " + update);
    }
}

// 数据库操作代理类
class DatabaseAccessorProxy implements DatabaseAccessor {
    private DatabaseAccessor realAccessor;
    private ConnectionPool connectionPool;

    public DatabaseAccessorProxy(ConnectionPool pool) {
        this.realAccessor = new RealDatabaseAccessor();
        this.connectionPool = pool;
    }

    // 前置操作：获取数据库连接
    @Override
    public void executeQuery(String query) {
        Connection connection = null;
        try {
            connection = connectionPool.getConnection();
            realAccessor.executeQuery(query);
        } catch (SQLException e) {
            // 日志记录和异常处理
            handleException(e);
        } finally {
            // 后置操作：释放数据库连接
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    handleException(e);
                }
            }
        }
    }

    // 前置操作：获取数据库连接
    @Override
    public void executeUpdate(String update) {
        Connection connection = null;
        try {
            connection = connectionPool.getConnection();
            realAccessor.executeUpdate(update);
        } catch (SQLException e) {
            // 日志记录和异常处理
            handleException(e);
        } finally {
            // 后置操作：释放数据库连接
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    handleException(e);
                }
            }
        }
    }

    // 日志记录和异常处理方法
    private void handleException(SQLException e) {
        // 日志记录异常
        System.err.println("Database operation failed: " + e.getMessage());
        // 这里可以根据需要进行更复杂的异常处理，例如重试逻辑
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        ConnectionPool pool = new ConnectionPool(); // 假设这是一个数据库连接池的实现
        DatabaseAccessor proxy = new DatabaseAccessorProxy(pool);

        // 使用代理执行数据库操作
        proxy.executeQuery("SELECT * FROM users");
        proxy.executeUpdate("UPDATE users SET status = 'active' WHERE id = 1");
    }
}
```

在这个例子中，`DatabaseAccessorProxy`类在执行数据库操作前后管理数据库连接。它使用`ConnectionPool`来获取和释放数据库连接，这是一个常见的性能优化实践，可以减少创建和关闭数据库连接的开销。同时，代理类还处理了可能发生的`SQLException`异常，并进行了日志记录。这样的代理模式使得我们可以在不修改数据库操作逻辑的情况下，为其添加额外的功能，如连接池管理和异常处理。

## 装饰器模式
 了解您的需求，让我们通过一个更实际的例子来展示装饰器模式。在这个例子中，我们将创建一个简单的文本处理系统，其中包含多个装饰器来添加不同的功能，如日志记录、文本加密和文本高亮。

首先，我们定义一个文本处理接口和一个具体实现：

```java
// 文本处理接口
interface TextProcessor {
    void process(String text);
}

// 具体文本处理器
class SimpleTextProcessor implements TextProcessor {
    @Override
    public void process(String text) {
        System.out.println("Processed text: " + text);
    }
}
```

接下来，我们创建一个装饰器基类，它将包含一个文本处理器引用，并实现`TextProcessor`接口：

```java
// 装饰器基类
abstract class TextDecorator implements TextProcessor {
    protected TextProcessor processor;

    public TextDecorator(TextProcessor processor) {
        this.processor = processor;
    }
}
```

现在，我们可以实现具体的装饰器来添加不同的功能：

```java
// 日志记录装饰器
class LoggingDecorator extends TextDecorator {
    public LoggingDecorator(TextProcessor processor) {
        super(processor);
    }

    @Override
    public void process(String text) {
        System.out.println("Before processing: " + text);
        processor.process(text); // 调用被装饰的处理器
        System.out.println("After processing: " + text);
    }
}

// 文本加密装饰器
class EncryptingDecorator extends TextDecorator {
    private String encryptionKey;

    public EncryptingDecorator(TextProcessor processor, String encryptionKey) {
        super(processor);
        this.encryptionKey = encryptionKey;
    }

    @Override
    public void process(String text) {
        String encryptedText = encrypt(text, encryptionKey); // 加密文本
        processor.process(encryptedText); // 调用下一个处理器
    }

    private String encrypt(String text, String key) {
        // 实现文本加密逻辑
        return text.replaceAll(".", "*"); // 这里只是一个简单的示例
    }
}

// 文本高亮装饰器
class HighlightingDecorator extends TextDecorator {
    private String highlightPattern;

    public HighlightingDecorator(TextProcessor processor, String highlightPattern) {
        super(processor);
        this.highlightPattern = highlightPattern;
    }

    @Override
    public void process(String text) {
        String highlightedText = text.replaceAll(highlightPattern, "<span>" + highlightPattern + "</span>");
        processor.process(highlightedText); // 调用下一个处理器
    }
}
```

最后，客户端代码可以组合使用这些装饰器：

```java
public class Client {
    public static void main(String[] args) {
        TextProcessor processor = new SimpleTextProcessor();
        processor = new LoggingDecorator(processor); // 添加日志记录功能
        processor = new EncryptingDecorator(processor, "secret"); // 添加文本加密功能
        processor = new HighlightingDecorator(processor, "Java"); // 添加文本高亮功能

        processor.process("Hello, Java developers!");
    }
}
```

在这个实际的例子中，我们可以看到装饰器模式如何允许我们在运行时动态地添加和组合不同的功能。每个装饰器都封装了一个特定的功能，并且可以通过组合多个装饰器来扩展对象的行为。这种方式使得系统更加灵活，易于维护，并且可以轻松地添加新功能。

## 适配器模式（Adapter Pattern）反例

在适配器模式的反例中，我们可能会看到直接修改已有类的结构或行为来适应新的接口，而不是使用适配器。这可能会导致代码的可维护性和可扩展性降低，因为直接修改类可能会影响其他依赖于该类的代码。

```java
// 目标接口
interface Target {
    void request();
}

// 需要适配的类
class Adaptee {
    public void specificRequest() {
        System.out.println("Adaptee specificRequest");
    }
}

// 客户端代码，直接使用 Adaptee 类
public class Client {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        adaptee.specificRequest(); // 直接调用 Adaptee 的方法
    }
}
```

**问题**：在这个反例中，`Adaptee`类直接被客户端使用，而没有适配到`Target`接口。如果`Adaptee`类的实现需要改变以适应`Target`接口，那么所有依赖于`Adaptee`的客户端代码都可能需要修改，这违反了开闭原则。

### 适配器模式正例

在适配器模式的正例中，我们创建一个适配器类，它包装了`Adaptee`类，并将其方法转换为`Target`接口所需的方法。这样，客户端代码就可以通过`Target`接口与`Adaptee`类交互，而不需要关心其内部实现。

```java
// 目标接口
interface Target {
    void request();
}

// 需要适配的类
class Adaptee {
    public void specificRequest() {
        System.out.println("Adaptee specificRequest");
    }
}

// 适配器类
class Adapter implements Target {
    private Adaptee adaptee;

    public Adapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }

    @Override
    public void request() {
        adaptee.specificRequest(); // 将 Adaptee 的方法转换为 Target 接口的方法
    }
}

// 客户端代码，使用适配器与 Adaptee 类交互
public class Client {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new Adapter(adaptee);
        target.request(); // 客户端通过 Target 接口调用方法
    }
}
```

**改进**：在这个正例中，`Adapter`类实现了`Target`接口，并在内部持有一个`Adaptee`实例。当客户端通过`Target`接口调用`request()`方法时，适配器将调用`Adaptee`的`specificRequest()`方法。这种方式使得客户端代码与`Adaptee`类的实现细节解耦，提高了系统的可维护性和可扩展性。如果需要更改`Adaptee`类的实现，只需要更新适配器类，而不需要修改客户端代码。
 在真实开发中，适配器模式常用于系统集成、库兼容性、API适配等场景。以下是一个接近商业开发的例子，其中适配器模式用于将一个第三方库的API适配到我们系统所需的接口。

### 真实开发实例：第三方库API适配

假设我们有一个第三方库`ThirdPartyLibrary`，它提供了一个用于发送HTTP请求的方法，但是它的API与我们系统的`HttpClient`接口不兼容。我们需要创建一个适配器来桥接这两个接口。

首先，我们定义系统所需的`HttpClient`接口：

```java
// 系统所需的HttpClient接口
public interface HttpClient {
    void sendGetRequest(String url, ResponseHandler handler);
}
```

然后，我们有一个第三方库的API：

```java
// 第三方库的API
public class ThirdPartyLibrary {
    public void sendRequest(String method, String url, Callback callback) {
        // 第三方库的HTTP请求逻辑
    }
}

// 第三方库的回调接口
public interface Callback {
    void onComplete(String response);
    void onError(Exception e);
}
```

现在，我们需要创建一个适配器来适配这两个API：

```java
// 适配器类
public class ThirdPartyAdapter implements HttpClient {
    private ThirdPartyLibrary library;

    public ThirdPartyAdapter() {
        this.library = new ThirdPartyLibrary();
    }

    @Override
    public void sendGetRequest(String url, ResponseHandler handler) {
        library.sendRequest("GET", url, new Callback() {
            @Override
            public void onComplete(String response) {
                handler.onSuccess(response); // 调用系统响应处理逻辑
            }

            @Override
            public void onError(Exception e) {
                handler.onError(e); // 调用系统错误处理逻辑
            }
        });
    }
}
```

最后，客户端代码可以使用`HttpClient`接口而不需要关心第三方库的具体实现：

```java
// 客户端代码
public class Client {
    public static void main(String[] args) {
        HttpClient client = new ThirdPartyAdapter();
        client.sendGetRequest("http://example.com/api/data", new ResponseHandler() {
            @Override
            public void onSuccess(String response) {
                System.out.println("Received data: " + response);
            }

            @Override
            public void onError(Exception e) {
                System.err.println("Error occurred: " + e.getMessage());
            }
        });
    }
}

// 系统响应处理接口
public interface ResponseHandler {
    void onSuccess(String response);
    void onError(Exception e);
}
```

在这个例子中，`ThirdPartyAdapter`类作为适配器，它将第三方库的API适配到我们系统的`HttpClient`接口。这样，客户端代码就可以通过我们定义的接口与第三方库交互，而不需要直接使用第三方库的API。这种方式提高了系统的可维护性和可扩展性，因为如果第三方库的API发生变化，我们只需要更新适配器，而不需要修改使用`HttpClient`接口的客户端代码。

## 外观模式（Facade Pattern）反例

在外观模式的反例中，客户端代码可能会直接与多个子系统组件交互，这会导致客户端代码与子系统的实现细节紧密耦合，使得系统难以维护和扩展。

```java
// 子系统组件A
class SubsystemA {
    public void operationA() {
        System.out.println("Subsystem A - Operation A");
    }
}

// 子系统组件B
class SubsystemB {
    public void operationB() {
        System.out.println("Subsystem B - Operation B");
    }
}

// 客户端代码，直接与子系统组件交互
public class Client {
    public static void main(String[] args) {
        SubsystemA subsystemA = new SubsystemA();
        SubsystemB subsystemB = new SubsystemB();

        subsystemA.operationA();
        subsystemB.operationB();
    }
}
```

**问题**：在这个反例中，客户端直接创建并操作了`SubsystemA`和`SubsystemB`。如果子系统变得更加复杂，或者需要添加新的子系统，客户端代码将变得难以管理，因为每个子系统的变化都可能影响客户端。

### 外观模式正例

在外观模式的正例中，我们创建一个外观类（Facade）来提供一个统一的接口，客户端通过这个接口与子系统交互。这样，客户端代码与子系统的实现细节解耦，使得系统更加易于维护和扩展。

```java
// 子系统组件A
class SubsystemA {
    public void operationA() {
        System.out.println("Subsystem A - Operation A");
    }
}

// 子系统组件B
class SubsystemB {
    public void operationB() {
        System.out.println("Subsystem B - Operation B");
    }
}

// 外观类
class Facade {
    private SubsystemA subsystemA;
    private SubsystemB subsystemB;

    public Facade() {
        this.subsystemA = new SubsystemA();
        this.subsystemB = new SubsystemB();
    }

    public void operation() {
        subsystemA.operationA();
        subsystemB.operationB();
    }
}

// 客户端代码，通过外观类与子系统交互
public class Client {
    public static void main(String[] args) {
        Facade facade = new Facade();
        facade.operation(); // 客户端通过外观类调用操作
    }
}
```

**关键注释**：

- `SubsystemA` 和 `SubsystemB` 是子系统的具体组件，它们提供了系统的部分功能。
- `Facade` 类作为外观，它封装了对子系统组件的调用。客户端不需要知道子系统的具体实现，只需要与外观类交互。
- 在 `Facade` 类的构造器中，我们创建了子系统组件的实例。这样，客户端不需要直接与子系统组件交互。
- `operation` 方法是外观类提供的方法，它协调子系统组件的操作。客户端通过调用这个方法来执行一系列操作，而不需要关心具体的实现细节。

在这个正例中，外观模式使得客户端代码与子系统的实现细节解耦，提高了系统的可维护性和可扩展性。如果需要添加新的子系统或修改现有子系统，只需要更新外观类，而不需要修改客户端代码。

 了解，让我们通过一个真实的开发实例来展示外观模式。在这个例子中，我们将模拟一个家庭影院系统，它包含多个组件，如DVD播放器、立体声系统和屏幕。我们将创建一个外观类来简化这些组件的操作。

```java
// 家庭影院系统的各个组件
class DvdPlayer {
    public void on() {
        System.out.println("DVD Player is on");
    }

    public void off() {
        System.out.println("DVD Player is off");
    }

    public void play(String movie) {
        System.out.println("Playing: " + movie);
    }
}

class StereoSystem {
    public void on() {
        System.out.println("Stereo System is on");
    }

    public void off() {
        System.out.println("Stereo System is off");
    }

    public void setVolume(int volume) {
        System.out.println("Volume set to: " + volume);
    }
}

class Projector {
    public void on() {
        System.out.println("Projector is on");
    }

    public void off() {
        System.out.println("Projector is off");
    }

    public void wideScreenMode() {
        System.out.println("Wide screen mode is on");
    }
}

// 家庭影院外观类
class HomeTheaterFacade {
    private DvdPlayer dvdPlayer;
    private StereoSystem stereoSystem;
    private Projector projector;

    public HomeTheaterFacade() {
        this.dvdPlayer = new DvdPlayer();
        this.stereoSystem = new StereoSystem();
        this.projector = new Projector();
    }

    // 打开影院系统
    public void watchMovie(String movie) {
        System.out.println("Get ready to watch a movie...");
        stereoSystem.on();
        projector.on();
        projector.wideScreenMode();
        dvdPlayer.on();
        dvdPlayer.play(movie);
    }

    // 关闭影院系统
    public void endMovie() {
        System.out.println("Shutting down theater...");
        dvdPlayer.off();
        projector.off();
        stereoSystem.off();
    }

    // 设置音量
    public void setVolume(int volume) {
        stereoSystem.setVolume(volume);
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        HomeTheaterFacade homeTheater = new HomeTheaterFacade();

        // 观看电影
        homeTheater.watchMovie("Inception");
        // 设置音量
        homeTheater.setVolume(5);
        // 结束电影
        homeTheater.endMovie();
    }
}
```

在这个真实实例中，`HomeTheaterFacade` 类作为外观，它封装了家庭影院系统的复杂操作。客户端代码通过与外观类交互，可以轻松地打开和关闭影院系统，而不需要知道每个组件的具体实现细节。这种方式使得家庭影院系统的使用变得更加简单和直观，同时也便于未来的维护和扩展。如果需要添加新的影院设备或更改现有设备的行为，只需更新外观类即可，无需修改客户端代码。

## Flyweight模式反例

在Flyweight模式的反例中，我们可能会看到每个对象都包含完整的数据，即使这些数据在多个对象之间是共享的。这会导致内存浪费和性能问题，因为相同的数据会被多次存储。

```java
// 共享数据的类
class SharedData {
    private String intrinsicState;

    public SharedData(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    public String getIntrinsicState() {
        return intrinsicState;
    }
}

// 客户端代码，创建多个包含共享数据的对象
public class Client {
    public static void main(String[] args) {
        SharedData data1 = new SharedData("State1");
        SharedData data2 = new SharedData("State2");
        // ... 创建更多对象
    }
}
```

**问题**：在这个反例中，每个`SharedData`对象都包含了自己的`intrinsicState`。如果`intrinsicState`在多个对象之间是共享的，那么这种方式会导致不必要的内存占用，因为相同的状态会被多次复制。

### Flyweight模式正例

在Flyweight模式的正例中，我们通过共享不可变数据（内在状态）来减少内存占用。我们创建一个外部的存储结构（通常是通过一个工厂类）来管理共享数据，并确保每个对象只包含不共享的特定数据（外在状态）。

```java
// 共享数据的接口
interface SharedData {
    String getState(String intrinsicState);
}

// 共享数据的实现，通常由工厂类管理
class ConcreteSharedData implements SharedData {
    private String intrinsicState;

    public ConcreteSharedData(String intrinsicState) {
        this.intrinsicState = intrinsicState;
    }

    @Override
    public String getState(String intrinsicState) {
        // 这里可以有更复杂的逻辑来确定状态
        return intrinsicState;
    }
}

// 工厂类，用于管理共享数据
class SharedDataFactory {
    private Map<String, SharedData> sharedDataMap = new HashMap<>();

    public SharedData getSharedData(String intrinsicState) {
        SharedData sharedData = sharedDataMap.get(intrinsicState);
        if (sharedData == null) {
            sharedData = new ConcreteSharedData(intrinsicState);
            sharedDataMap.put(intrinsicState, sharedData);
        }
        return sharedData;
    }
}

// 使用共享数据的对象
class FlyweightObject {
    private SharedData sharedData;
    private String extrinsicState; // 外在状态

    public FlyweightObject(SharedDataFactory factory, String intrinsicState, String extrinsicState) {
        this.sharedData = factory.getSharedData(intrinsicState);
        this.extrinsicState = extrinsicState;
    }

    public void doOperation() {
        // 使用共享数据和外在状态执行操作
        System.out.println("Intrinsic State: " + sharedData.getState(sharedData.getIntrinsicState()));
        System.out.println("Extrinsic State: " + extrinsicState);
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        SharedDataFactory factory = new SharedDataFactory();
        FlyweightObject object1 = new FlyweightObject(factory, "State1", "Extrinsic1");
        FlyweightObject object2 = new FlyweightObject(factory, "State1", "Extrinsic2");
        // ... 创建更多对象

        object1.doOperation();
        object2.doOperation();
    }
}
```

**关键注释**：

- `SharedData`接口定义了共享数据的获取方式。
- `ConcreteSharedData`实现了`SharedData`接口，它包含了不可变的内在状态。
- `SharedDataFactory`工厂类负责管理共享数据的实例，确保相同的内在状态只被创建一次。
- `FlyweightObject`类使用共享数据和特定的外在状态。它通过工厂类获取共享数据实例。
- 客户端代码通过工厂类创建`FlyweightObject`实例，而不是直接创建共享数据实例。

在这个正例中，Flyweight模式通过共享不可变的数据来减少内存占用，同时保持了对象的独立性和可重用性。这种方式提高了系统的性能和效率，尤其是在处理大量相似对象时。
 在真实开发中，Flyweight模式常用于处理大量对象的场景，尤其是当这些对象的状态可以被细分为共享的内在状态和独立的外在状态时。以下是一个真实的例子，它展示了如何在一个游戏中使用Flyweight模式来优化内存使用。

### 真实实例：游戏中的精灵（Sprite）管理

在游戏开发中，我们经常需要创建大量的精灵（如敌人、道具、角色等）。这些精灵可能有很多共享的属性（如纹理、动画等），但每个精灵也有自己独特的位置、速度等属性。

首先，我们定义一个精灵的接口和具体的实现类：

```java
// 精灵接口
interface Sprite {
    void draw(int x, int y); // 绘制精灵在指定位置
}

// 精灵的具体实现
class GameSprite implements Sprite {
    private String texture; // 精灵的纹理，共享数据
    private int x, y; // 精灵的位置，独立数据

    public GameSprite(String texture) {
        this.texture = texture;
    }

    @Override
    public void draw(int x, int y) {
        // 绘制精灵的逻辑，使用共享的纹理和独立的位置
        System.out.println("Drawing " + texture + " at (" + x + ", " + y + ")");
    }
}
```

接下来，我们创建一个Flyweight工厂来管理共享的纹理：

```java
// 精灵工厂
class SpriteFactory {
    private Map<String, Sprite> sprites = new HashMap<>();

    public Sprite getSprite(String texture) {
        if (!sprites.containsKey(texture)) {
            sprites.put(texture, new GameSprite(texture));
        }
        return sprites.get(texture);
    }
}
```

现在，我们可以在游戏的渲染循环中使用这个工厂来创建和管理精灵：

```java
// 游戏渲染循环
public class GameLoop {
    private SpriteFactory spriteFactory;

    public GameLoop() {
        spriteFactory = new SpriteFactory();
    }

    public void renderSprites() {
        // 创建多个精灵实例，共享相同的纹理
        Sprite sprite1 = spriteFactory.getSprite("enemy.png");
        Sprite sprite2 = spriteFactory.getSprite("enemy.png");
        // ...

        // 绘制精灵
        sprite1.draw(100, 100);
        sprite2.draw(200, 200);
        // ...
    }
}
```

在这个例子中，`SpriteFactory`确保了相同的纹理只被创建一次，并且所有精灵共享这个纹理。这样，即使游戏中有成千上万的敌人，我们也只需要加载一次纹理，从而大大减少了内存的使用。同时，每个精灵的位置是独立的，这允许我们在不同的位置上绘制相同的纹理。

**关键注释**：

- `GameSprite`类实现了`Sprite`接口，它包含了共享的纹理和独立的坐标。
- `SpriteFactory`类负责创建和管理`GameSprite`实例，确保相同的纹理不会被重复创建。
- `GameLoop`类在游戏的渲染循环中使用`SpriteFactory`来获取和管理精灵，从而优化了内存使用。

通过这种方式，Flyweight模式使得游戏能够高效地管理大量对象，提高了性能并减少了资源消耗。

## Composite模式反例

在Composite模式的反例中，我们可能会看到对象的层次结构没有被正确地抽象化，导致客户端代码需要处理不同类型的对象，而不是统一的接口。这会增加代码的复杂性，并且使得添加新的层次结构组件变得困难。

```java
// 叶子节点
class Leaf {
    public void operation() {
        System.out.println("Leaf operation");
    }
}

// 复合节点
class Composite {
    private List<Leaf> leaves = new ArrayList<>();

    public void addLeaf(Leaf leaf) {
        leaves.add(leaf);
    }

    public void operation() {
        for (Leaf leaf : leaves) {
            leaf.operation();
        }
    }
}

// 客户端代码，直接处理叶子和复合节点
public class Client {
    public static void main(String[] args) {
        Leaf leaf1 = new Leaf();
        Composite composite = new Composite();

        composite.addLeaf(leaf1);
        leaf1.operation(); // 客户端需要知道如何处理叶子节点
        composite.operation(); // 客户端需要知道如何处理复合节点
    }
}
```

**问题**：在这个反例中，客户端代码需要区分叶子节点和复合节点，并分别调用它们的`operation()`方法。这使得代码难以维护，并且如果需要添加新的层次结构组件，客户端代码可能需要修改。

### Composite模式正例

在Composite模式的正例中，我们定义一个统一的接口，所有层次结构的节点都实现这个接口。这样，客户端代码就可以通过这个接口与层次结构中的任何节点交互，而不需要知道它是叶子节点还是复合节点。

```java
// 组件接口
interface Component {
    void operation();
}

// 叶子节点
class Leaf implements Component {
    @Override
    public void operation() {
        System.out.println("Leaf operation");
    }
}

// 复合节点
class Composite implements Component {
    private List<Component> children = new ArrayList<>();

    public void add(Component component) {
        children.add(component);
    }

    public void remove(Component component) {
        children.remove(component);
    }

    @Override
    public void operation() {
        for (Component child : children) {
            child.operation();
        }
    }
}

// 客户端代码，统一处理组件
public class Client {
    public static void main(String[] args) {
        Component leaf1 = new Leaf();
        Composite composite = new Composite();

        composite.add(leaf1);
        composite.operation(); // 客户端统一处理组件

        // 可以继续添加更多的叶子节点或复合节点
        Component leaf2 = new Leaf();
        Composite nestedComposite = new Composite();
        nestedComposite.add(leaf2);
        composite.add(nestedComposite);
        composite.operation(); // 客户端仍然统一处理组件
    }
}
```

**关键注释**：

- `Component`接口定义了层次结构中所有节点的行为。
- `Leaf`类和`Composite`类都实现了`Component`接口，这样它们就可以被客户端统一处理。
- `Composite`类管理其子组件的列表，可以添加或移除子组件。
- 客户端代码通过`Component`接口与层次结构中的节点交互，无需关心节点是叶子还是复合。

在这个正例中，Composite模式允许客户端代码以统一的方式处理层次结构中的所有节点，无论它们是叶子节点还是复合节点。这种方式简化了客户端代码，并且使得添加新的层次结构组件变得容易，因为它们只需要实现`Component`接口。

 在真实开发中，Composite模式常用于表示部分整体层次结构，这种结构在软件架构、用户界面组件、文件系统等场景中非常常见。以下是一个用户界面组件层次结构的真实例子，其中使用了Composite模式。

### 真实实例：图形用户界面（GUI）组件

在构建图形用户界面时，我们可能会有各种组件，如按钮（Button）、文本框（TextBox）、面板（Panel）等。面板可以包含其他组件，形成一个层次结构。我们希望客户端代码能够统一处理所有类型的组件，无论它们是单个元素还是包含其他元素的容器。

首先，我们定义一个组件接口：

```java
// 组件接口
interface GuiComponent {
    void draw(); // 绘制组件
    void add(GuiComponent child); // 添加子组件
    void remove(GuiComponent child); // 移除子组件
}
```

然后，我们实现具体的组件类：

```java
// 单个元素组件，如按钮
class Button implements GuiComponent {
    private String label;

    public Button(String label) {
        this.label = label;
    }

    @Override
    public void draw() {
        System.out.println("Drawing Button: " + label);
    }

    @Override
    public void add(GuiComponent child) {
        // 按钮不接受子组件
    }

    @Override
    public void remove(GuiComponent child) {
        // 按钮不接受子组件
    }
}

// 容器组件，如面板
class Panel implements GuiComponent {
    private List<GuiComponent> children = new ArrayList<>();

    @Override
    public void draw() {
        System.out.println("Drawing Panel");
        for (GuiComponent child : children) {
            child.draw(); // 递归绘制所有子组件
        }
    }

    @Override
    public void add(GuiComponent child) {
        children.add(child);
    }

    @Override
    public void remove(GuiComponent child) {
        children.remove(child);
    }
}
```

现在，我们可以在客户端代码中创建一个用户界面，并统一处理所有组件：

```java
// 客户端代码，创建用户界面
public class Client {
    public static void main(String[] args) {
        Panel mainPanel = new Panel();
        Button button1 = new Button("Click Me");
        Button button2 = new Button("Another Button");

        mainPanel.add(button1); // 添加按钮到面板
        mainPanel.add(button2);

        // 绘制整个用户界面
        mainPanel.draw();
    }
}
```

在这个例子中，`GuiComponent`接口定义了所有GUI组件的行为。`Button`类和`Panel`类都实现了这个接口。`Panel`类可以包含其他`GuiComponent`实例，形成一个层次结构。客户端代码通过`GuiComponent`接口与所有组件交互，无论是单个元素还是容器。这种方式使得添加新的组件类型变得非常简单，因为新组件只需要实现`GuiComponent`接口即可。

**关键注释**：

- `GuiComponent`接口提供了统一的操作接口，使得客户端代码可以统一处理所有类型的组件。
- `Button`类实现了`GuiComponent`接口，表示一个不可嵌套的单个元素。
- `Panel`类实现了`GuiComponent`接口，并管理其子组件的列表，允许添加和移除子组件。
- 客户端代码通过`GuiComponent`接口创建和管理用户界面组件，无需关心组件的具体类型。

通过使用Composite模式，我们可以构建一个灵活的用户界面，其中组件可以自由组合，而客户端代码保持简洁和一致。

## Bridge模式反例

在Bridge模式的反例中，我们可能会看到实现（Implementation）和抽象（Abstraction）紧密耦合在一起，这导致它们不能独立变化。当需要改变实现或者扩展抽象时，这种耦合会导致代码难以维护。

```java
// 抽象部分
class Abstraction {
    private Implementation implementation;

    public Abstraction(Implementation implementation) {
        this.implementation = implementation;
    }

    public void operation() {
        implementation.execute();
    }
}

// 实现部分
class Implementation {
    public void execute() {
        System.out.println("Implementation execute");
    }
}

// 客户端代码，直接使用抽象和实现
public class Client {
    public static void main(String[] args) {
        Implementation impl = new Implementation();
        Abstraction abs = new Abstraction(impl);
        abs.operation();
    }
}
```

**问题**：在这个反例中，`Abstraction`类直接依赖于`Implementation`类的实例。如果我们需要替换或者扩展`Implementation`，我们可能需要修改`Abstraction`类，这违反了开闭原则。

### Bridge模式正例

在Bridge模式的正例中，我们分离了抽象和实现，使得它们可以独立变化。这样，我们可以在不修改抽象类的情况下，替换或者扩展实现类。

```java
// 抽象部分的接口
interface Implementation {
    void execute();
}

// 实现部分的接口
interface Abstraction {
    void operation();
}

// 具体实现
class ConcreteImplementationA implements Implementation {
    @Override
    public void execute() {
        System.out.println("Concrete Implementation A execute");
    }
}

class ConcreteImplementationB implements Implementation {
    @Override
    public void execute() {
        System.out.println("Concrete Implementation B execute");
    }
}

// 抽象部分的具体实现
class RefinedAbstraction implements Abstraction {
    private Implementation implementation;

    public RefinedAbstraction(Implementation implementation) {
        this.implementation = implementation;
    }

    @Override
    public void operation() {
        implementation.execute();
    }
}

// 客户端代码，使用桥接模式
public class Client {
    public static void main(String[] args) {
        Implementation implA = new ConcreteImplementationA();
        Abstraction absA = new RefinedAbstraction(implA);
        absA.operation(); // 使用 ConcreteImplementationA

        Implementation implB = new ConcreteImplementationB();
        Abstraction absB = new RefinedAbstraction(implB);
        absB.operation(); // 使用 ConcreteImplementationB
    }
}
```

**关键注释**：

- `Implementation`接口定义了实现部分的行为。
- `Abstraction`接口定义了抽象部分的行为。
- `ConcreteImplementationA`和`ConcreteImplementationB`类实现了`Implementation`接口，它们提供了不同的实现。
- `RefinedAbstraction`类实现了`Abstraction`接口，它持有一个`Implementation`实例，这样抽象部分的行为可以通过不同的实现来定义。
- 客户端代码通过`Abstraction`接口与具体的实现类交互，而不需要知道实现的具体细节。

在这个正例中，Bridge模式允许我们独立地变化抽象和实现，提高了系统的灵活性和可扩展性。我们可以在不修改抽象类的情况下，通过替换实现类来改变系统的行为。

 在真实开发中，Bridge模式常用于系统架构设计，特别是在需要将一个对象的抽象部分与实现部分分离的场景。以下是一个真实的例子，它展示了如何在一个软件系统中使用Bridge模式来实现可插拔的插件。

### 真实实例：软件系统中的插件架构

假设我们正在开发一个软件系统，该系统允许用户通过插件来扩展其功能。我们希望插件的实现可以独立于核心系统开发，以便在不修改核心系统代码的情况下，添加新的插件功能。

首先，我们定义插件的抽象接口：

```java
// 插件接口
interface PluginInterface {
    void performAction();
}
```

然后，我们定义插件的实现类，这些类将由第三方开发者提供：

```java
// 插件实现类A
class PluginA implements PluginInterface {
    @Override
    public void performAction() {
        System.out.println("Plugin A is performing an action");
    }
}

// 插件实现类B
class PluginB implements PluginInterface {
    @Override
    public void performAction() {
        System.out.println("Plugin B is performing an action");
    }
}
```

接下来，我们创建一个插件管理器，它将作为核心系统与插件之间的桥梁：

```java
// 插件管理器
class PluginManager {
    private PluginInterface plugin;

    public PluginManager(PluginInterface plugin) {
        this.plugin = plugin;
    }

    public void usePlugin() {
        plugin.performAction();
    }
}
```

现在，我们可以在客户端代码中使用插件管理器来加载和使用插件：

```java
// 客户端代码
public class Client {
    public static void main(String[] args) {
        PluginInterface pluginA = new PluginA();
        PluginInterface pluginB = new PluginB();

        PluginManager managerA = new PluginManager(pluginA);
        PluginManager managerB = new PluginManager(pluginB);

        managerA.usePlugin(); // 使用插件A
        managerB.usePlugin(); // 使用插件B
    }
}
```

在这个例子中，`PluginInterface`定义了插件的行为。`PluginA`和`PluginB`是具体的插件实现。`PluginManager`作为桥梁，它持有一个`PluginInterface`实例，允许核心系统通过统一的接口与不同的插件交互。客户端代码通过`PluginManager`来使用插件，而不需要知道插件的具体实现。

**关键注释**：

- `PluginInterface`提供了插件的抽象定义，使得核心系统可以与任何实现了该接口的插件交互。
- `PluginA`和`PluginB`是第三方开发者提供的插件实现，它们可以独立于核心系统开发。
- `PluginManager`作为桥梁，它封装了插件的实现，使得核心系统可以在不知道具体插件实现的情况下使用它们。
- 客户端代码通过`PluginManager`来加载和执行插件，这提供了高度的灵活性和可扩展性。

通过使用Bridge模式，我们可以设计出一个可扩展的软件系统，允许用户通过插件来增强系统功能，同时保持核心系统的稳定性和可维护性。

## 模板方法模式（Template Method Pattern）反例

在模板方法模式的反例中，我们可能会看到子类直接覆盖父类的方法，而不是遵循父类定义的算法骨架。这可能导致子类的方法与父类的算法骨架不匹配，从而破坏了模板方法的意图。

```java
// 父类，定义算法骨架
class Parent {
    public void templateMethod() {
        step1();
        step2();
        step3();
    }

    protected void step1() {
        System.out.println("Parent step1");
    }

    protected void step2() {
        System.out.println("Parent step2");
    }

    protected void step3() {
        System.out.println("Parent step3");
    }
}

// 子类，直接覆盖父类的方法
class Child extends Parent {
    @Override
    protected void step1() {
        System.out.println("Child step1");
    }

    @Override
    protected void step2() {
        // 子类覆盖了 step2，但没有遵循算法骨架
        System.out.println("Child step2 override");
    }

    @Override
    protected void step3() {
        System.out.println("Child step3");
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Child child = new Child();
        child.templateMethod();
    }
}
```

**问题**：在这个反例中，`Child`类覆盖了`Parent`类的`step2`方法，但是它没有遵循`Parent`类定义的算法骨架。这可能导致`templateMethod`的执行流程被破坏，因为子类可能没有按照预期的方式执行所有步骤。

### 模板方法模式正例

在模板方法模式的正例中，我们定义了一个算法的骨架，由一系列步骤组成，这些步骤中的某些步骤由子类实现。这样，子类可以在不改变算法结构的情况下，重新定义算法的某些特定步骤。

```java
// 父类，定义算法骨架
abstract class Parent {
    public void templateMethod() {
        step1();
        step2();
        step3();
    }

    // 子类必须实现的步骤
    protected abstract void step1();

    // 子类可以选择性地覆盖的步骤
    protected void step2() {
        System.out.println("Parent step2");
    }

    // 子类可以选择性地覆盖的步骤
    protected void step3() {
        System.out.println("Parent step3");
    }
}

// 子类，实现算法的特定步骤
class Child extends Parent {
    @Override
    protected void step1() {
        System.out.println("Child step1");
    }

    // 覆盖父类的 step2 方法
    @Override
    protected void step2() {
        System.out.println("Child step2 override");
    }

    // 可以选择不覆盖 step3 方法，使用父类的默认实现
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Child child = new Child();
        child.templateMethod();
    }
}
```

在这个正例中，`Parent`类定义了算法的骨架，其中`step1`是抽象方法，必须由子类实现；`step2`和`step3`是可选的，子类可以选择覆盖它们。`Child`类继承自`Parent`类，并实现了`step1`方法，同时覆盖了`step2`方法。客户端代码通过`Child`类的实例调用`templateMethod`，这将按照`Parent`类定义的算法骨架执行，同时允许`Child`类自定义某些步骤。

**关键注释**：

- `Parent`类定义了算法的骨架，其中包含了一系列步骤。
- `Child`类继承自`Parent`类，并根据需要实现了或覆盖了某些步骤。
- 客户端代码通过子类实例调用模板方法，这保证了算法的结构不变，同时允许子类自定义行为。

通过使用模板方法模式，我们可以确保子类遵循统一的算法结构，同时提供了足够的灵活性来自定义特定步骤的行为。这使得代码更加模块化，易于维护和扩展。
## 观察者模式（Observer Pattern）反例
在观察者模式的反例中，我们可能会看到主题（Subject）和观察者（Observer）之间存在直接的耦合，或者观察者直接访问主题的内部状态，这违反了观察者模式的原则。

```java
// 主题类，包含观察者列表和内部状态
class Subject {
    private List<Observer> observers = new ArrayList<>();
    private String state;

    public void setState(String state) {
        this.state = state;
        notifyObservers();
    }

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    private void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(this.state); // 直接访问内部状态
        }
    }
}

// 观察者接口
interface Observer {
    void update(String state);
}

// 具体观察者类
class ConcreteObserver implements Observer {
    @Override
    public void update(String state) {
        System.out.println("State changed to: " + state);
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Subject subject = new Subject();
        ConcreteObserver observer = new ConcreteObserver();
        subject.addObserver(observer);

        subject.setState("New State"); // 状态改变，观察者被通知
    }
}
```

**问题**：在这个反例中，`Subject`类直接通知观察者内部状态的改变，这违反了封装原则。如果`Subject`的内部状态结构发生变化，所有观察者都需要修改以适应新的结构。

### 观察者模式正例

在观察者模式的正例中，我们定义了清晰的接口和契约，使得主题和观察者之间的耦合最小化。主题通知观察者状态的改变，但不暴露内部状态的实现细节。

```java
// 主题接口
interface Subject {
    void setState(String state);
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
}

// 观察者接口
interface Observer {
    void update();
}

// 具体主题类
class ConcreteSubject implements Subject {
    private String state;
    private List<Observer> observers = new ArrayList<>();

    @Override
    public void setState(String state) {
        this.state = state;
        notifyObservers();
    }

    @Override
    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    private void notifyObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }
}

// 具体观察者类
class ConcreteObserver implements Observer {
    private String lastKnownState;

    @Override
    public void update() {
        String currentState = ((ConcreteSubject) this.getSubject()).getState();
        if (!currentState.equals(lastKnownState)) {
            lastKnownState = currentState;
            System.out.println("State changed to: " + lastKnownState);
        }
    }

    // 假设有一个方法可以获取当前注册的主题
    public Subject getSubject() {
        // 实际应用中，这可能通过其他方式实现，例如观察者持有主题的引用
        return null;
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        ConcreteSubject subject = new ConcreteSubject();
        ConcreteObserver observer = new ConcreteObserver();
        subject.addObserver(observer);

        subject.setState("New State"); // 状态改变，观察者被通知
    }
}
```

在这个正例中，`Subject`接口定义了状态改变和观察者管理的方法。`ConcreteSubject`类实现了这些方法，并在状态改变时通知所有注册的观察者。`Observer`接口定义了观察者在状态改变时应该执行的操作。`ConcreteObserver`类实现了这个接口，并在`update`方法中检查状态是否真正发生了变化。客户端代码通过`Subject`接口与主题和观察者交互，而不需要知道它们的具体实现。

**实用示例**：

在实际开发中，观察者模式常用于实现事件监听和通知系统。例如，在一个桌面应用程序中，当用户修改了文档，应用程序可能会通知所有注册的插件或视图组件，以便它们可以更新显示或执行其他相关操作。

```java
// 文档类作为主题
class Document implements Subject {
    private String content;
    private List<Observer> observers;

    public Document() {
        observers = new ArrayList<>();
    }

    public void setContent(String content) {
        this.content = content;
        notifyObservers();
    }

    // ... 实现 Subject 接口的其他方法 ...
}

// 插件类作为观察者
class Plugin implements Observer {
    @Override
    public void update() {
        // 当文档内容改变时，执行插件的特定逻辑
        System.out.println("Plugin updated with new content: " + getDocumentContent());
    }

    private String getDocumentContent() {
        // 假设有一个方法可以从文档中获取当前内容
        return null;
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Document document = new Document();
        Plugin plugin = new Plugin();
        document.addObserver(plugin);

        document.setContent("New content"); // 插件被通知并更新
    }
}
```

在这个实用示例中，`Document`类作为主题，它维护了内容状态和观察者列表。当内容改变时，它会通知所有观察者。`Plugin`类作为观察者，它注册到文档上，并在内容改变时更新自己。客户端代码创建了文档和插件实例，并通过设置文档内容来触发更新。这种模式使得应用程序可以在不修改现有代码的情况下，添加新的插件或视图组件来响应文档内容的变化。

### 观察者模式应用实例 
当然，让我们通过一个完整的实用示
例来展示观察者模式。在这个例子中，我们将创建一个简单的天气应用，其中`WeatherData`类作为主题，它提供天气信息，而`CurrentConditionDisplay`和`ForecastDisplay`作为观察者，它们订阅天气信息并在数据更新时显示相关信息。

```java
// 观察者接口
interface Observer {
    void update(float temperature, float humidity, float pressure);
}

// 具体观察者1：当前条件显示器
class CurrentConditionDisplay implements Observer {
    private String display;

    public CurrentConditionDisplay(String display) {
        this.display = display;
    }

    @Override
    public void update(float temperature, float humidity, float pressure) {
        System.out.println(display + " : " + temperature + " F degrees, " + humidity + "% humidity, " +
                           pressure + " millibars");
    }
}

// 具体观察者2：预报显示器
class ForecastDisplay implements Observer {
    @Override
    public void update(float temperature, float humidity, float pressure) {
        System.out.println("Forecast: " + temperature + " F degrees, " + humidity + "% humidity, " +
                           pressure + " millibars");
    }
}

// 主题：天气数据
class WeatherData {
    private List<Observer> observers = new ArrayList<>();
    private float temperature = 0;
    private float humidity = 0;
    private float pressure = 0;

    public void registerObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyObservers();
    }
}

// 客户端代码
public class WeatherApp {
    public static void main(String[] args) {
        WeatherData weatherData = new WeatherData();
        CurrentConditionDisplay currentDisplay = new CurrentConditionDisplay("Current Conditions");
        ForecastDisplay forecastDisplay = new ForecastDisplay("Forecast");

        weatherData.registerObserver(currentDisplay);
        weatherData.registerObserver(forecastDisplay);

        // 模拟天气数据更新
        weatherData.setMeasurements(85, 80, 1013.25f);
    }
}
```

在这个示例中：

- `Observer`接口定义了观察者在接收到更新时需要执行的操作。
- `CurrentConditionDisplay`和`ForecastDisplay`类实现了`Observer`接口，并定义了它们各自的`update`方法，用于在天气数据更新时显示信息。
- `WeatherData`类作为主题，它维护了天气数据，并提供了注册和注销观察者的方法。当天气数据发生变化时，它会通知所有注册的观察者。
- 客户端代码创建了`WeatherData`实例和两个观察者实例，并将观察者注册到`WeatherData`上。然后，它模拟了天气数据的更新，这导致所有注册的观察者都接收到了通知并更新了它们的显示。

这个例子展示了观察者模式如何在实际应用中使用，允许多个观察者独立地响应同一个主题的状态变化。这种模式非常适合于实现事件驱动的程序设计。

## 责任链模式（Chain of Responsibility Pattern）反例

在责任链模式的反例中，我们可能会看到请求的处理流程不是通过一系列处理者（Handler）按顺序处理，而是直接在客户端代码中进行处理。这可能导致代码的耦合度高，难以维护和扩展。

```java
// 请求处理接口
interface RequestHandler {
    void handleRequest(String request);
}

// 具体处理者A
class HandlerA implements RequestHandler {
    @Override
    public void handleRequest(String request) {
        if ("A".equals(request)) {
            System.out.println("Handler A handled request A");
        } else {
            // 直接处理请求，而不是传递给下一个处理者
            System.out.println("Handler A cannot handle request, request dropped");
        }
    }
}

// 具体处理者B
class HandlerB implements RequestHandler {
    @Override
    public void handleRequest(String request) {
        if ("B".equals(request)) {
            System.out.println("Handler B handled request B");
        } else {
            // 直接处理请求，而不是传递给下一个处理者
            System.out.println("Handler B cannot handle request, request dropped");
        }
    }
}

// 客户端代码，直接处理请求
public class Client {
    public static void main(String[] args) {
        RequestHandler handlerA = new HandlerA();
        RequestHandler handlerB = new HandlerB();

        handlerA.handleRequest("A"); // 正确处理
        handlerB.handleRequest("B"); // 正确处理
        handlerA.handleRequest("B"); // 错误处理，请求被丢弃
    }
}
```

**问题**：在这个反例中，每个处理者都直接处理请求，而不是将请求传递给链中的下一个处理者。这导致请求的处理流程在客户端代码中硬编码，使得添加新的处理者或修改处理逻辑变得困难。

### 责任链模式正例

在责任链模式的正例中，我们定义了一个处理者接口，每个处理者都持有对下一个处理者的引用。请求在处理者之间按顺序传递，直到被正确处理或传递到链的末端。

```java
// 请求处理接口
interface RequestHandler {
    void handleRequest(String request);
    RequestHandler getNext();
    void setNext(RequestHandler next);
}

// 具体处理者A
class HandlerA implements RequestHandler {
    private RequestHandler next;

    @Override
    public void handleRequest(String request) {
        if ("A".equals(request)) {
            System.out.println("Handler A handled request A");
        } else if (next != null) {
            // 将请求传递给下一个处理者
            next.handleRequest(request);
        } else {
            System.out.println("No handler found for request");
        }
    }

    @Override
    public RequestHandler getNext() {
        return next;
    }

    @Override
    public void setNext(RequestHandler next) {
        this.next = next;
    }
}

// 具体处理者B
class HandlerB implements RequestHandler {
    private RequestHandler next;

    // ... 实现与 HandlerA 类似的方法 ...
}

// 客户端代码，构建责任链并处理请求
public class Client {
    public static void main(String[] args) {
        RequestHandler handlerA = new HandlerA();
        RequestHandler handlerB = new HandlerB();
        handlerA.setNext(handlerB); // 构建责任链

        handlerA.handleRequest("A"); // 正确处理
        handlerA.handleRequest("B"); // 传递给 HandlerB 处理
        handlerA.handleRequest("C"); // 传递给 HandlerB，然后丢弃
    }
}
```

在这个正例中，`RequestHandler`接口定义了处理请求的方法以及获取和设置下一个处理者的方法。每个处理者都实现了这个接口，并在`handleRequest`方法中根据请求类型进行处理，如果当前处理者无法处理请求，则将请求传递给链中的下一个处理者。客户端代码负责构建责任链，然后通过链的头部处理请求。

**实用示例**：

在实际开发中，责任链模式常用于处理工作流、审批流程等场景。例如，在一个公司中，员工提交的报销单需要经过多个级别的审批。每个审批级别可以看作是一个处理者，报销单在这些处理者之间按顺序传递，直到被最终批准或拒绝。

```java
// 报销单处理者接口
interface ReimbursementHandler {
    void processReimbursement(Reimbursement reimbursement);
    ReimbursementHandler getNextHandler();
    void setNextHandler(ReimbursementHandler nextHandler);
}

// 具体处理者：部门经理
class DepartmentManager implements ReimbursementHandler {
    private ReimbursementHandler nextHandler;

    // ... 实现处理报销单的方法 ...
    // ... 实现获取和设置下一个处理者的方法 ...
}

// 具体处理者：财务主管
class FinanceManager implements ReimbursementHandler {
    private ReimbursementHandler nextHandler;

    // ... 实现处理报销单的方法 ...
    // ... 实现获取和设置下一个处理者的方法 ...
}

// 客户端代码，构建责任链并处理报销单
public class ReimbursementSystem {
    public static void main(String[] args) {
        ReimbursementHandler departmentManager = new DepartmentManager();
        ReimbursementHandler financeManager = new FinanceManager();
        departmentManager.setNextHandler(financeManager); // 构建责任链

        // 假设有一个报销单对象
        Reimbursement reimbursement = new Reimbursement();
        departmentManager.processReimbursement(reimbursement); // 开始处理报销单
    }
}
```

在这个实用示例中，`ReimbursementHandler`接口定义了处理报销单的方法以及获取和设置下一个处理者的方法。每个审批级别的处理者实现了这个接口，并在`processReimbursement`方法中根据报销单的金额和规则进行处理。如果当前处理者无法批准报销单，它将传递给链中的下一个处理者。客户端代码负责构建责任链，并启动报销单的处理流程。这种模式使得添加新的审批级别或修改审批逻辑变得非常简单，因为只需要调整责任链的构建即可。

 ### 迭代器模式（Iterator Pattern）反例

在迭代器模式的反例中，我们可能会看到客户端代码直接访问集合的内部结构，而不是通过迭代器进行访问。这违反了封装原则，因为客户端代码依赖于集合的具体实现。

```java
// 集合类
class Collection {
    private List<Integer> list = new ArrayList<>();

    public void add(Integer element) {
        list.add(element);
    }

    // 客户端代码直接访问集合内部
    public void display() {
        for (Integer element : list) {
            System.out.println(element);
        }
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Collection collection = new Collection();
        collection.add(1);
        collection.add(2);
        collection.add(3);

        // 直接访问集合内部
        collection.display();
    }
}
```

**问题**：在这个反例中，`Collection`类的`display`方法直接暴露了其内部的`list`结构。这使得客户端代码依赖于`Collection`类的内部实现，如果`Collection`类的内部结构发生变化（例如，从`List`变为其他数据结构），客户端代码可能需要修改。

### 迭代器模式正例

在迭代器模式的正例中，我们定义了一个迭代器接口，它提供了遍历集合的方法，而不需要暴露集合的内部结构。这样，客户端代码可以通过迭代器与集合交互，而不需要知道集合的具体实现。

```java
// 迭代器接口
interface Iterator {
    boolean hasNext();
    Integer next();
}

// 集合类
class Collection {
    private List<Integer> list = new ArrayList<>();

    public Iterator createIterator() {
        return new ListIterator(list.iterator());
    }

    public void add(Integer element) {
        list.add(element);
    }
}

// 具体迭代器类
class ListIterator implements Iterator {
    private Iterator listIterator;

    public ListIterator(Iterator listIterator) {
        this.listIterator = listIterator;
    }

    @Override
    public boolean hasNext() {
        return listIterator.hasNext();
    }

    @Override
    public Integer next() {
        return (Integer) listIterator.next();
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Collection collection = new Collection();
        collection.add(1);
        collection.add(2);
        collection.add(3);

        // 使用迭代器遍历集合
        Iterator iterator = collection.createIterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }
}
```

在这个正例中，`Collection`类提供了一个`createIterator`方法，它返回一个`Iterator`对象。客户端代码通过这个迭代器来遍历集合，而不需要直接访问集合的内部结构。`ListIterator`类实现了`Iterator`接口，并使用`List`的迭代器来提供遍历功能。

**实用示例**：

在实际开发中，迭代器模式常用于遍历各种数据结构，如列表、树、图等。以下是一个简单的图书管理系统的例子，其中使用了迭代器模式来遍历图书集合。

```java
// 图书接口
interface Book {
    String getTitle();
}

// 图书类
class Novel implements Book {
    private String title;

    public Novel(String title) {
        this.title = title;
    }

    @Override
    public String getTitle() {
        return title;
    }
}

// 图书集合类
class BookCollection {
    private List<Book> books = new ArrayList<>();

    public void addBook(Book book) {
        books.add(book);
    }

    public Iterator<Book> createIterator() {
        return books.iterator();
    }
}

// 图书迭代器接口
interface BookIterator extends Iterator<Book> {
    // 可以添加特定于图书的迭代器方法
}

// 图书迭代器实现类
class BookListIterator implements BookIterator {
    private Iterator<Book> iterator;

    public BookListIterator(Iterator<Book> iterator) {
        this.iterator = iterator;
    }

    @Override
    public boolean hasNext() {
        return iterator.hasNext();
    }

    @Override
    public Book next() {
        return iterator.next();
    }
}

// 客户端代码
public class Bookstore {
    public static void main(String[] args) {
        BookCollection collection = new BookCollection();
        collection.addBook(new Novel("The Great Gatsby"));
        collection.addBook(new Novel("1984"));

        // 使用迭代器遍历图书集合
        BookIterator iterator = collection.createIterator();
        while (iterator.hasNext()) {
            Book book = iterator.next();
            System.out.println(book.getTitle());
        }
    }
}
```

在这个实用示例中，`BookCollection`类提供了一个`createIterator`方法，它返回一个`BookIterator`对象。客户端代码通过这个迭代器来遍历图书集合，而不需要知道集合的具体实现。这样，即使`BookCollection`类的内部结构发生变化，客户端代码也可以保持不变。

 ### 命令模式（Command Pattern）反例

在命令模式的反例中，我们可能会看到请求的发送者（客户端）直接调用接收者（执行者）的方法，而不是通过命令对象。这会导致请求的发送者与接收者之间紧密耦合，使得系统难以扩展和维护。

```java
// 接收者类
class Receiver {
    public void action() {
        System.out.println("Receiver performs an action");
    }
}

// 客户端代码，直接调用接收者的方法
public class Client {
    public static void main(String[] args) {
        Receiver receiver = new Receiver();
        receiver.action(); // 直接调用接收者的方法
    }
}
```

**问题**：在这个反例中，客户端直接与接收者交互，这使得客户端依赖于接收者的具体实现。如果接收者的行为或接口发生变化，客户端代码可能需要修改。

### 命令模式正例

在命令模式的正例中，我们引入了命令接口和具体命令类，它们封装了接收者和要执行的操作。客户端通过命令对象来发送请求，而不是直接调用接收者的方法。这样，请求的发送者和接收者之间解耦，系统更加灵活。

```java
// 命令接口
interface Command {
    void execute();
}

// 具体命令类
class ConcreteCommand implements Command {
    private Receiver receiver;

    public ConcreteCommand(Receiver receiver) {
        this.receiver = receiver;
    }

    @Override
    public void execute() {
        receiver.action();
    }
}

// 接收者类
class Receiver {
    public void action() {
        System.out.println("Receiver performs an action");
    }
}

// 客户端代码，使用命令对象发送请求
public class Client {
    public static void main(String[] args) {
        Receiver receiver = new Receiver();
        Command command = new ConcreteCommand(receiver);
        command.execute(); // 使用命令对象发送请求
    }
}
```

在这个正例中，`Command`接口定义了执行操作的方法。`ConcreteCommand`类实现了`Command`接口，并封装了接收者对象和要执行的操作。客户端创建了`ConcreteCommand`对象，并在需要时调用其`execute`方法来执行操作。这种方式使得客户端与接收者解耦，即使接收者发生变化，客户端代码也可以保持不变。

**实用示例**：

在实际开发中，命令模式常用于实现撤销/重做功能、日志记录、事件处理等。以下是一个简单的文本编辑器的例子，其中使用了命令模式来实现撤销功能。

```java
// 编辑器类
class Editor {
    private String content;

    public void setContent(String content) {
        this.content = content;
    }

    public String getContent() {
        return content;
    }
}

// 命令接口
interface Command {
    void execute();
    void undo();
}

// 具体命令类：设置内容命令
class SetContentCommand implements Command {
    private Editor editor;
    private String originalContent;
    private String newContent;

    public SetContentCommand(Editor editor, String newContent) {
        this.editor = editor;
        this.newContent = newContent;
    }

    @Override
    public void execute() {
        originalContent = editor.getContent();
        setContent(editor, newContent);
    }

    @Override
    public void undo() {
        setContent(editor, originalContent);
    }

    private void setContent(Editor editor, String content) {
        editor.setContent(content);
    }
}

// 客户端代码
public class TextEditor {
    public static void main(String[] args) {
        Editor editor = new Editor();
        Command command = new SetContentCommand(editor, "New content");

        command.execute(); // 设置新内容
        System.out.println(editor.getContent()); // 输出新内容

        command.undo(); // 撤销操作
        System.out.println(editor.getContent()); // 输出原始内容
    }
}
```

在这个实用示例中，`SetContentCommand`类封装了设置编辑器内容的操作。它实现了`Command`接口，并提供了`execute`和`undo`方法。客户端代码创建了`SetContentCommand`对象，并在需要时调用这些方法来执行或撤销操作。这样，编辑器的内部逻辑与客户端代码解耦，使得添加新的命令或修改现有命令变得更加容易。

 ### Memento模式（Memento Pattern）反例

在Memento模式的反例中，我们可能会看到对象的内部状态直接被外部对象访问和修改，而不是通过封装和恢复机制。这会导致对象的状态管理变得复杂，难以追踪状态的变化，也不利于实现撤销/重做功能。

```java
// 原始类
class Original {
    private String state;

    public Original(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }
}

// 客户端代码，直接访问和修改原始对象的状态
public class Client {
    public static void main(String[] args) {
        Original original = new Original("Initial State");
        System.out.println("Original State: " + original.getState());

        original.setState("New State");
        System.out.println("Original State: " + original.getState());

        // 无法撤销到原始状态
    }
}
```

**问题**：在这个反例中，客户端代码直接访问和修改`Original`对象的状态。这种方式没有提供状态保存和恢复的机制，使得撤销操作变得困难。

### Memento模式正例

在Memento模式的正例中，我们引入了备忘录（Memento）和负责人（Caretaker）角色。原始对象（Originator）的状态被保存到备忘录中，负责人负责管理备忘录。这样，原始对象可以在不同的状态之间切换，而不需要暴露其内部状态。

```java
// 备忘录类
class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

// 原始类
class Originator {
    private String state;

    public Originator(String initialState) {
        this.state = initialState;
    }

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }

    public Memento saveState() {
        return new Memento(state);
    }

    public void restoreState(Memento memento) {
        this.state = memento.getState();
    }
}

// 负责人类
class Caretaker {
    private Memento memento;

    public void saveMemento(Memento memento) {
        this.memento = memento;
    }

    public void undo() {
        if (memento != null) {
            Originator originator = new Originator(memento.getState());
            // 应用撤销逻辑
        }
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Originator originator = new Originator("Initial State");
        System.out.println("Original State: " + originator.getState());

        originator.setState("New State");
        System.out.println("New State: " + originator.getState());

        // 保存当前状态
        Caretaker caretaker = new Caretaker();
        caretaker.saveMemento(originator.saveState());

        // 执行撤销操作
        caretaker.undo();
        System.out.println("Restored State: " + originator.getState());
    }
}
```

在这个正例中，`Originator`类负责维护其内部状态，并提供了保存和恢复状态的方法。`Memento`类封装了原始对象的状态，但只提供了有限的访问接口。`Caretaker`类负责管理备忘录，它保存了原始对象的状态，并在需要时执行撤销操作。客户端代码通过`Caretaker`来管理原始对象的状态变化，从而实现了撤销功能。

**实用示例**：

在实际开发中，Memento模式常用于实现软件系统中的撤销/重做功能。以下是一个简单的文本编辑器的例子，其中使用了Memento模式来实现撤销功能。

```java
// 文本编辑器类
class TextEditor {
    private String text;

    public TextEditor(String initialText) {
        this.text = initialText;
    }

    public String getText() {
        return text;
    }

    public void setText(String newText) {
        // 保存当前状态
        Caretaker caretaker = new Caretaker();
        caretaker.saveMemento(new Memento(text));

        this.text = newText;
    }

    public void undo() {
        Caretaker caretaker = new Caretaker();
        Memento memento = caretaker.getMemento();
        if (memento != null) {
            this.text = memento.getState();
        }
    }
}

// 备忘录类
class Memento {
    private String text;

    public Memento(String text) {
        this.text = text;
    }

    public String getText() {
        return text;
    }
}

// 客户端代码
public class TextEditorApp {
    public static void main(String[] args) {
        TextEditor editor = new TextEditor("Hello, World!");

        editor.setText("Hello, Memento!");
        System.out.println("Text after change: " + editor.getText());

        editor.undo();
        System.out.println("Text after undo: " + editor.getText());
    }
}
```

在这个实用示例中，`TextEditor`类封装了文本编辑器的状态，并提供了设置文本和撤销操作的方法。通过使用`Memento`和`Caretaker`，`TextEditor`可以在不同的文本状态之间切换，而不需要暴露其内部状态。客户端代码通过`TextEditor`来管理文本的变化，从而实现了撤销功能。

 ### 策略模式（Strategy Pattern）反例

在策略模式的反例中，我们可能会看到客户端代码中硬编码了不同的算法或行为，而不是通过策略对象来封装这些行为。这会导致代码的可扩展性和可维护性降低，因为每次添加新的策略都需要修改客户端代码。

```java
// 不同的策略实现
class StrategyA {
    public void doOperation() {
        System.out.println("Strategy A in action");
    }
}

class StrategyB {
    public void doOperation() {
        System.out.println("Strategy B in action");
    }
}

// 客户端代码，直接使用不同的策略实现
public class Client {
    public static void main(String[] args) {
        StrategyA strategyA = new StrategyA();
        StrategyB strategyB = new StrategyB();

        strategyA.doOperation(); // 使用策略A
        strategyB.doOperation(); // 使用策略B

        // 如果需要添加新的策略，客户端代码需要修改
    }
}
```

**问题**：在这个反例中，客户端代码直接创建并使用了不同的策略实现。如果需要添加新的策略或者改变策略的选择逻辑，客户端代码将需要修改，这违反了开闭原则。

### 策略模式正例

在策略模式的正例中，我们定义了一个策略接口，所有策略实现都遵循这个接口。客户端代码通过这个接口与策略对象交互，而不是直接与具体的策略实现交互。这样，我们可以在不修改客户端代码的情况下，动态地切换策略。

```java
// 策略接口
interface Strategy {
    void doOperation();
}

// 策略实现A
class StrategyA implements Strategy {
    @Override
    public void doOperation() {
        System.out.println("Strategy A in action");
    }
}

// 策略实现B
class StrategyB implements Strategy {
    @Override
    public void doOperation() {
        System.out.println("Strategy B in action");
    }
}

// 上下文类，持有策略对象
class Context {
    private Strategy strategy;

    public Context(Strategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }

    public void executeStrategy() {
        strategy.doOperation();
    }
}

// 客户端代码，使用策略接口与策略对象交互
public class Client {
    public static void main(String[] args) {
        Context context = new Context(new StrategyA());
        context.executeStrategy(); // 使用策略A

        context.setStrategy(new StrategyB());
        context.executeStrategy(); // 切换到策略B
    }
}
```

在这个正例中，`Strategy`接口定义了策略的行为。`StrategyA`和`StrategyB`是具体的策略实现。`Context`类持有一个策略对象，并提供了执行策略的方法。客户端代码通过`Context`来执行策略，而不是直接与策略实现交互。这样，我们可以根据需要动态地切换策略，而不需要修改客户端代码。

**实用示例**：

在实际开发中，策略模式常用于实现不同的算法或行为策略。以下是一个简单的排序算法选择器的例子，其中使用了策略模式来动态选择排序算法。

```java
// 排序策略接口
interface SortStrategy {
    void sort(int[] array);
}

// 冒泡排序策略
class BubbleSort implements SortStrategy {
    @Override
    public void sort(int[] array) {
        // 实现冒泡排序算法
    }
}

// 快速排序策略
class QuickSort implements SortStrategy {
    @Override
    public void sort(int[] array) {
        // 实现快速排序算法
    }
}

// 排序上下文类
class SortContext {
    private SortStrategy strategy;

    public SortContext(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }

    public void performSort(int[] array) {
        strategy.sort(array);
    }
}

// 客户端代码
public class SortApp {
    public static void main(String[] args) {
        int[] array = {64, 34, 25, 12, 22, 11, 90};
        SortContext sortContext = new SortContext(new BubbleSort());

        sortContext.performSort(array); // 使用冒泡排序
        // ... 可以动态切换到其他排序策略
    }
}
```

在这个实用示例中，`SortStrategy`接口定义了排序算法的行为。`BubbleSort`和`QuickSort`是具体的排序策略实现。`SortContext`类持有一个排序策略对象，并提供了执行排序的方法。客户端代码通过`SortContext`来执行排序，可以根据需要动态地切换排序策略，而不需要修改客户端代码。这样，排序算法的选择和实现与客户端代码解耦，使得系统更加灵活和可维护。

 ### 状态模式（State Pattern）反例

在状态模式的反例中，我们可能会看到对象的状态变化直接在对象内部硬编码，而不是通过外部状态对象来管理。这会导致对象的逻辑变得复杂，难以理解和维护，尤其是在状态变化逻辑复杂或者状态数量较多时。

```java
// 具有状态的行为类
class Behavior {
    private String state;

    public Behavior(String initialState) {
        this.state = initialState;
    }

    public void changeState(String newState) {
        this.state = newState;
        handleStateChange();
    }

    private void handleStateChange() {
        // 根据状态执行不同的逻辑
        switch (state) {
            case "STATE_A":
                // 处理状态A
                break;
            case "STATE_B":
                // 处理状态B
                break;
            // ... 其他状态
            default:
                // 未知状态处理
                break;
        }
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Behavior behavior = new Behavior("STATE_A");
        behavior.changeState("STATE_B"); // 改变状态
    }
}
```

**问题**：在这个反例中，`Behavior`类内部包含了状态变化的逻辑。这种方式使得状态逻辑与行为逻辑混合在一起，难以管理和扩展。如果需要添加新的状态或改变状态逻辑，可能需要修改`Behavior`类，这违反了开闭原则。

### 状态模式正例

在状态模式的正例中，我们定义了一个状态接口和一系列具体的状态类。每个状态类封装了在特定状态下对象的行为。对象本身持有一个状态对象，并且根据当前状态来执行相应的行为。这样，状态的变化和行为逻辑被分离，使得对象的行为更加清晰和易于管理。

```java
// 状态接口
interface State {
    void handle(Context context);
}

// 具体状态A
class StateA implements State {
    @Override
    public void handle(Context context) {
        System.out.println("Handling in State A");
        // 根据业务逻辑切换到其他状态
        context.changeState(new StateB());
    }
}

// 具体状态B
class StateB implements State {
    @Override
    public void handle(Context context) {
        System.out.println("Handling in State B");
        // 根据业务逻辑切换到其他状态
        context.changeState(new StateA());
    }
}

// 上下文类，持有状态对象
class Context {
    private State state;

    public Context() {
        this.state = new StateA(); // 初始化状态
    }

    public void changeState(State newState) {
        this.state = newState;
    }

    public void handle() {
        state.handle(this);
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Context context = new Context();
        context.handle(); // 执行状态A的行为
        context.handle(); // 切换到状态B并执行行为
    }
}
```

在这个正例中，`State`接口定义了状态对象的行为。`StateA`和`StateB`是具体的状态类，它们封装了特定状态下的行为。`Context`类持有一个状态对象，并且提供了切换状态和执行行为的方法。客户端代码通过`Context`来管理状态的变化和行为的执行，而不需要关心状态逻辑的具体实现。

**实用示例**：

在实际开发中，状态模式常用于实现对象在不同状态下的行为变化。以下是一个简单的交通灯控制的例子，其中使用了状态模式来管理交通灯的状态变化。

```java
// 交通灯状态接口
interface TrafficLightState {
    void handle(Context context);
}

// 红灯状态
class RedLightState implements TrafficLightState {
    @Override
    public void handle(Context context) {
        System.out.println("Red Light: Stop");
        context.changeState(new GreenLightState());
    }
}

// 绿灯状态
class GreenLightState implements TrafficLightState {
    @Override
    public void handle(Context context) {
        System.out.println("Green Light: Go");
        context.changeState(new YellowLightState());
    }
}

// 黄灯状态
class YellowLightState implements TrafficLightState {
    @Override
    public void handle(Context context) {
        System.out.println("Yellow Light: Prepare to stop");
        context.changeState(new RedLightState());
    }
}

// 交通灯上下文类
class TrafficLightContext {
    private TrafficLightState state;

    public TrafficLightContext() {
        this.state = new RedLightState(); // 初始化状态为红灯
    }

    public void changeState(TrafficLightState newState) {
        this.state = newState;
    }

    public void handle() {
        state.handle(this);
    }
}

// 客户端代码
public class TrafficLightControl {
    public static void main(String[] args) {
        TrafficLightContext trafficLight = new TrafficLightContext();
        trafficLight.handle(); // 红灯
        trafficLight.handle(); // 绿灯
        trafficLight.handle(); // 黄灯
        // ... 循环状态变化
    }
}
```

在这个实用示例中，`TrafficLightState`接口定义了交通灯状态的行为。`RedLightState`、`GreenLightState`和`YellowLightState`是具体的状态类，它们封装了不同状态下的交通灯行为。`TrafficLightContext`类持有一个状态对象，并提供了切换状态和执行行为的方法。客户端代码通过`TrafficLightContext`来控制交通灯的状态变化，实现了一个简单的交通灯控制系统。

 ### Mediator模式（中介者模式）反例

在Mediator模式的反例中，对象之间可能会直接通信，导致它们之间存在高度的耦合。这种直接的通信使得代码难以维护，特别是在对象数量增加或通信逻辑变得复杂时。

```java
// 同事类A
class ColleagueA {
    public void receive(String message) {
        System.out.println("Colleague A received: " + message);
    }

    public void send(String message) {
        ColleagueB colleagueB = new ColleagueB();
        colleagueB.receive(message);
    }
}

// 同事类B
class ColleagueB {
    public void receive(String message) {
        System.out.println("Colleague B received: " + message);
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        ColleagueA colleagueA = new ColleagueA();
        ColleagueB colleagueB = new ColleagueB();

        colleagueA.send("Hello from A to B"); // 直接通信
    }
}
```

**问题**：在这个反例中，`ColleagueA`和`ColleagueB`直接相互通信。这种直接的耦合使得系统难以扩展，因为添加新的同事类可能需要修改现有的同事类代码。此外，这种直接通信也使得追踪和理解对象间的交互变得困难。

### Mediator模式正例

在Mediator模式的正例中，我们引入了一个中介者（Mediator）来封装对象之间的通信。同事类（Colleague）不再直接相互通信，而是通过中介者来传递消息。这样，对象之间的耦合度降低，系统的可维护性和可扩展性得到提高。

```java
// 中介者接口
interface Mediator {
    void send(String message, Colleague sender);
}

// 具体中介者
class ConcreteMediator implements Mediator {
    private ColleagueA colleagueA;
    private ColleagueB colleagueB;

    public ConcreteMediator(ColleagueA colleagueA, ColleagueB colleagueB) {
        this.colleagueA = colleagueA;
        this.colleagueB = colleagueB;
    }

    @Override
    public void send(String message, Colleague sender) {
        if (sender == colleagueA) {
            colleagueB.receive(message);
        } else if (sender == colleagueB) {
            colleagueA.receive(message);
        }
    }
}

// 同事类A
class ColleagueA {
    private Mediator mediator;

    public ColleagueA(Mediator mediator) {
        this.mediator = mediator;
    }

    public void send(String message) {
        mediator.send(message, this);
    }

    public void receive(String message) {
        System.out.println("Colleague A received: " + message);
    }
}

// 同事类B
class ColleagueB {
    private Mediator mediator;

    public ColleagueB(Mediator mediator) {
        this.mediator = mediator;
    }

    public void send(String message) {
        mediator.send(message, this);
    }

    public void receive(String message) {
        System.out.println("Colleague B received: " + message);
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Mediator mediator = new ConcreteMediator();
        ColleagueA colleagueA = new ColleagueA(mediator);
        ColleagueB colleagueB = new ColleagueB(mediator);

        colleagueA.send("Hello from A to B"); // 通过中介者通信
        colleagueB.send("Hello from B to A"); // 通过中介者通信
    }
}
```

在这个正例中，`ConcreteMediator`类作为中介者，负责处理同事类之间的通信。`ColleagueA`和`ColleagueB`不再直接相互通信，而是通过调用中介者的方法来发送和接收消息。这种方式使得同事类之间的耦合度降低，系统更加模块化，易于维护和扩展。

**实用示例**：

在实际开发中，Mediator模式常用于实现聊天应用中的用户通信、游戏中的单位管理等场景。以下是一个简单的聊天室的例子，其中使用了Mediator模式来管理用户之间的消息传递。

```java
// 用户接口
interface User {
    void sendMessage(String message);
    void receiveMessage(String message);
}

// 具体用户类
class UserImpl implements User {
    private String name;

    public UserImpl(String name) {
        this.name = name;
    }

    @Override
    public void sendMessage(String message) {
        mediator.sendMessage(message, this);
    }

    @Override
    public void receiveMessage(String message) {
        System.out.println(name + " received: " + message);
    }
}

// 中介者接口
interface ChatMediator {
    void sendMessage(User user, String message);
}

// 具体中介者
class ChatRoom implements ChatMediator {
    private List<User> users = new ArrayList<>();

    public void addUser(User user) {
        users.add(user);
    }

    @Override
    public void sendMessage(User user, String message) {
        for (User otherUser : users) {
            if (otherUser != user) {
                otherUser.receiveMessage(message);
            }
        }
    }
}

// 客户端代码
public class ChatApp {
    public static void main(String[] args) {
        ChatRoom chatRoom = new ChatRoom();
        User user1 = new UserImpl("Alice");
        User user2 = new UserImpl("Bob");

        chatRoom.addUser(user1);
        chatRoom.addUser(user2);

        user1.sendMessage("Hi, Bob!"); // 通过中介者发送消息
        user2.sendMessage("Hi, Alice!"); // 通过中介者发送消息
    }
}
```

在这个实用示例中，`ChatRoom`类作为中介者，管理用户之间的消息传递。`UserImpl`类实现了用户接口，提供了发送和接收消息的方法。用户通过中介者来发送消息，而不是直接相互通信。这种方式使得聊天室的通信逻辑更加清晰，易于管理和扩展。

 ### 访问者模式（Visitor Pattern）反例

在访问者模式的反例中，我们可能会看到对象直接包含其他对象，并且这些对象之间存在直接的依赖关系。当需要对这些对象执行某些操作时，客户端代码可能需要直接调用这些对象的方法，这会导致对象之间的耦合度增加，使得系统难以维护和扩展。

```java
// 元素接口
interface Element {
    void accept(Visitor visitor);
}

// 具体元素A
class ConcreteElementA implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// 具体元素B
class ConcreteElementB implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// 访问者接口
interface Visitor {
    void visit(ConcreteElementA element);
    void visit(ConcreteElementB element);
}

// 客户端代码，直接操作元素
public class Client {
    public static void main(String[] args) {
        Element elementA = new ConcreteElementA();
        Element elementB = new ConcreteElementB();
        Visitor visitor = new ConcreteVisitor();

        elementA.accept(visitor); // 直接调用元素的 accept 方法
        elementB.accept(visitor); // 直接调用元素的 accept 方法
    }
}
```

**问题**：在这个反例中，客户端代码直接与元素和访问者交互。如果元素的种类增加或者访问者的操作发生变化，客户端代码可能需要修改，这违反了开闭原则。

### 访问者模式正例

在访问者模式的正例中，我们定义了元素（Element）和访问者（Visitor）两个接口。元素接口有一个接受（accept）方法，它接受一个访问者对象。访问者接口定义了对各种元素的访问操作。这样，元素和访问者之间没有直接的依赖关系，元素只需要知道如何接受访问者，而具体的访问逻辑由访问者决定。

```java
// 元素接口
interface Element {
    void accept(Visitor visitor);
}

// 具体元素A
class ConcreteElementA implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// 具体元素B
class ConcreteElementB implements Element {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// 访问者接口
interface Visitor {
    void visit(ConcreteElementA element);
    void visit(ConcreteElementB element);
}

// 具体访问者
class ConcreteVisitor implements Visitor {
    public void visit(ConcreteElementA element) {
        System.out.println("Visited ConcreteElementA");
    }

    public void visit(ConcreteElementB element) {
        System.out.println("Visited ConcreteElementB");
    }
}

// 客户端代码，通过访问者操作元素
public class Client {
    public static void main(String[] args) {
        Element elementA = new ConcreteElementA();
        Element elementB = new ConcreteElementB();
        Visitor visitor = new ConcreteVisitor();

        elementA.accept(visitor); // 通过访问者操作元素
        elementB.accept(visitor); // 通过访问者操作元素
    }
}
```

在这个正例中，客户端代码通过访问者来操作元素，而不是直接调用元素的方法。这样，当元素的种类增加或者访问者的操作发生变化时，客户端代码不需要修改。元素和访问者之间的耦合度降低，系统更加灵活和可维护。

**实用示例**：

在实际开发中，访问者模式常用于表示作用于某对象结构中的各元素的操作。以下是一个简单的文档处理系统的例子，其中使用了访问者模式来处理不同类型的文档元素。

```java
// 文档元素接口
interface DocumentElement {
    void accept(Visitor visitor);
}

// 段落元素
class Paragraph implements DocumentElement {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// 图片元素
class Image implements DocumentElement {
    public void accept(Visitor visitor) {
        visitor.visit(this);
    }
}

// 访问者接口
interface Visitor {
    void visit(Paragraph paragraph);
    void visit(Image image);
}

// 具体访问者：打印文档
class PrintVisitor implements Visitor {
    public void visit(Paragraph paragraph) {
        System.out.println("Printing Paragraph");
    }

    public void visit(Image image) {
        System.out.println("Printing Image");
    }
}

// 客户端代码
public class DocumentProcessing {
    public static void main(String[] args) {
        DocumentElement paragraph = new Paragraph();
        DocumentElement image = new Image();
        Visitor visitor = new PrintVisitor();

        paragraph.accept(visitor); // 打印段落
        image.accept(visitor); // 打印图片
    }
}
```

在这个实用示例中，`DocumentElement`接口定义了文档元素的行为，`Paragraph`和`Image`是具体的文档元素。`Visitor`接口定义了对文档元素的操作。客户端代码通过访问者来处理文档元素，而不是直接与元素交互。这种方式使得文档处理逻辑与文档元素的实现解耦，便于维护和扩展。

 ### 解释器模式（Interpreter Pattern）反例

在解释器模式的反例中，我们可能会看到复杂的逻辑直接嵌入在代码中，而不是通过定义语言的语法规则和相应的解释器来处理。这会导致代码难以理解和维护，尤其是在处理复杂的语法结构时。

```java
// 表达式类，直接包含逻辑
class Expression {
    public int interpret() {
        // 复杂的逻辑处理
        // ...
        return 0;
    }
}

// 客户端代码，直接使用表达式
public class Client {
    public static void main(String[] args) {
        Expression expression = new Expression();
        int result = expression.interpret(); // 直接解释表达式
        System.out.println("Result: " + result);
    }
}
```

**问题**：在这个反例中，`Expression`类直接包含了解释逻辑。这种方式使得代码难以扩展，因为添加新的表达式类型或修改解释逻辑都需要修改现有的类。此外，这种直接的逻辑处理也使得代码难以理解和维护。

### 解释器模式正例

在解释器模式的正例中，我们定义了一个抽象表达式类，它包含了一个解释方法。具体的表达式类继承自抽象表达式类，并实现自己的解释逻辑。这样，我们可以为不同的语法结构定义不同的表达式类，而客户端代码可以通过统一的接口与这些表达式交互。

```java
// 抽象表达式类
abstract class Expression {
    public abstract int interpret();
}

// 具体表达式类A
class ConcreteExpressionA extends Expression {
    @Override
    public int interpret() {
        // 实现具体的解释逻辑
        return 42;
    }
}

// 具体表达式类B
class ConcreteExpressionB extends Expression {
    @Override
    public int interpret() {
        // 实现具体的解释逻辑
        return 100;
    }
}

// 客户端代码，使用解释器处理表达式
public class Client {
    public static void main(String[] args) {
        Expression expressionA = new ConcreteExpressionA();
        Expression expressionB = new ConcreteExpressionB();

        int resultA = expressionA.interpret();
        int resultB = expressionB.interpret();

        System.out.println("Result A: " + resultA);
        System.out.println("Result B: " + resultB);
    }
}
```

在这个正例中，`Expression`是一个抽象类，它定义了解释方法。`ConcreteExpressionA`和`ConcreteExpressionB`是具体的表达式类，它们实现了自己的解释逻辑。客户端代码通过创建具体的表达式对象并调用它们的`interpret`方法来获取解释结果。这种方式使得解释逻辑与客户端代码解耦，便于扩展和维护。

**实用示例**：

在实际开发中，解释器模式常用于实现编程语言的解析器、配置文件的解析器等。以下是一个简单的算术表达式解析器的例子，其中使用了解释器模式来处理算术运算。

```java
// 抽象表达式类
abstract class ArithmeticExpression {
    public abstract int evaluate();
}

// 具体表达式类：数字
class NumberExpression extends ArithmeticExpression {
    private int number;

    public NumberExpression(int number) {
        this.number = number;
    }

    @Override
    public int evaluate() {
        return number;
    }
}

// 具体表达式类：加法
class AddExpression extends ArithmeticExpression {
    private ArithmeticExpression left;
    private ArithmeticExpression right;

    public AddExpression(ArithmeticExpression left, ArithmeticExpression right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int evaluate() {
        return left.evaluate() + right.evaluate();
    }
}

// 客户端代码
public class ArithmeticInterpreter {
    public static void main(String[] args) {
        ArithmeticExpression left = new NumberExpression(5);
        ArithmeticExpression right = new NumberExpression(3);
        ArithmeticExpression expression = new AddExpression(left, right);

        int result = expression.evaluate();
        System.out.println("Result: " + result);
    }
}
```

在这个实用示例中，`ArithmeticExpression`是一个抽象类，它定义了评估方法。`NumberExpression`和`AddExpression`是具体的表达式类，它们实现了自己的评估逻辑。客户端代码通过创建具体的表达式对象并调用它们的`evaluate`方法来计算表达式的值。这种方式使得算术表达式的解析逻辑与客户端代码解耦，便于维护和扩展。

 ### Null Object Pattern 反例

在Null Object模式的反例中，我们可能会看到在代码中直接使用`null`值来表示空对象或缺失的数据。这会导致空指针异常（NullPointerException）的风险，使得代码难以维护，尤其是在复杂的系统中。

```java
// 假设我们有一个方法，它接受一个对象作为参数
public class Service {
    public void process(Object obj) {
        if (obj == null) {
            // 处理空对象的逻辑
            System.out.println("Object is null");
        } else {
            // 处理非空对象的逻辑
            obj.doSomething();
        }
    }
}

// 客户端代码，直接传递null
public class Client {
    public static void main(String[] args) {
        Service service = new Service();
        service.process(null); // 直接传递null
    }
}
```

**问题**：在这个反例中，`process`方法直接接收`null`作为参数。这种方式虽然可以处理空对象的情况，但是它依赖于`null`检查，这可能会导致代码中出现大量的空检查逻辑，增加了代码的复杂性。此外，如果`Object`的`doSomething`方法没有正确处理`null`，那么在调用时可能会抛出空指针异常。

### Null Object Pattern 正例

在Null Object模式的正例中，我们使用一个明确的空对象（Null Object）来代替`null`值。这个空对象实现了相同的接口或继承了相同的类，并且提供了默认的行为，以避免空指针异常。

```java
// 空对象接口
interface ObjectInterface {
    void doSomething();
}

// 实际对象实现
class RealObject implements ObjectInterface {
    @Override
    public void doSomething() {
        System.out.println("Doing something with real object");
    }
}

// 空对象实现
class NullObject implements ObjectInterface {
    @Override
    public void doSomething() {
        // 提供默认行为，避免空指针异常
        System.out.println("Doing nothing with null object");
    }
}

// 服务类，接受空对象
public class Service {
    public void process(ObjectInterface obj) {
        if (obj == null) {
            // 使用空对象代替null
            obj = new NullObject();
        }
        obj.doSomething();
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Service service = new Service();
        ObjectInterface obj = null; // 假设我们有一个可能为null的对象
        service.process(obj); // 不需要空检查，因为空对象会处理默认行为
    }
}
```

在这个正例中，我们定义了一个`ObjectInterface`接口和两个实现：`RealObject`和`NullObject`。`NullObject`提供了一个默认的`doSomething`方法，它避免了空指针异常。在`Service`类中，我们不再需要检查`null`，因为即使传入`null`，也会自动使用`NullObject`。这样，客户端代码可以更简洁，且不担心空指针异常。

**实用示例**：

在实际开发中，Null Object模式常用于数据库操作、缓存系统等场景，其中可能存在对象为空的情况。以下是一个简单的数据库查询示例，其中使用了Null Object模式来处理可能为空的结果。

```java
// 数据库结果接口
interface DatabaseResult {
    boolean hasData();
    String getData();
}

// 实际数据库结果实现
class RealDatabaseResult implements DatabaseResult {
    private String data;

    public RealDatabaseResult(String data) {
        this.data = data;
    }

    @Override
    public boolean hasData() {
        return data != null;
    }

    @Override
    public String getData() {
        return data;
    }
}

// 空数据库结果实现
class NullDatabaseResult implements DatabaseResult {
    @Override
    public boolean hasData() {
        return false;
    }

    @Override
    public String getData() {
        return ""; // 或者返回一个默认值或空字符串
    }
}

// 数据库查询服务
public class DatabaseService {
    public DatabaseResult query(String query) {
        // 这里可能是数据库查询逻辑
        // 如果查询结果为空，返回NullDatabaseResult
        if (/* 查询结果为空 */) {
            return new NullDatabaseResult();
        } else {
            String data = /* 获取查询结果 */;
            return new RealDatabaseResult(data);
        }
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        DatabaseService service = new DatabaseService();
        DatabaseResult result = service.query("SELECT * FROM users");

        if (result.hasData()) {
            System.out.println("Data: " + result.getData());
        } else {
            System.out.println("No data found");
        }
    }
}
```

在这个实用示例中，`DatabaseService`类提供了一个查询方法，它返回一个`DatabaseResult`对象。无论查询结果如何，客户端代码都不需要进行空检查，因为`NullDatabaseResult`提供了默认行为。这样，数据库查询的逻辑变得更加简洁和安全。

 ### MVC模式（Model-View-Controller Pattern）反例

在MVC模式的反例中，我们可能会看到视图（View）和模型（Model）之间存在直接的耦合，控制器（Controller）没有起到协调视图和模型的作用，或者视图直接处理用户输入，而不是通过控制器。这会导致代码难以维护，因为视图和模型的逻辑混合在一起，难以分离。

```java
// 模型类，直接与视图交互
class Model {
    private String data;

    public String getData() {
        return data;
    }

    public void setData(String data) {
        this.data = data;
    }
}

// 视图类，直接处理用户输入
class View {
    private Model model;

    public View(Model model) {
        this.model = model;
    }

    public void render() {
        System.out.println("Data: " + model.getData());
    }

    public void handleUserInput(String input) {
        // 直接修改模型数据
        model.setData(input);
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Model model = new Model();
        View view = new View(model);

        view.render(); // 渲染初始数据
        view.handleUserInput("New Data"); // 直接处理用户输入
        view.render(); // 渲染新数据
    }
}
```

**问题**：在这个反例中，`View`类直接与`Model`类交互，处理用户输入并更新模型。这种方式没有遵循MVC模式的原则，因为视图和模型之间存在直接的耦合，控制器的角色没有得到体现。

### MVC模式正例

在MVC模式的正例中，我们明确分离了模型、视图和控制器的角色。模型代表数据和业务逻辑，视图负责显示数据，控制器接收用户输入并更新模型，然后通知视图进行渲染。这样，每个部分都有明确的职责，代码更加模块化，易于维护和扩展。

```java
// 模型类
class Model {
    private String data;
    private View view;

    public Model(View view) {
        this.view = view;
    }

    public String getData() {
        return data;
    }

    public void setData(String data) {
        this.data = data;
        view.render(); // 更新视图
    }
}

// 视图类，只负责显示数据
class View {
    private Model model;

    public View(Model model) {
        this.model = model;
    }

    public void render() {
        System.out.println("Data: " + model.getData());
    }
}

// 控制器类，处理用户输入
class Controller {
    private Model model;
    private View view;

    public Controller(Model model, View view) {
        this.model = model;
        this.view = view;
    }

    public void handleUserInput(String input) {
        model.setData(input); // 更新模型
    }
}

// 客户端代码
public class Client {
    public static void main(String[] args) {
        Model model = new Model(null); // 初始时视图为null
        View view = new View(model);
        Controller controller = new Controller(model, view);

        controller.handleUserInput("Initial Data"); // 通过控制器处理用户输入
        view.render(); // 渲染初始数据

        controller.handleUserInput("New Data"); // 处理新的用户输入
        view.render(); // 渲染新数据
    }
}
```

在这个正例中，`Model`类持有`View`的引用，当数据发生变化时，它会通知`View`进行渲染。`View`类只负责显示数据，不处理用户输入。`Controller`类接收用户输入，更新模型，并确保视图得到更新。这样，模型、视图和控制器之间没有直接的耦合，系统更加灵活和可维护。

**实用示例**：

在实际开发中，MVC模式常用于Web应用程序的开发。以下是一个简单的Web页面计数器的例子，其中使用了MVC模式来分离用户界面和业务逻辑。

```java
// 模型类：计数器
class CounterModel {
    private int count;

    public int getCount() {
        return count;
    }

    public void increment() {
        count++;
    }
}

// 视图类：显示计数器
class CounterView {
    private CounterModel model;

    public CounterView(CounterModel model) {
        this.model = model;
    }

    public void render() {
        System.out.println("Current count: " + model.getCount());
    }
}

// 控制器类：处理用户点击事件
class CounterController {
    private CounterModel model;
    private CounterView view;

    public CounterController(CounterModel model, CounterView view) {
        this.model = model;
        this.view = view;
    }

    public void incrementCount() {
        model.increment();
        view.render();
    }
}

// 客户端代码：模拟用户点击事件
public class CounterApp {
    public static void main(String[] args) {
        CounterModel model = new CounterModel();
        CounterView view = new CounterView(model);
        CounterController controller = new CounterController(model, view);

        // 模拟用户点击增加按钮
        for (int i = 0; i < 5; i++) {
            controller.incrementCount();
        }
    }
}
```

在这个实用示例中，`CounterModel`类代表计数器的模型，`CounterView`类负责显示计数器的值，`CounterController`类处理用户点击增加按钮的事件。客户端代码通过控制器来模拟用户操作，控制器更新模型并通知视图进行渲染。这种方式使得用户界面和业务逻辑分离，便于维护和扩展。

 设计模式在软件开发中被广泛使用，但它们也受到了一些批评。以下是对设计模式的一些常见批评：

1. **滥用和过度工程**：设计模式是为了解决特定问题而存在的。然而，有些开发者可能会在不适当的情况下使用设计模式，或者在问题并不复杂时引入复杂的设计模式，导致过度工程和代码膨胀。

2. **学习曲线**：设计模式的概念和应用需要一定的学习和理解。对于新手开发者来说，这可能是一个挑战。此外，设计模式的术语和概念可能会让那些不熟悉它们的人感到困惑。

3. **僵化的思维**：依赖设计模式可能会导致开发者在面对问题时，首先寻找现有的模式而不是创造性地思考解决方案。这种“模式先行”的思维可能会限制创新和灵活性。

4. **语言和环境限制**：有些设计模式可能在特定的编程语言或环境中效果不佳。例如，单例模式在某些语言中可能难以实现，或者在多线程环境中出现问题。

5. **维护成本**：设计模式可能会增加代码的复杂性，这可能会在长期维护中带来额外的成本。如果设计模式没有被正确地应用或者文档化不足，后来的开发者可能会难以理解系统的结构和逻辑。

6. **不适用于所有场景**：并非所有的设计模式都适用于所有类型的软件项目。在某些情况下，简单的解决方案可能比复杂的设计模式更合适。

7. **模式之间的冲突**：在某些情况下，不同的设计模式可能会相互冲突，导致难以整合或者选择最合适的模式。

尽管存在这些批评，设计模式仍然是软件工程中的一个重要工具。它们提供了一种共同的语言和框架，帮助开发者理解和解决常见的软件设计问题。关键在于合理地选择和应用设计模式，避免上述批评中提到的问题。

 反设计模式（Anti-Pattern）是指那些在软件开发中常见的、看似合理但实际上会导致问题的设计和编程实践。它们通常看起来像是解决方案，但实际上会导致代码难以维护、性能下降、可靠性降低等问题。反设计模式与设计模式（Design Pattern）相对立，后者是经过验证的、解决特定问题的高效方法。

以下是一些常见的反设计模式及其问题：

1. **紧密耦合（Tight Coupling）**：
   - 问题：两个或多个组件之间存在强烈的依赖关系，使得一个组件的变化可能需要其他组件也进行相应的修改。
   - 解决方法：使用松耦合的设计，如接口和抽象类，以及依赖注入。

2. **无限循环（Infinite Loop）**：
   - 问题：循环没有明确的退出条件，导致程序无法正常结束。
   - 解决方法：确保循环有明确的终止条件，使用循环控制语句如`break`。

3. **全局变量（Global Variables）**：
   - 问题：过度使用全局变量可能导致代码难以理解和维护，因为全局变量可以在程序的任何地方被修改。
   - 解决方法：尽量减少全局变量的使用，改为使用局部变量和传递参数。

4. **魔法数字（Magic Numbers）**：
   - 问题：在代码中直接使用数字常量，这些数字没有明确的意义，使得代码难以理解和维护。
   - 解决方法：将数字常量替换为有意义的常量或配置项。

5. **过度工程（Over-Engineering）**：
   - 问题：在设计和实现中引入不必要的复杂性，导致系统难以理解和维护。
   - 解决方法：遵循“简单胜于复杂”的原则，只实现必要的功能。

6. **复制粘贴代码（Copy-Paste Code）**：
   - 问题：在代码中重复使用相同的代码片段，导致代码冗余和难以维护。
   - 解决方法：使用函数、类或模块来重用代码，避免复制粘贴。

7. **忽略错误处理（Ignoring Error Handling）**：
   - 问题：不处理或不适当处理错误，可能导致程序在遇到异常时崩溃或产生不可预期的行为。
   - 解决方法：实现健壮的错误处理机制，包括异常捕获、日志记录和错误恢复。

8. **过早优化（Premature Optimization）**：
   - 问题：在没有充分理解问题和需求的情况下，过早地进行性能优化，可能导致过度复杂的设计。
   - 解决方法：遵循“不要过早优化”的原则，先实现功能，再根据需要进行优化。

识别和避免这些反设计模式对于提高软件质量和可维护性至关重要。开发者应该通过代码审查、持续集成和测试来识别潜在的反设计模式，并采取措施进行改进。
