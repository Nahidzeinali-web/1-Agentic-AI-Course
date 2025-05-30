# 🤖 Why I Switched from `dataclass` to `pydantic` for Robust Data Validation in Python

If you're like me and love writing clean, type-hinted Python code, you've probably used `@dataclass` from the standard library. It's elegant and concise, but it doesn’t validate types at runtime. That’s where [Pydantic](https://docs.pydantic.dev/) enters the scene, and this post is about how I discovered its real-world advantages.

---

## 🔍 The Problem with `dataclass`

Let’s consider this simple example:

```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    city: str

person = Person(name="Nahid", age=38, city="San Diego")
print(person)

# No error! But city is an int?
Person(name="Nahid", age=38, city="35")
print(person)
```

**The result?** Python happily accepts `city=35` even though it’s meant to be a string. This can silently cause bugs downstream, especially in larger systems.

---

## ✅ Enter Pydantic: Type Validation You Can Trust

Now let’s try using Pydantic instead:

```python
from pydantic import BaseModel

class Person1(BaseModel):
    name: str
    age: int
    city: str

person = Person1(name="Nahid", age=38, city="35")
print(person)

# Raises a validation error!
person = Person1(name="Nahid", age=38, city="San Diego")
```

Pydantic enforces type hints **at runtime** and provides clear error messages when something doesn’t match. It also supports type coercion when appropriate.

---

## 🎯 Optional Fields and Defaults

You can easily define optional fields with default values:

```python
from typing import Optional
from pydantic import BaseModel

class Employee(BaseModel):
    id: int
    name: str
    department: str
    salary: Optional[float] = None
    is_active: Optional[bool] = True

emp1 = Employee(id=1, name="John", department="CS")
print(emp1)
```

Even when a value comes in as a string, Pydantic can convert it:

```python
emp2 = Employee(id=2, name="Nahid", department="CS", salary="30000")
print(emp2)  # salary becomes float
```

---

## ⚙️ Why This Matters

With `dataclass`, you have to validate types or write extra checks manually. Pydantic makes it:

- ✅ **Safe**: Validates fields automatically  
- ✅ **Clear**: Provides great error reporting  
- ✅ **Smart**: Coerces values when possible  
- ✅ **Scalable**: Fits perfectly into real-world applications like FastAPI, ETL, and ML pipelines

---

## 🚀 Final Thoughts

After switching to Pydantic, my codebase became cleaner, more robust, and easier to debug.

If you care about reliability, especially when working with external data or APIs, Pydantic is a no-brainer.

Have you used Pydantic in your projects? I’d love to hear how it improved your workflow. 👇
