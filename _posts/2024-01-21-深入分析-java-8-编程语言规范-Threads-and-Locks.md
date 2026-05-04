---
layout: single
title: "深入分析 java 8 编程语言规范：Threads and Locks"
categories: Java-并发
date: 2024-01-21
---

> 关于后面的几个小节，我自己对其理解不够，也不希望误导大家，如果大家感兴趣的话，请参阅其他资料。

#### 17.4.6. Executions

> 未完成

#### 17.4.7. Well-Formed Executions

> 未完成

#### 17.4.8. Executions and Causality Requirements

> 未完成

#### 17.4.9. Observable Behavior and Nonterminating Executions

> 未完成

### 17.5. final 属性的语义（final Field Semantics）

> 我们经常使用 final，关于它最基础的知识是：用 final 修饰的类不可以被继承，用 final 修饰的方法不可以被覆写，用 final 修饰的属性一旦初始化以后不可以被修改。
>
> 当然，这节说的不是这些，这里将阐述 final 关键字的深层次含义。

用 final 声明的属性正常情况下初始化一次后，就不会被改变。final 属性的语义与普通属性的语义有一些不一样。尤其是，对于 final 属性的读操作，compilers 可以自由地去除不必要的同步。相应地，compilers 可以将 final 属性的值缓存在寄存器中，而不用像普通属性一样从内存中重新读取。

final 属性同时也允许程序员不需要使用同步就可以实现**线程安全**的**不可变对象**。一个线程安全的不可变对象对于所有线程来说都是不可变的，即使传递这个对象的引用存在数据竞争。这可以提供安全的保证，即使是错误的或者恶意的对于这个不可变对象的使用。如果需要保证对象不可变，需要正确地使用 final 属性域。

对象只有在构造方法结束了才被认为`完全初始化`了。如果一个对象**完全初始化**以后，一个线程持有该对象的引用，那么这个线程一定可以看到正确初始化的 final 属性的值。

> 这个隐含了，如果属性值不是 final 的，那就不能保证一定可以看到正确初始化的值，可能看到初始零值。

final 属性的使用是非常简单的：在对象的构造方法中设置 final 属性；同时在对象初始化完成前，不要将此对象的引用写入到其他线程可以访问到的地方。如果这个条件满足，当其他线程看到这个对象的时候，那个线程始终可以看到正确初始化后的对象的 final 属性。It will also see versions of any object or array referenced by those `final` fields that are at least as up-to-date as the `final` fields are.

> 这里面说到了一个**正确初始化**的问题，看过《Java并发编程实战》的可能对这个会有印象，不要在构造方法中将 this 发布出去。



---

**Example 17.5-1. final Fields In The Java Memory Model**

这段代码把final属性和普通属性进行对比。

```java
class FinalFieldExample { 
    final int x;
    int y; 
    static FinalFieldExample f;

    public FinalFieldExample() {
        x = 3; 
        y = 4; 
    } 

    static void writer() {
        f = new FinalFieldExample();
    } 

    static void reader() {
        if (f != null) {
            int i = f.x;  // 程序一定能得到 3  
            int j = f.y;  // 也许会看到 0
        } 
    } 
}
```

这个类`FinalFieldExample`有一个 final 属性 x 和一个普通属性 y。我们假定有一个线程执行 writer() 方法，另一个线程再执行 reader() 方法。

因为 writer() 方法在对象完全构造后将引用写入 f，那么 reader() 方法将一定可以看到初始化后的 f.x : 将读到一个 int 值 3。然而， f.y 不是 final 的，所以程序不能保证可以看到 4，可能会得到 0。

---

---

**Example 17.5-2. final Fields For Security**

final 属性被设计成用来保障很多操作的安全性。

考虑以下代码，线程 1 执行：

```java
Global.s = "/tmp/usr".substring(4);
```

同时，线程 2 执行：

```java
String myS = Global.s; 
if (myS.equals("/tmp")) System.out.println(myS);
```

