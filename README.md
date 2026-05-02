# Where-is-class-stored-in-Java-memory-
Where is class stored in Java memory?
Where Java Classes Are Stored in Memory
A Java class is not stored in the same memory area as ordinary objects. When the JVM loads a class, it keeps the class's metadata in a special runtime area managed by the JVM, while ordinary instances created with new live on the heap.

Class file on disk
Before a class is loaded, it exists as a .class file on disk that contains Java bytecode and related symbolic information.
 The JVM reads this file through a class loader and turns it into runtime structures it can execute and manage.

Loaded class in JVM memory
Once loaded, the class definition is stored as class metadata in a JVM-managed memory area reserved for class information, such as method bytecode, runtime constant-pool data, and structural details about fields and methods.
**Note: In older JVMs this area was commonly described as PermGen, while in Java 8 and later it is known as Metaspace.**

This area is distinct from the normal object heap in the usual mental model of Java memory, even though it is still ultimately backed by system RAM during program execution.

\That is why saying “classes live in Metaspace, objects live in the heap” is a useful simplification for learning Java memory layout.
​
<img width="2814" height="1536" alt="Gemini_Generated_Image_61kymg61kymg61ky" src="https://github.com/user-attachments/assets/24bbf05a-c04f-471b-b4b0-92f6fc30e8f5" />

​A practical mental model
The easiest way to picture Java class storage is this:

.class file on disk → bytecode stored in a file.

Loaded class → metadata stored in JVM class memory:

In Java 7 and earlier: PermGen (Permanent Generation).

In Java 8 and later: Metaspace.

Person.class → a Class object available to the program at runtime, typically on the heap.

new Person() → a normal object instance stored on the heap.

So the key simplification is:

Class definitions (metadata, methods, statics) live in PermGen (Java 7 and earlier) or Metaspace (Java 8+) in the JVM;
the Class object (e.g., Person.class) typically lives on the heap.

That Java‑8 cutoff is important because it’s exactly when the JVM switched from PermGen to Metaspace for class‑metadata storage.

What lives there
The loaded form of a class includes several kinds of runtime information: method definitions, constant-pool data, field and method descriptors, and other metadata the JVM needs for linking and execution.
 Static members are associated with the class rather than with any one object instance, which is one reason class-level storage is discussed separately from per-object heap storage.

The Class object
Each loaded Java class also has a corresponding java.lang.Class object, such as Person.class.
 That Class object is itself an ordinary object used by reflection and runtime type inspection, and it generally resides on the heap while referring to the underlying class metadata maintained by the JVM.

A practical mental model
The easiest way to picture Java class storage is this:

.class file on disk → bytecode stored in a file.

Loaded class → metadata stored in JVM class memory such as Metaspace or, in older JVMs, PermGen.

Person.class → a Class object available to the program at runtime, typically on the heap.

new Person() → a normal object instance stored on the heap.
​
​

Why this distinction matters
This distinction helps explain why many objects can share the same class definition without duplicating method code for every instance.
​
​ It also explains why problems involving too many loaded classes are discussed differently from ordinary heap-memory issues, because class metadata and object instances are managed in different runtime areas.
