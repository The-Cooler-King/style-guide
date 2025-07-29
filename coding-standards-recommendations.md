# Python Coding Standards (Recommendations)

> These recommendations are intended to make our shared Python code **readable, maintainable, and bug‑resistant**, helping us deliver faster with fewer surprises. They are based on Clean Code principles and [PEP 8](https://peps.python.org/pep-0008/).  
>  
> They are not rigid rules, but following them will make our shared library easier to extend, test, and review.

---

## Functions Should Do One Thing

Small, focused functions are easier to test, reuse, and maintain. Avoid bundling unrelated logic together.  

```python
# Bad
def process_order(order):
    validate(order)
    save_to_database(order)
    send_confirmation_email(order)
    update_inventory(order)

# Good
def validate_order(order):
    ...

def save_order(order):
    ...

def notify_customer(order):
    ...

def update_inventory(order):
    ...
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

---

## Document Non‑Obvious Code

Comments should explain *why*, not *what*. Save explanations for logic that isn’t immediately clear.  

```python
# Bad
result = sorted(users, key=lambda u: (u[1], u[0]))

# Good
# Sort users by last name, then by first name
result = sorted(users, key=lambda u: (u.last_name, u.first_name))
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

if len(password) < MIN_PASSWORD_LENGTH:
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
