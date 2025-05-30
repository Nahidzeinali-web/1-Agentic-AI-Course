
# 🧠 Pydantic vs Dataclass in Python + LangChain Agentic AI Tutorial

This guide provides a comprehensive walkthrough on:
- Using `dataclass` and `pydantic` for structured, validated data modeling
- Creating LLM-based apps using LangChain, OpenAI, Groq, and structured output parsing

🔗 Reference: [Pydantic Docs](https://docs.pydantic.dev/) | Course: Krish Naik's AI Series

---

## 📚 Table of Contents

- [1. Python `dataclass`: Basic Structure](#1-python-dataclass-basic-structure-no-validation)
- [2. Pydantic Basics](#2-pydantic-basics-type-safe-models)
- [3. Optional Fields in Pydantic](#3-optional-fields-in-models)
- [4. List Fields in Pydantic](#4-list-fields-in-pydantic)
- [5. Nested Models with Pydantic](#5-nested-models-with-pydantic)
- [6. Field Constraints with `Field()`](#6-custom-field-validation-with-field)
- [7. Default Factories and Descriptions](#7-using-field-with-default-factories-and-descriptions)
- [8. Pydantic vs Dataclass Summary](#8-summary-comparison)
- [9. LangChain Agentic AI Tutorial](#9-langchain-agentic-ai-tutorial)
- [10. Loading API Keys](#10-loading-environment-variables)
- [11. Using OpenAI & Groq](#11-using-openai-and-groq-llms)
- [12. Prompt Templates](#12-prompt-engineering-with-templates)
- [13. Prompt + Model Chaining](#13-chaining-prompt-with-model)
- [14. Output Parsers](#14-output-parsing)
- [15. Structured Output: JSON/XML/YAML](#15-structured-outputs-with-pydantic)
- [16. Final Assignment: Product Assistant](#16-final-assignment--product-assistant)

---

## 1. Python `dataclass`: Basic Structure (No Validation)
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    city: str

person = Person(name="Nahid", age=35, city="San Diego")
print(person)

# This still works (but shouldn't): city passed as int
person = Person(name="Krish", age=35, city=35)
print(person)
```

✅ Simple, lightweight, but no type enforcement at runtime.

---

## 2. Pydantic Basics: Type-Safe Models
```python
from pydantic import BaseModel

class Person1(BaseModel):
    name: str
    age: int
    city: str

person = Person1(name="Nahid", age=35, city="San Diego")
print(person)

person1 = Person1(name="Nahid", age=35, city=35)  # ❌ raises ValidationError
```

📌 Strict type checking with automatic parsing.

---

## 3. Optional Fields in Models
```python
from typing import Optional
from pydantic import BaseModel

class Employee(BaseModel):
    id: int
    name: str
    department: str
    salary: Optional[float] = None
    is_active: Optional[bool] = True
```

📘 Notes:
- Optional fields can be omitted.
- If provided, they must match expected types.

---

## 4. List Fields in Pydantic
```python
from typing import List

class Classroom(BaseModel):
    room_number: str
    students: List[str]
    capacity: int
```

Handles list validation and provides meaningful errors.

---

## 5. Nested Models with Pydantic
```python
class Address(BaseModel):
    street: str
    city: str
    zip_code: str

class Customer(BaseModel):
    customer_id: int
    name: str
    address: Address
```

🏗️ Useful for hierarchical data structures.

---

## 6. Custom Field Validation with `Field()`
```python
class Item(BaseModel):
    name: str = Field(min_length=2, max_length=50)
    price: float = Field(gt=0, le=10000)
    quantity: int = Field(ge=0)
```

🎯 Define input rules with clarity.

---

## 7. Using `Field()` with Default Factories and Descriptions
```python
class User(BaseModel):
    username: str = Field(description="Unique username for the user")
    age: int = Field(default=18)
    email: str = Field(default_factory=lambda: "user@example.com")
```

🧪 Great for OpenAPI & FastAPI.

---

## 8. Summary Comparison

| Feature                        | `dataclass` | `Pydantic` |
|-------------------------------|-------------|------------|
| Runtime Type Checking         | ❌          | ✅         |
| Type Coercion                 | ❌          | ✅         |
| Field Constraints             | ❌          | ✅         |
| Nested Models                 | ❌          | ✅         |
| Optional Field Defaults       | ✅          | ✅         |
| Serialization & Schema Export| ❌          | ✅         |
| FastAPI Integration           | ❌          | ✅         |

---

## 9. LangChain Agentic AI Tutorial

### 10. Loading Environment Variables
```python
from dotenv import load_dotenv
import os

load_dotenv()
os.environ["OPENAI_API_KEY"] = os.getenv("OPENAI_API_KEY")
```

---

### 11. Using OpenAI and Groq LLMs
```python
from langchain_openai import ChatOpenAI
from langchain_groq import ChatGroq
```

---

### 12. Prompt Engineering with Templates
```python
from langchain_core.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_messages([...])
```

---

### 13. Chaining Prompt with Model
```python
chain = prompt | model
chain.invoke({"input": "What is Langsmith?"})
```

---

### 14. Output Parsing
- `StrOutputParser`
- `JsonOutputParser`
- `XMLOutputParser`
- `YamlOutputParser`

---

## 15. Structured Outputs with Pydantic

```python
from pydantic import BaseModel, Field

class Joke(BaseModel):
    setup: str
    punchline: str
```

### 16. Final Assignment – Product Assistant

```python
class ProductInfo(BaseModel):
    product_name: str
    product_details: str
    tentative_price_usd: int
```

🔧 Use LangChain, ChatPromptTemplate, and Pydantic to build a structured product info assistant.

---

🧠 **Try integrating `pydantic` into your AI apps to ensure reliable and structured output.**
