## C# Delegates

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