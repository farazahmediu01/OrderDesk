# Lecture 4: Loops and Running Totals

**Goal:** students repeat a step for every item in a list with `for`, and use the running-total pattern to add up, count, and build new lists.

**How this doc works:** same as Lectures 2 and 3. Live-code the **example**, students predict, you run it, then **Your turn** right away. Answers sit under each task; don't scroll to them while projecting.

**Prep (15 minutes):** type each example yourself once. Practise saying the trace in 1.2 out loud; it's the heart of this lecture.

**If running late:** give 2.1 and 3.1 as homework. Never skip 1.2, 2.2 or the exit ticket.

## Warm-up · 3 min

From Lecture 3.

```python
prices = [1200, 1800, 160]
print(prices[-1] + prices[0])
```

Output: `1360`, which is 160 + 1200.

## Part 1: Doing something for each item

### 1.1 Example: packing the cart · 4 min

Ask: "How many lines will this print, and what's the last one?"

```python
cart = ["rice 5kg", "tea 1kg", "sugar 1kg"]

for item in cart:
    print("Packing", item)

print("Done")
```

Output:

```
Packing rice 5kg
Packing tea 1kg
Packing sugar 1kg
Done
```

**Point at:** the indented line runs once for each item, and `item` holds a different item each time. `Done` is not indented, so it runs once, after the loop.

### Your turn 1.1: price tags · 4 min

Write a loop that prints every price in `[1200, 1800, 160]` with `Rs.` in front. There's no check this time: compare your output with this.

```
Rs. 1200
Rs. 1800
Rs. 160
```