**String** 对象是不可变对象，同时 String 操作不需要使用同步。虽然 String 的实现没有任何的数据竞争，但是其他使用到 String 对象的代码可能是存在数据竞争的，内存模型没有对存在数据竞争的代码提供安全性保证。特别是，如果 String 类中的属性不是 final 的，那么有可能（虽然不太可能）线程 2 会看到这个 string 对象的 offset 为初始值 0，那么就会出现 myS.equals("/tmp")。之后的一个操作可能会看到这个 String 对象的正确的 offset 值 4，那么会得到 “/usr”。Java 中的许多安全特性都依赖于 String 对象的不可变性，即使是恶意代码在数据竞争的环境中在线程之间传递 String 对象的引用。

> 大家看这段的时候，如果要看代码，请注意，这里说的是  JDK6 及以前的 String 类：

```java
public final class String  
    implements java.io.Serializable, Comparable<String>, CharSequence  
{  
    /** The value is used for character storage. */  
    private final char value[];  
  
    /** The offset is the first index of the storage that is used. */  
    private final int offset;  
  
    /** The count is the number of characters in the String. */  
    private final int count;  
  
    /** Cache the hash code for the string */  
    private int hash; // Default to 0  
```

> 因为到 JDK7 和 JDK8 的时候，代码已经变为：

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];

    /** Cache the hash code for the string */
    private int hash; // Default to 0

    /** use serialVersionUID from JDK 1.0.2 for interoperability */
    private static final long serialVersionUID = -6849794470754667710L;
```

---

#### 17.5.1. final属性的语义（Semantics of final Fields）

令 o 为一个对象，c 为 o 的构造方法，构造方法中对 final 的属性 f 进行写入值。当构造方法 c 退出的时候，会在final 属性 f 上执行一个 freeze 操作。

注意，如果一个构造方法调用了另一个构造方法，在被调用的构造方法中设置 final 属性，那么对于 final 属性的 freeze 操作发生于被调用的构造方法结束的时候。

> 我没懂这边的 freeze 操作是什么。

对于每一个执行，读操作的行为被其他的两个偏序影响，解引用链 *dereferences()* 和内存链 *mc()*，它们被认为是执行的一部分。这些偏序必须满足下面的约束：

> 我对于解引用链和内存链完全不熟悉，所以下面这段我就不翻译了。

- Dereference Chain: If an action *a* is a read or write of a field or element of an object *o* by a thread *t* that did not initialize *o*, then there must exist some read *r* by thread *t* that sees the address of *o* such that *r* *dereferences(r, a)*.

- Memory Chain: There are several constraints on the memory chain ordering:
  - If *r* is a read that sees a write *w*, then it must be the case that *mc(w, r)*.
  - If *r* and *a* are actions such that *dereferences(r, a)*, then it must be the case that *mc(r, a)*.
  - If *w* is a write of the address of an object *o* by a thread *t* that did not initialize *o*, then there must exist some read *r* by thread *t* that sees the address of *o* such that *mc(r, w)*.

Given a write *w*, a freeze *f*, an action *a* (that is not a read of a `final` field), a read *r1* of the `final` field frozen by *f*, and a read *r2* such that *hb(w, f)*, *hb(f, a)*, *mc(a, r1)*, and *dereferences(r1, r2)*, then when determining which values can be seen by *r2*, we consider *hb(w, r2)*. (This *happens-before* ordering does not transitively close with other *happens-before* orderings.)

Note that the *dereferences* order is reflexive, and *r1* can be the same as *r2*.

For reads of `final` fields, the only writes that are deemed to come before the read of the `final` field are the ones derived through the `final` field semantics.

#### 17.5.2. 在构造期间读 final 属性（Reading final Fields During Construction）

在构造对象的线程中，对该对象的 final 属性的读操作，遵守正常的 happens-before 规则。如果在构造方法内，读某个 final 属性晚于对这个属性的写操作，那么这个读操作可以看到这个 final 属性已经被定义的值，否则就会看到默认值。

#### 17.5.3. final 属性的修改（Subsequent Modification of final Fields）

在许多场景下，如反序列化，系统需要在对象构造之后改变 final 属性的值。final 属性可以通过反射和其他方法来改变。唯一的具有合理语义的模式是：对象被构造出来，然后对象中的 final 属性被更新。在这个对象的所有 final 属性更新操作完成之前，此对象不应该对其他线程可见，也不应该对 final 属性进行读操作。对于 final 属性的 freeze 操作发生于**构造方法的结束，这个时候 final 属性已经被设值**，还有**通过反射或其他方式对于 final 属性的更新之后**。

即使是这样，依然存在几个难点。如果一个 final 属性在属性声明的时候初始化为一个常量表达式，对于这个 final 属性值的变化过程也许是不可见的，因为对于这个 final 属性的使用是在编译时用常量表达式来替换的。

另一个问题是，该规范允许 JVM 实现对 final 属性进行强制优化。在一个线程内，允许**对于 final 属性的读操作**与**构造方法之外的对于这个 final 属性的修改**进行重排序。

---

**Example 17.5.3-1. 对于 final 属性的强制优化（Aggressive Optimization of final Fields**）

```java
class A {
    final int x;
    A() { 
        x = 1; 
    } 

