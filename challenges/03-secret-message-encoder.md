# Challenge 03 — Secret Message Encoder

**Difficulty:** Hard  
**Language:** Python 3  
**Suggested filename:** `cipher.py`

## Objective

Build a command-line encryption and decryption application using the Caesar cipher.

This challenge focuses on algorithm design, character indexing, modular arithmetic, function reuse, and edge-case handling.

## What Is a Caesar Cipher?

A Caesar cipher replaces each letter with another letter a fixed number of positions away in the alphabet.

For example, with a shift of `3`:

```text
Original:   Hello World!
Encrypted:  Khoor Zruog!
Decrypted:  Hello World!
```

The letter `H` becomes `K`, `e` becomes `h`, and `Z` wraps around to `C`.

Spaces, numbers, punctuation, and capitalization must be preserved.

## Required Functions

Implement:

```python
def find_letter_position(letter, alphabet):
    pass

def shift_letter(letter, shift, alphabet):
    pass

def encrypt_message(message, shift):
    pass

def decrypt_message(message, shift):
    pass
```

Use these alphabets:

```python
upper_alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
lower_alphabet = "abcdefghijklmnopqrstuvwxyz"
```

### 1. Character Position

`find_letter_position(letter, alphabet)` must:

- Search the alphabet for an exact character match.
- Return the matching zero-based index.
- Return `-1` when the character isn't found.
- Reject multi-character matches by returning `-1`.

### 2. Character Shifting

`shift_letter(letter, shift, alphabet)` must:

- Find the character's original position.
- Shift its position forward or backward.
- Wrap around the alphabet when necessary.
- Return the shifted character.
- Return the original input unchanged when no exact match exists.
- Reuse `find_letter_position()`.

### 3. Message Encryption

`encrypt_message(message, shift)` must:

- Encrypt an entire string.
- Preserve uppercase and lowercase letters.
- Leave spaces, numbers, and punctuation unchanged.
- Support positive, negative, zero, and large integer shifts.
- Return the encrypted string.
- Reuse `shift_letter()`.

### 4. Message Decryption

`decrypt_message(message, shift)` must:

- Reverse the encryption.
- Restore the original text using the correct shift.
- Preserve capitalization and nonalphabetic characters.
- Reuse the existing encryption algorithm rather than duplicating it.

## Rules and Restrictions

**Allowed:**

- `for` and `while` loops
- String indexing
- Functions and return values
- Arithmetic and modulo (`%`)
- Conditional statements
- String concatenation
- `len()`
- `sys.argv`
- `int()` and `try/except`

**Not allowed:**

- `ord()` or `chr()`
- `.find()` or `.index()`
- `.replace()` to implement encryption
- `str.translate()` or `str.maketrans()`
- External encryption packages
- Hardcoded character-to-character mappings
- Duplicating the encryption algorithm to implement decryption

The encryption logic must be implemented from scratch.

## Public Function Tests

### Character Position

| Function Call | Expected Result |
|---|---|
| `find_letter_position("A", upper_alphabet)` | `0` |
| `find_letter_position("D", upper_alphabet)` | `3` |
| `find_letter_position("Z", upper_alphabet)` | `25` |
| `find_letter_position("!", upper_alphabet)` | `-1` |
| `find_letter_position("a", upper_alphabet)` | `-1` |
| `find_letter_position("AB", upper_alphabet)` | `-1` |

### Single-Character Shifting

| Function Call | Expected Result |
|---|---|
| `shift_letter("A", 3, upper_alphabet)` | `"D"` |
| `shift_letter("Y", 3, upper_alphabet)` | `"B"` |
| `shift_letter("Z", 1, upper_alphabet)` | `"A"` |
| `shift_letter("A", -1, upper_alphabet)` | `"Z"` |
| `shift_letter("B", 27, upper_alphabet)` | `"C"` |
| `shift_letter("!", 3, upper_alphabet)` | `"!"` |
| `shift_letter("AB", 3, upper_alphabet)` | `"AB"` |
| `shift_letter("", 3, upper_alphabet)` | `""` |