**Answer (don't project).**

```python
for price in [1200, 1800, 160]:
    print("Rs.", price)
```

### 1.2 Example: adding up the bill · 6 min

The most important pattern of the course. Ask: "What will `bill_total` return?"

```python
def bill_total(prices):
    total = 0
    for price in prices:
        total = total + price
    return total

print(bill_total([1200, 1800, 160]))
print(bill_total([]))
```

Output: `3160`, then `0`.

Trace it on the board, one row per step:

| Step | `price` | `total` after the step |
| --- | --- | --- |
| Before the loop | — | `0` |
| 1st time round | `1200` | `1200` |
| 2nd time round | `1800` | `3000` |
| 3rd time round | `160` | `3160` |
| After the loop | — | returns `3160` |

**Point at:** three places, always. **Start** the total before the loop, **update** it inside the loop, **return** it after the loop. An empty list skips the loop, so the total stays 0.

### Your turn 1.2: count the items · 5 min

Write `count_items(cart)` that counts the items with a loop. Don't use `len`: the point is to practise the pattern.

| Call | Result |
| --- | --- |
| `count_items(["milk", "eggs", "bread"])` | `3` |
| `count_items([])` | `0` |

```python
assert count_items(["milk", "eggs", "bread"]) == 3, "three items"
assert count_items([]) == 0, "an empty cart has 0 items"
print("1.2 passed")
```

**Answer (don't project).** Same three places as `bill_total`; only the update changes.

```python
def count_items(cart):
    count = 0
    for item in cart:
        count = count + 1
    return count
```

## Part 2: Building lists, and the most common loop bug

### 2.1 Example: making a new list in a loop · 4 min

Every item needs 50 rupees of packing added. Ask: "What list comes back?"

```python
def add_packing(prices):
    result = []
    for price in prices:
        result.append(price + 50)
    return result

print(add_packing([100, 200]))
```

Output: `[150, 250]`.

**Point at:** the same three places as `bill_total`. Start with an empty list, `append` inside the loop, return after the loop.

### Your turn 2.1: discount every price · 5 min

Paste `apply_discount` from Lecture 1 into your file first. Then write `discounted_prices(prices, percent)` that returns a new list with the discount applied to every price.

```python
def apply_discount(total, percent=0):
    return total - total * percent / 100
```

| Call | Result |
| --- | --- |
| `discounted_prices([1000, 2000], 10)` | `[900, 1800]` |
| `discounted_prices([], 10)` | `[]` |

If you print the result you'll see `[900.0, 1800.0]`. That's fine: the check treats them as equal.

```python
assert discounted_prices([1000, 2000], 10) == [900, 1800], "10% off each price"
assert discounted_prices([], 10) == [], "no prices gives an empty list"
print("2.1 passed")
```

**Answer (don't project).**

```python
def discounted_prices(prices, percent):
    result = []
    for price in prices:
        result.append(apply_discount(price, percent))
    return result
```

### 2.2 Example: one indent too far · 3 min

This looks almost like `bill_total`. Ask: "What does it print, and why?"

```python
def bill_total_wrong(prices):
    total = 0
    for price in prices:
        total = total + price
        return total

print(bill_total_wrong([1200, 1800, 160]))
```

Output: `1200`.

**Point at:** `return` is inside the loop, so the function leaves after the first item. Indentation decides what runs once and what runs every time.

### Your turn 2.2: fix the bug · 4 min

This function should add up quantities, but `total_quantity([2, 1, 3])` gives `3` instead of `6`. Copy it, find the bug and fix it.

```python
def total_quantity(qtys):
    for q in qtys:
        total = 0
        total = total + q
    return total
```

```python
assert total_quantity([2, 1, 3]) == 6, "2 + 1 + 3 is 6"
assert total_quantity([]) == 0, "no quantities gives 0"
print("2.2 passed")
```

**Answer (don't project).** `total = 0` was inside the loop, so it reset every time. With the bug, `total_quantity([])` also crashes with `UnboundLocalError: cannot access local variable 'total' where it is not associated with a value`, because the loop never ran to create `total`.

```python
def total_quantity(qtys):
    total = 0
    for q in qtys:
        total = total + q
    return total
```

## Part 3: Putting it together

### 3.1 Example: average price · 3 min

Ask: "What happens with an empty list if we remove the `if`?"

```python
def average_price(prices):
    if len(prices) == 0:
        return 0
    return bill_total(prices) / len(prices)

print(average_price([1200, 1800]))
print(average_price([]))
```

Output: `1500.0`, then `0`.

**Point at:** one function using another (`bill_total`), plus an `if` that protects against dividing by zero.

### Your turn 3.1: check out a whole cart · 5 min

Paste these functions from Lecture 2 into your file. Then write `cart_checkout(prices, km)`: add up the cart's prices and return the checkout total for that distance.

```python
def discount_rate(total):
    if total >= 10000:
        return 15
    elif total >= 5000:
        return 10
    else:
        return 0

def zone_charge(km):
    if km <= 3:
        return 0
    elif km <= 10:
        return 150
    else:
        return 300

def final_price(total):
    return apply_discount(total, discount_rate(total))

def checkout_total(total, km):
    return final_price(total) + zone_charge(km)
```

| Call | Result | Why |
| --- | --- | --- |
| `cart_checkout([6000, 1000], 7)` | `6450` | 7000 total, 10% off, plus 150 |
| `cart_checkout([500, 300], 15)` | `1100` | 800 total, no discount, plus 300 |
| `cart_checkout([], 2)` | `0` | Nothing to pay, free delivery |

```python
assert cart_checkout([6000, 1000], 7) == 6450, "7000 at 7 km should be 6450"
assert cart_checkout([500, 300], 15) == 1100, "800 at 15 km should be 1100"
assert cart_checkout([], 2) == 0, "an empty cart at 2 km costs 0"
print("3.1 passed")
```

**Answer (don't project).** One line: Lecture 4's loop feeds Lecture 2's functions.

```python
def cart_checkout(prices, km):
    return checkout_total(bill_total(prices), km)
```

## Exit ticket · 5 min

Students answer on their own, without running it: what does this print, and what does `mystery` actually do?

```python
def mystery(nums):
    result = 0
    for n in nums:
        result = result + 1
    return result

print(mystery([5, 10, 15]))
```

**Answer.** `3`. It counts the items; it never uses the numbers themselves. Students who answer `30` read it as a sum, so they're matching the shape of the code without reading the update line.

## After class · 2 min

Note how many reached `3.1 passed` and how many answered `3` with the right reason. If many said `30`, open Lecture 5 by tracing `mystery` with the trace table from 1.2.