    int f() { 
        return d(this,this); 
    } 

    int d(A a1, A a2) { 
        int i = a1.x; 
        g(a1); 
        int j = a2.x; 
        return j - i; 
    }

    static void g(A a) { 
    	// 利用反射将 a.x 的值修改为 2
        // uses reflection to change a.x to 2 
    } 
}
```

在方法 d 中，编译器允许对 x 的读操作和方法 g 进行重排序，这样的话，`new A().f()`可能会返回 -1, 0, 或 1。

> 我在我的 MBP 上试了好多办法，真的没法重现出来，不过并发问题就是这样，我们不能重现不代表不存在。StackOverflow 上有网友说在 Sparc 上运行，可惜我没有 Sparc 机器。

---



> 下文将说到一个比较少见的 **final-field-safe context**

JVM 实现可以提供一种方式在 **final 属性安全**上下文（final-field-safe context）中执行代码块。如果一个对象是在 *final 属性安全上下文*中构造出来的，那么在这个 *final 属性安全上下文 *中对于 final 属性的读操作不会和相应的对于 final 属性的修改进行重排序。

*final 属性安全上下文*还提供了额外的保障。如果一个线程已经看到一个不正确发布的一个对象的引用，那么此线程可以看到了 final 属性的默认值，然后，在 *final 属性安全上下文*中读取该对象的正确发布的引用，这可以保证看到正确的 final 属性的值。在形式上，在*final 属性安全上下文*中执行的代码被认为是一个独立的线程（仅用于满足 final 属性的语义）。

在实现中，compiler 不应该将对 final 属性的访问移入或移出*final 属性安全上下文*（尽管它可以在这个执行上下文的周边移动，只要这个对象没有在这个上下文中进行构造）。

对于 *final 属性安全上下文*的使用，一个恰当的地方是执行器或者线程池。在每个独立的 *final 属性安全上下文*中执行每一个 `Runnable`，执行器可以保证在一个 `Runnable` 中对对象 o 的不正确的访问不会影响同一执行器内的其他 `Runnable` 中的 final 带来的安全保障。

#### 17.5.4. 写保护属性（Write-Protected Fields）

通常，如果一个属性是 `final` 的和 `static` 的，那么这个属性是不会被改变的。但是， `System.in`, `System.out`, 和 `System.err` 是 `static final` 的，出于遗留的历史原因，它们必须允许被 `System.setIn`, `System.setOut`, 和 `System.setErr` 这几个方法改变。我们称这些属性是**写保护**的，用以区分普通的 final 属性。

```java
    public final static InputStream in = null;
    public final static PrintStream out = null;
    public final static PrintStream err = null;
