# Challenge 01 — Text Inspector

**Difficulty:** Beginner  
**Language:** Python 3  
**Suggested filename:** `text_inspector.py`

## Objective

Build a command-line program that analyzes a string using functions, loops, and indexing.

## Required Functions

Implement the following functions:

```python
def my_count(string, char, case_insensitive=False):
    pass

def my_index(string, char):
    pass

def my_length(string):
    pass

def my_upper(string):
    pass

def my_lower(string):
    pass
```

### Functional Requirements

1. `my_count()` returns the number of occurrences of a character. It must support optional case-insensitive counting.
2. `my_index()` returns the index of the first occurrence, or `-1` if not found.
3. `my_length()` calculates the length of a string.
4. `my_upper()` returns the uppercase version of a string.
5. `my_lower()` returns the lowercase version of a string.
6. All functions must return values, not print them.

## Rules and Restrictions

**Allowed:**
- `for` and `while` loops
- Conditions
- String indexing
- Functions and return values
- `.upper()` and `.lower()`
- `sys.argv`

**Not allowed:**
- `.count()` for character counting
- `.find()` or `.index()` for searching
- `len()` inside `my_length()`
- External libraries

## Command-Line Interface

Your program must accept exactly two arguments: a text string and a target character.

Example:

```bash
python text_inspector.py "Hello" "l"
```

Expected output:

```text
Original: Hello
Uppercase: HELLO
Lowercase: hello
Length: 5
First occurrence of 'l': 2
Total occurrences of 'l': 2
```

If the user supplies the wrong number of arguments or a target that isn't exactly one character long, display a clear error message.

## Public Tests

| Function Call | Expected Result |
|---|---|
| `my_count("banana", "a")` | `3` |
| `my_count("Banana", "A")` | `0` |
| `my_count("Banana", "A", True)` | `3` |
| `my_count("hello", "x")` | `0` |
| `my_index("Hello", "l")` | `2` |
| `my_index("Hello", "x")` | `-1` |
| `my_length("Python")` | `6` |
| `my_length("")` | `0` |
| `my_upper("aBc!")` | `"ABC!"` |
| `my_lower("PyTHon")` | `"python"` |

## Additional CLI Tests

**Invalid target:**

```bash
python text_inspector.py "Hello" "ll"
```

Expected: A clear error explaining that the target must contain exactly one character.

**Missing argument:**

```bash
python text_inspector.py "Hello"
```

Expected: An argument-count error.

## Completion Criteria

- All required functions work correctly.
- All public tests pass.
- Invalid CLI input is handled gracefully.
- All restrictions are respected.

**Bonus:** Add a command-line option for case-insensitive counting.

---

**Solve the challenge independently. Do not hardcode the expected outputs.**