### Message Encryption

| Function Call | Expected Result |
|---|---|
| `encrypt_message("HELLO", 3)` | `"KHOOR"` |
| `encrypt_message("Hello World!", 3)` | `"Khoor Zruog!"` |
| `encrypt_message("XYZ", 3)` | `"ABC"` |
| `encrypt_message("abc", -1)` | `"zab"` |
| `encrypt_message("Python 123!", 5)` | `"Udymts 123!"` |
| `encrypt_message("ABC", 29)` | `"DEF"` |
| `encrypt_message("ABC", -2)` | `"YZA"` |
| `encrypt_message("Hello!", 26)` | `"Hello!"` |
| `encrypt_message("", 10)` | `""` |
| `encrypt_message("123!?", 10)` | `"123!?"` |

### Message Decryption

| Function Call | Expected Result |
|---|---|
| `decrypt_message("KHOOR", 3)` | `"HELLO"` |
| `decrypt_message("Khoor Zruog!", 3)` | `"Hello World!"` |
| `decrypt_message("ABC", 3)` | `"XYZ"` |
| `decrypt_message("zab", -1)` | `"abc"` |
| `decrypt_message("Udymts 123!", 5)` | `"Python 123!"` |

## Command-Line Interface

The application must accept exactly three arguments, excluding the script filename:

```text
python cipher.py OPERATION MESSAGE SHIFT
```

The available operations are:

- `encrypt`
- `decrypt`

Operations must be case-insensitive.

The shift must be an integer.

### Test 1 — Encryption

```bash
python cipher.py encrypt "Hello World!" 3
```

Expected output:

```text
Khoor Zruog!
```

### Test 2 — Decryption

```bash
python cipher.py decrypt "Khoor Zruog!" 3
```

Expected output:

```text
Hello World!
```

### Test 3 — Negative Shift

```bash
python cipher.py encrypt "abc" -1
```

Expected output:

```text
zab
```

### Test 4 — Large Shift

```bash
python cipher.py encrypt "XYZ" 29
```

Expected output:

```text
ABC
```

### Test 5 — Case-Insensitive Operation

```bash
python cipher.py ENCRYPT "Hello" 3
```

Expected output:

```text
Khoor
```

### Test 6 — Invalid Operation

```bash
python cipher.py unknown "Hello" 3
```

Expected: A clear operation error.

### Test 7 — Invalid Shift

```bash
python cipher.py encrypt "Hello" abc
```

Expected: A numeric conversion error without a traceback.

### Test 8 — Missing Argument

```bash
python cipher.py encrypt "Hello"
```

Expected: Argument-count error.

### Test 9 — Extra Argument

```bash
python cipher.py encrypt "Hello" 3 EXTRA
```

Expected: Argument-count error.

## Final Challenge — Round-Trip Verification

A correctly designed encryption system should recover the original message after encryption followed by decryption using the same shift.

Run:

```python
message = "Python 123!"
shift = 57

encrypted = encrypt_message(message, shift)
decrypted = decrypt_message(encrypted, shift)

print(encrypted)
print(decrypted)
print(decrypted == message)
```

Expected output:

```text
Udymts 123!
Python 123!
True
```

Test this behavior with other messages and shifts.

## Edge Cases to Consider

Your implementation must handle:

- Empty messages
- Single-character messages
- Spaces and punctuation
- Uppercase and lowercase letters
- Negative shifts
- Shifts greater than 26
- A shift of zero
- Shifts that are multiples of 26
- Strings containing no alphabetic characters

## Completion Criteria

The project is complete when:

- All four required functions are implemented.
- The public tests pass.
- Capitalization and punctuation are preserved.
- Encryption and decryption work with all valid integer shifts.
- Decryption restores the original message.
- Invalid command-line input is handled gracefully.
- All restrictions are respected.

## Bonus Challenge — Cipher Cracking

Build a mode that tries all 26 possible Caesar cipher shifts for an encrypted message.

The
