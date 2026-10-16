# Challenge 02 — Mini Store Checkout

**Difficulty:** Intermediate  
**Language:** Python 3  
**Suggested filename:** `checkout.py`

## Objective

Build a command-line checkout system that processes two products, calculates totals, applies discount coupons, handles payments, and generates a receipt.

This challenge focuses on function decomposition, reusable calculations, Boolean logic, input validation, and exception handling.

## Required Functions

Implement the following functions:

```python
def calculate_item_total(price, quantity):
    pass

def calculate_subtotal(price1, quantity1, price2, quantity2):
    pass

def apply_discount(subtotal, coupon):
    pass

def calculate_payment(total, paid):
    pass

def payment_status(total, paid):
    pass

def validate_item(price, quantity):
    pass

def print_receipt(name1, price1, qty1,
                  name2, price2, qty2,
                  coupon, paid):
    pass
```

The calculation and validation functions must return values. `print_receipt()` must display the receipt.

## Functional Requirements

### 1. Product Calculations

Each checkout contains exactly two products.

For each product:

- Calculate the item total from its price and quantity.
- Calculate the combined subtotal.
- Reuse existing calculation functions instead of repeating calculations.

### 2. Discount System

Implement the following coupon rules:

| Coupon | Minimum Subtotal | Discount |
|---|---:|---:|
| `SAVE5` | 30 | 5 |
| `SAVE10` | 60 | 10 |
| Any other code | None | 0 |

**Additional rules:**

- Coupon codes are case-insensitive.
- Only one coupon is applied per checkout.
- Discounts are fixed amounts, not percentages.
- Coupons below their required subtotal must not reduce the total.
- `apply_discount()` must return the final total after the discount.

### 3. Payment Processing

Calculate the difference between the customer's payment and the final total.

`payment_status()` must return one of the following:

| Condition | Return Value |
|---|---|
| Payment exceeds total | `"Change: N"` |
| Payment is below total | `"Amount owed: N"` |
| Payment equals total | `"Payment complete"` |

`N` is the positive amount of change or outstanding payment.

### 4. Input Validation

Your application must reject:

- Negative product prices
- Zero or negative quantities
- Negative payments
- Nonnumeric values in integer fields
- Missing or extra command-line arguments

A product price of `0` is allowed. A payment of `0` is also allowed.

When validation fails, display a clear error message and do not generate a receipt.

### 5. Receipt

The receipt must display:

- Both product names
- Quantities
- Individual item totals
- Subtotal
- Discount coupon and discount amount
- Final total
- Amount paid
- Payment status

The exact visual formatting is up to you.

## Rules and Restrictions

**Allowed:**

- Functions and return values
- Arithmetic operators
- `if`, `elif`, and `else`
- Boolean operators (`and`, `or`, `not`)
- String formatting
- `sys.argv`
- `int()`
- `try` and `except`
- `.upper()` and `.lower()`

**Not allowed:**

- External checkout or payment libraries
- Hardcoded totals or discounts
- Repeating calculations instead of reusing required functions
- Generating a receipt after invalid input
- Allowing invalid numeric input to produce an unhandled traceback

## Public Function Tests

### Item Totals and Subtotals

| Function Call | Expected Result |
|---|---:|
| `calculate_item_total(10, 3)` | `30` |
| `calculate_item_total(0, 5)` | `0` |
| `calculate_subtotal(10, 2, 15, 3)` | `65` |
| `calculate_subtotal(5, 1, 5, 1)` | `10` |

### Discount Tests

| Function Call | Expected Result |
|---|---:|
| `apply_discount(30, "SAVE5")` | `25` |
| `apply_discount(29, "SAVE5")` | `29` |
| `apply_discount(60, "SAVE10")` | `50` |
| `apply_discount(59, "SAVE10")` | `59` |
| `apply_discount(100, "save5")` | `95` |
| `apply_discount(100, "INVALID")` | `100` |
| `apply_discount(100, "NONE")` | `100` |

### Payment Tests

| Function Call | Expected Result |
|---|---|
| `calculate_payment(40, 50)` | `10` |
| `calculate_payment(40, 20)` | `-20` |
| `payment_status(40, 50)` | `"Change: 10"` |
| `payment_status(40, 20)` | `"Amount owed: 20"` |
| `payment_status(40, 40)` | `"Payment complete"` |

### Validation Tests

| Function Call | Expected Result |
|---|---|
| `validate_item(10, 2)` | `True` |
| `validate_item(0, 1)` | `True` |
| `validate_item(-10, 2)` | `False` |
| `validate_item(10, 0)` | `False` |
| `validate_item(10, -2)` | `False` |

## Command-Line Interface

Your application must accept exactly eight arguments, excluding the script name.

```text
python checkout.py NAME1 PRICE1 QTY1 NAME2 PRICE2 QTY2 COUPON PAID
```

### Test 1 — Successful Checkout

```bash
python checkout.py "Pen" 10 2 "Notebook" 15 3 SAVE10 70
```

The receipt must contain these results:

```text
Pen x 2              20
Notebook x 3         45

Subtotal             65
Discount (SAVE10)    10
Total                55
Paid                 70
Change               15
```

### Test 2 — Insufficient Payment

```bash
python checkout.py "Pen" 10 2 "Notebook" 15 1 NONE 10
```

Expected:

- Subtotal: `35`
- Discount: `0`
- Total: `35`
- Amount owed: `25`

### Test 3 — Invalid Price

```bash
python checkout.py "Pen" -10 2 "Notebook" 15 1 NONE 50
```

Expected: Product validation error. No receipt.

### Test 4 — Invalid Quantity

```bash
python checkout.py "Pen" 10 0 "Notebook" 15 1 NONE 50
```

Expected: Product validation error. No receipt.

### Test 5 — Negative Payment

```bash
python checkout.py "Pen" 10 2 "Notebook" 15 1 NONE -10
```

Expected: Payment validation error. No receipt.

### Test 6 — Nonnumeric Price

```bash
python checkout.py "Pen" abc 2 "Notebook" 15 1 NONE 50
```

Expected: Numeric conversion error without a traceback.

### Test 7 — Nonnumeric Quantity

```bash
python checkout.py "Pen" 10 abc "Notebook" 15 1 NONE 50
```

Expected: Numeric conversion error without a traceback.

### Test 8 — Nonnumeric Payment

```bash
python checkout.py "Pen" 10 2 "Notebook" 15 1 NONE abc
```

Expected: Numeric conversion error without a traceback.

### Test 9 — Missing Arguments

```bash
python checkout.py "Pen" 10 2
```

Expected: Argument-count error.

### Test 10 — Extra Arguments

```bash
python checkout.py "Pen" 10 2 "Notebook" 15 1 NONE 50 EXTRA
```

Expected: Argument-count error.

## Completion Criteria

The project is complete when:

- All seven required functions are implemented.
- All public calculation, discount, payment, and validation tests pass.
- The CLI generates an accurate receipt.
- Invalid inputs are rejected without an unhandled traceback.
- All restrictions are followed.

## Bonus Challenge — Unlimited Products

Extend the application to process any number of products rather than exactly two.

Consider how your data structures and function design would need to change.

---

**Focus on correct calculations, reusable functions, and robust input validation. Build the solution independently.**
