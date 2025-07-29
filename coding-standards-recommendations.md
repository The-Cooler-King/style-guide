# Python Coding Standards (Recommendations)

> These recommendations are intended to make our shared Python code **readable, maintainable, and bug‑resistant**, helping us deliver faster with fewer surprises. They are based on [Clean Code](https://github.com/jnguyen095/clean-code/blob/master/Clean.Code.A.Handbook.of.Agile.Software.Craftsmanship.pdf) principles and [PEP 8](https://peps.python.org/pep-0008/).  
>  
> They are not rigid rules, but following them will make our shared library easier to extend, test, and review.

---

## Functions Should Do One Thing

Small, focused functions are easier to test, reuse, and maintain. Avoid bundling unrelated logic together.  

```python
# Bad
def format_user_profile(user):
    full_name = f"{user['first']} {user['last']}"
    username = f"{user['first'][0].lower()}{user['last'].lower()}"
    return f"{full_name} ({username})"

# Good
def build_full_name(user):
    return f"{user['first']} {user['last']}"

def generate_username(user):
    return f"{user['first'][0].lower()}{user['last'].lower()}"

def format_user_profile(user):
    return f"{build_full_name(user)} ({generate_username(user)})"
```

---
## Log Within Shared Functions Sparingly

Shared functions may be used in many different situations, and logging may not always be appropriate or desired. If logging is needed for debugging or tracing, prefer doing it **outside** the shared function, in the calling code.  


```python
# Bad
def normalize_username(username: str) -> str:
    normalized = username.strip().lower()
    print(f"Normalized username: {normalized}")  # Logging not always wanted
    return normalized

# Good
def normalize_username(username: str) -> str:
    return username.strip().lower()

username = normalize_username(" Alice ")
logger.info(f"Normalized username: {username}")
```
Exception: Similar to the standard "Handle Errors Gracefully", log inside a shared function only when you can provide meaningful context that would be hard to attain in the calling code.

```python
def load_config(path: str) -> dict:
    try:
        with open(path) as f:
            return json.load(f)
    except FileNotFoundError as e:
        logger.error(f"Configuration file not found at {path}")
        raise
```
---
## Use Meaningful Names

Choose descriptive names for functions, variables, and arguments. Code should explain itself without extra comments. 

```python
# Bad
def calc(x, y):
    return x * y / 100

# Good
def calculate_discount(price: float, percent: float) -> float:
    return price * (1 - percent / 100)
```
Abbreviations used to be necessary to save time and space, but languages like Python compile your code and convert variables and functions into shorter names. With the compiler handling the time and space concerns, the code we write should be descriptive and explicit.

---

## Document Non‑Obvious Code

Comments should explain *why*, not *what*. Save explanations for logic that isn’t immediately clear.  

```python
# Bad
numbers.sort()
median = numbers[len(numbers) // 2]

# Good

# Sort numbers first so the middle element can be used as the median
numbers.sort()
median = numbers[len(numbers) // 2]
```

---

## Prefer Constants Over Magic Numbers

Replace “magic numbers” or strings with named constants to clarify intent and simplify updates.  

```python
# Bad
if len(password) < 8:
    raise ValueError("Password too short")

# Good
min_password_length = 8

if len(password) < min_password_length:
    raise ValueError("Password too short")
```

---

## Fail Fast and Explicitly

Raise clear errors when inputs are invalid instead of failing silently or returning defaults.  

```python
# Bad
def divide(a, b):
    return a / b if b else 0

# Good
def divide(a, b):
    if b == 0:
        raise ValueError("Denominator cannot be zero")
    return a / b
```

---
## Handle Errors Gracefully

Catch exceptions only when you can provide meaningful context, and re‑raise with clear messages.  

```python
# Bad
data = json.loads(config_text)

# Good
try:
    data = json.loads(config_text)
except json.JSONDecodeError as e:
    raise ValueError("Invalid configuration format") from e
```

---

## Test Shared Functions

Shared code should always have test coverage, especially logic that could affect multiple jobs or teams.  

- Vince has been using `unittest`, but we should move to `pytest`  
- Focus on testing business logic  

```python
def test_calculate_discount():
    assert calculate_discount(200, 10) == 180
```
