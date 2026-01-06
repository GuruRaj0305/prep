# C# Delegates

A delegate is a type-safe reference to a method that can be invoked later.
+ It stores method reference, not execution
+ The method must match:
   + Parameter types
   + Return type
``` csharp
delegate int Operation(int a, int b);
```

### Why Delegates Exist
Delegates solve decoupling.

They allow:
+ Passing behavior as data
+ Choosing logic at runtime
+ Letting caller decide, not callee

Delegates answer:
“I don’t know which method now — but I’ll call it later.”

---

# .NET Garbage Collector (GC)


## What Problem GC Solves

Programs allocate memory.
Memory is limited.
Developers forget to free memory.

> **Garbage Collector (GC)** automatically reclaims memory occupied by objects that are **no longer reachable**.

GC is about **memory management**, not resource management.

---

## Core Rule of Garbage Collection (MOST IMPORTANT)

> **If an object is reachable → it stays alive**  
> **If an object is unreachable → it is eligible for collection**

- GC does NOT care about:
  - Variable names
  - Scope blocks
  - “I’m done using it”
- GC ONLY cares about **references**

---

## GC Is NOT Immediate

- Objects are NOT destroyed as soon as they go out of scope
- GC runs **when the runtime decides**, not when you want
- GC trades **performance** for **safety**

Calling:
```csharp
GC.Collect();
```

## Generations in .NET GC

### Generation 0 (Gen 0)

- Newly allocated objects  
- Very small  
- Collected **frequently**  
- Very fast to collect  

👉 Temporary, short-lived objects

---

### Generation 1 (Gen 1)

- Objects that survived Gen 0  
- Acts as a **buffer**  
- Collected less often  

👉 Medium-lived objects

---

### Generation 2 (Gen 2)

- Objects that survived Gen 1  
- Long-lived objects  
- Collected **rarely**  
- Expensive to collect  

👉 Important, stable objects

---

## Object Promotion Rule

> **Survival = Promotion**

- Survive Gen 0 → promoted to Gen 1  
- Survive Gen 1 → promoted to Gen 2  

Promotion happens automatically.

---

## Why Generations Improve Performance

- GC checks **Gen 0 first**  
- Most garbage is found there  
- Small memory region → fast cleanup  
- Avoids scanning long-lived objects repeatedly  

Gen 2 collection happens only under **high memory pressure**.

---

## Stop-the-World Behavior

During GC:

- Application threads are paused  
- Execution stops temporarily  

Pause duration:

- Gen 0 → very short  
- Gen 2 → noticeable  

> Excessive allocations or long-lived objects = performance issues

---

## Large Object Heap (LOH)

- Objects **larger than ~85 KB**  
- Allocated on **Large Object Heap**  
- Treated like Gen 2  
- Collected rarely  
- Can cause fragmentation  

Examples:

- Large arrays  
- Big buffers  
- Image data  