```

编译器需要将这些属性与 final 属性区别对待。例如，普通 final 属性的读操作对于同步是“免疫的”：锁或 volatile 读操作中的内存屏障并不会影响到对于 final 属性的读操作读到的值。因为写保护属性的值是可以被改变的，所以同步事件应该对它们有影响。因此，语义规定这些属性被当做普通属性，不能被用户的代码改变，除非是 `System`类中的代码。

### 17.6. 字分裂（Word Tearing）

实现 Java 虚拟机需要考虑的一件事情是，每个对象属性以及数组元素之间是独立的，更新一个属性或元素不能影响其他属性或元素的读取与更新。尤其是，两个线程在分别更新 byte 数组相邻的元素时，不能互相影响与干扰，且不需要同步来保证连续一致性。

一些处理器不提供写入单个字节的能力。 通过简单地读取整个字，更新相应的字节，然后将整个字写入内存，用这种方式在这种处理器上实现字节数组更新是非法的。 这个问题有时被称为字分裂（word tearing），在这种不能单独更新单个字节的处理器上，将需要寻求其他的方法。

> 请注意，对于大部分处理器来说，都没有这个问题

---

**Example 17.6-1. Detection of Word Tearing**

以下程序用于测试是否存在字分裂：

```java
public class WordTearing extends Thread {
    static final int LENGTH = 8;
    static final int ITERS = 1000000;
    static byte[] counts = new byte[LENGTH];
    static Thread[] threads = new Thread[LENGTH];

    final int id;

    WordTearing(int i) {
        id = i;
    }

    public void run() {
        byte v = 0;
        for (int i = 0; i < ITERS; i++) {
            byte v2 = counts[id];
            if (v != v2) {
                System.err.println("Word-Tearing found: " +
                        "counts[" + id + "] = " + v2 +
                        ", should be " + v);
                return;
            }
            v++;
            counts[id] = v;
        }
        System.out.println("done");
    }

    public static void main(String[] args) {
        for (int i = 0; i < LENGTH; ++i)
            (threads[i] = new WordTearing(i)).start();
    }
}
```

这表明写入字节时不得覆写相邻的字节。

---

### 17.7. double 和 long 的非原子处理 （Non-Atomic Treatment of double and long）

在Java内存模型中，对于 non-volatile 的 long 或 double 值的写入是通过两个单独的写操作完成的：long 和 double 是 64 位的，被分为两个 32 位来进行写入。那么可能就会导致一个线程看到了某个操作的低 32 位的写入和另一个操作的高 32 位的写入。

写入或者读取 volatile 的 long 和 double 值是原子的。

写入和读取对象引用一定是原子的，不管具体实现是32位还是64位。

将一个 64 位的 long 或 double 值的写入分为相邻的两个 32 位的写入对于 JVM 的实现来说是很方便的。为了性能上的考虑，JVM 的实现是可以决定采用原子写入还是分为两个部分写入的。

如果可能的话，我们鼓励 JVM 的实现避开将 64 位值的写入分拆成两个操作。我们也希望程序员将共享的 64 位值操作设置为 volatile 或者使用正确的同步，这样可以提供更好的兼容性。

> 目前来看，64 位虚拟机对于 long 和 double 的写入都是原子的，没必要加 volatile 来保证原子性。



（全文完）

### References

**官方原文**：https://docs.oracle.com/javase/specs/

**JSR 133 (Java Memory Model) FAQ**: http://www.cs.umd.edu/~pugh/java/memoryModel/jsr-133-faq.html

**The JSR-133 Cookbook for Compiler Writers**： http://gee.cs.oswego.edu/dl/jmm/cookbook.html

> 这是 Doug Lea 大神写的，属于更深层次的实现上的解读了，如果大家有需要的话，我后续也许可以整理整理。



### 结语

路还很长，如果有机会，我会在其中挑出一些 topic 出来和大家分享我自己的理解。