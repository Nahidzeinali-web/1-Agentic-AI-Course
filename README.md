
# 🧠 Pydantic vs Dataclass in Python – A Hands-On Guide

This guide explores data modeling in Python using both `dataclass` and `Pydantic`. Learn how `Pydantic` enhances data validation and serialization, with examples for optional fields, nested models, lists, and custom constraints.
ref: [Pydantic](https://docs.pydantic.dev/)

---

## 📍 1. Python `dataclass`: Basic Structure (No Validation)

```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    city: str

person = Person(name="Krish", age=35, city="Bangalore")
print(person)

# This still works (but shouldn't): city passed as int
person = Person(name="Krish", age=35, city=35)
print(person)
```

✅ `dataclass` provides a clean syntax for class data structures — but no runtime validation.

---

## ✅ 2. Pydantic Basics: Type-Safe Models

```python
from pydantic import BaseModel

class Person1(BaseModel):
    name: str
    age: int
    city: str

person = Person1(name="Krish", age=35, city="Bangalore")
print(person)

# Raises validation error due to invalid type
person1 = Person1(name="Krish", age=35, city=35)

# Works: city is a string
person2 = Person1(name="Krish", age=35, city="35")
print(person2)
```

📌 Pydantic automatically checks types and raises clear errors at runtime.

---

## 🧩 3. Optional Fields in Models

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

emp2 = Employee(id=2, name="Krish", department="CS", salary="30000")
print(emp2)

emp3 = Employee(id=2, name="Krish", department="CS", salary="30000", is_active=1)
print(emp3)
```

### 📘 Notes:
- `Optional[type]`: Means the field can be `None`.
- Default values make fields optional.
- If a value is provided, it must match the declared type.

---

## 👥 4. List Fields in Pydantic

```python
from typing import List
from pydantic import BaseModel

class Classroom(BaseModel):
    room_number: str
    students: List[str]
    capacity: int

# Tuple is converted to list automatically
classroom = Classroom(
    room_number="A101",
    students=("Alice", "Bob", "Charlie"),
    capacity=30
)
print(classroom)

# Raises validation error due to non-string in list
classroom1 = Classroom(
    room_number="A101",
    students=("Alice", 123, "Charlie"),
    capacity=30
)
print(classroom1)
```

You can also catch validation exceptions:

```python
try:
    invalid_val = Classroom(room_number="A1", students=["Krish", 123], capacity=30)
except Exception as e:
    print(e)
```

---

## 🏗️ 5. Nested Models with Pydantic

```python
from pydantic import BaseModel

class Address(BaseModel):
    street: str
    city: str
    zip_code: str

class Customer(BaseModel):
    customer_id: int
    name: str
    address: Address

customer = Customer(
    customer_id=1,
    name="Krish",
    address={"street": "Main street", "city": "Boston", "zip_code": "02108"}
)
print(customer)

# Will raise validation error because city is not a string
customer = Customer(
    customer_id=1,
    name="Krish",
    address={"street": "Main street", "city": 123, "zip_code": "02108"}
)
print(customer)
```

🏗️ Nested models are very useful for building complex schemas.

---

## 🛠️ 6. Custom Field Validation with `Field()`

```python
from pydantic import BaseModel, Field

class Item(BaseModel):
    name: str = Field(min_length=2, max_length=50)
    price: float = Field(gt=0, le=10000)
    quantity: int = Field(ge=0)

# Will raise error due to price > 10000
item = Item(name="Book", price=100000, quantity=10)
print(item)
```

### 🎯 Field Constraints
- `min_length`, `max_length` for strings
- `gt`, `lt`, `ge`, `le` for numbers

---

## 👤 7. Using `Field()` with Default Factories and Descriptions

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    username: str = Field(description="Unique username for the user")
    age: int = Field(default=18, description="User age default to 18")
    email: str = Field(default_factory=lambda: "user@example.com", description="Default email address")

user1 = User(username="alice")
print(user1)

user2 = User(username="bob", age=25, email="bob@domain.com")
print(user2)

# View model JSON schema
print(User.model_json_schema())
```

🧪 Pydantic models can self-document and are compatible with tools like FastAPI.

---

## 📊 Summary Comparison

| Feature                          | `dataclass` | `Pydantic` |
|----------------------------------|-------------|------------|
| Runtime Type Checking            | ❌          | ✅         |
| Type Coercion                    | ❌          | ✅         |
| Field Constraints                | ❌          | ✅         |
| Nested Models                    | ❌          | ✅         |
| Optional Field Defaults          | ✅          | ✅         |
| Serialization & Schema Export   | ❌          | ✅         |
| FastAPI Compatibility            | ❌          | ✅         |

---

## ✅ Conclusion

If you're dealing with **API inputs**, **data validation**, or **schema generation**, use **Pydantic**. It's robust, flexible, and integrates beautifully with frameworks like **FastAPI** and **LangChain**.

For lightweight, internal data structures without validation needs, `dataclasses` still work well.

---

🧠 **Try replacing `dataclass` with `Pydantic` in your projects — and see the difference.**
