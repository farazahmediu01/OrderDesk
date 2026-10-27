# Lecture 3: Lists

**Goal:** students store several items in one list, read any item by position, add items, and check whether an item is in the list, all inside functions.

**How this doc works:** same as Lecture 2. Live-code each **example**, students predict, you run it, then students do **Your turn** right away. Each check prints `… passed`. Answers sit under each task; don't scroll to them while projecting.

**Prep (15 minutes):** type each example yourself once so you can live-code it.

**If running late:** give Your turn 2.1 and Example 3.1 as homework. Never skip 1.1, 2.2 or the exit ticket.

## Warm-up · 3 min

From Lecture 2. Students write their answer before you run it.

```python
def can_order(stock, qty):
    return qty <= stock and qty > 0

print(can_order(5, 0))
```

Output: `False`. 0 is within stock, but `qty > 0` is false, and `and` needs both.

## Part 1: Making and reading lists

### 1.1 Example: a shopping cart · 4 min

One variable can hold many items. Ask: "What will each line print?"

```python
cart = ["rice 5kg", "tea 1kg", "sugar 1kg"]

print(cart[0])
print(cart[2])
print(len(cart))
print(cart[-1])
```

Output: `rice 5kg`, `sugar 1kg`, `3`, `sugar 1kg`.

**Point at:** counting starts at **0**, so the third item is `cart[2]`. `len` gives the number of items. `-1` always means the last item, however long the list is.

### Your turn 1.1: first and last item · 4 min

Write `first_item(cart)` and `last_item(cart)`.

| Call | Result |
| --- | --- |
| `first_item(["milk", "eggs", "bread"])` | `"milk"` |
| `last_item(["milk", "eggs", "bread"])` | `"bread"` |
| `last_item(["soap"])` | `"soap"` |

```python
c = ["milk", "eggs", "bread"]
assert first_item(c) == "milk", "first item should be milk"
assert last_item(c) == "bread", "last item should be bread"
assert last_item(["soap"]) == "soap", "a one-item cart: last item is soap"
print("1.1 passed")
```

**Answer (don't project).** A student who writes `cart[2]` for the last item fails the one-item check; point them to `-1`.

```python
def first_item(cart):
    return cart[0]

def last_item(cart):
    return cart[-1]
```

### 1.2 Example: asking for an item that isn't there · 3 min

Ask: "The cart has three items. What happens with `cart[3]`?"

```python
cart = ["rice 5kg", "tea 1kg", "sugar 1kg"]
print(cart[3])
```

Output: `IndexError: list index out of range`.

**Point at:** the valid positions are 0 to `len(cart) - 1`. Read the last line of an error first.

### Your turn 1.2: the shopkeeper's "2nd item" · 5 min

Shopkeepers count from 1, not 0. Write `item_at(cart, position)` that returns the item at that position, counting from 1. If the position doesn't exist, return `"no such item"`.

| Call (with `c = ["milk", "eggs", "bread"]`) | Result |
| --- | --- |
| `item_at(c, 1)` | `"milk"` |
| `item_at(c, 3)` | `"bread"` |
| `item_at(c, 4)` | `"no such item"` |
| `item_at(c, 0)` | `"no such item"` |

```python
assert item_at(c, 1) == "milk", "position 1 is milk"
assert item_at(c, 3) == "bread", "position 3 is bread"
assert item_at(c, 4) == "no such item", "there is no 4th item"
assert item_at(c, 0) == "no such item", "there is no 0th item"
print("1.2 passed")
```

**Answer (don't project).** Uses `or` from Lecture 2. Students who forget the check get an `IndexError` on position 4.

```python
def item_at(cart, position):
    if position < 1 or position > len(cart):
        return "no such item"
    return cart[position - 1]
```

## Part 2: Changing lists and checking what's inside

### 2.1 Example: adding to the cart · 3 min

Ask: "What will the cart look like after `append`?"

```python
cart = ["rice 5kg"]
cart.append("tea 1kg")

print(cart)
print(len(cart))
```

Output: `['rice 5kg', 'tea 1kg']`, then `2`.

**Point at:** `append` adds to the end and changes the list itself. Nothing needs to be stored again.

### Your turn 2.1: add and count · 4 min

Write `add_item(cart, item)` that adds the item to the cart and returns how many items the cart now has.

| Steps | Result |
| --- | --- |
| `c2 = []`, then `add_item(c2, "milk")` | `1` |
| then `add_item(c2, "eggs")` | `2` |
| then `c2` | `["milk", "eggs"]` |

```python
c2 = []
assert add_item(c2, "milk") == 1, "after one item the count is 1"
assert add_item(c2, "eggs") == 2, "after two items the count is 2"
assert c2 == ["milk", "eggs"], "the cart itself should now hold both items"
print("2.1 passed")
```

**Answer (don't project).** The last check shows that the function changed the caller's list, the same idea as `add_order` in OrderDesk later.

```python
def add_item(cart, item):
    cart.append(item)
    return len(cart)
```

### 2.2 Example: is it in the cart? · 3 min

Ask students to predict all four lines. The third one is the trap.

```python
cart = ["rice 5kg", "tea 1kg"]

print("tea 1kg" in cart)
print("milk" in cart)
print("Tea 1kg" in cart)
print("milk" not in cart)
```

Output: `True`, `False`, `False`, `True`.

**Point at:** `in` needs an exact match, capital letters included. `not in` is the opposite. Both give `True` or `False`, so they work in an `if`.

### Your turn 2.2: no duplicates · 5 min

Write `add_if_new(cart, item)`. If the item is already in the cart, don't add it and return `False`. Otherwise add it and return `True`.

| Steps | Result |
| --- | --- |
| `c3 = ["milk"]`, then `add_if_new(c3, "eggs")` | `True` |
| then `add_if_new(c3, "milk")` | `False` |
| then `c3` | `["milk", "eggs"]` |

```python
c3 = ["milk"]
assert add_if_new(c3, "eggs") == True, "eggs is new, so it is added"
assert add_if_new(c3, "milk") == False, "milk is already there"
assert c3 == ["milk", "eggs"], "milk should appear only once"
print("2.2 passed")
```

**Answer (don't project).** Combines `if`, `in`, `append` and `return`. A student who appends before checking fails the last check.

```python
def add_if_new(cart, item):
    if item in cart:
        return False
    cart.append(item)
    return True
```

## Part 3: Putting it together

### 3.1 Example: a rule based on the cart's size · 3 min

Ask: "What do these two lines print?"

```python
def delivery_for_cart(cart):
    if len(cart) >= 5:
        return 0
    return 150

print(delivery_for_cart(["rice 5kg", "tea 1kg"]))
print(delivery_for_cart(["rice 5kg", "tea 1kg", "sugar 1kg", "oil 1L", "salt"]))
```

Output: `150`, then `0`.

**Point at:** `len` gives a number, so it works in any condition.

### Your turn 3.1: can we check out? · 5 min

One item is out of stock today. Write `can_checkout(cart, out_of_stock)` that returns `True` only if the cart is **not empty** and the out-of-stock item is **not in** the cart.

| Call | Result |
| --- | --- |
| `can_checkout([], "sugar 1kg")` | `False` |
| `can_checkout(["rice 5kg", "tea 1kg"], "sugar 1kg")` | `True` |
| `can_checkout(["rice 5kg", "sugar 1kg"], "sugar 1kg")` | `False` |

```python
assert can_checkout([], "sugar 1kg") == False, "an empty cart cannot check out"
assert can_checkout(["rice 5kg", "tea 1kg"], "sugar 1kg") == True, "no out-of-stock item: yes"
assert can_checkout(["rice 5kg", "sugar 1kg"], "sugar 1kg") == False, "sugar is out of stock: no"
print("3.1 passed")
```

**Answer (don't project).** An `if`/`else` version is also correct.

```python
def can_checkout(cart, out_of_stock):
    return len(cart) > 0 and out_of_stock not in cart
```

## Exit ticket · 5 min

Students answer on their own, without running it: what do the three lines print?

```python
items = ["soap", "oil", "salt"]
items.append("tea")

print(len(items))
print(items[1])
print("Oil" in items)
```

**Answer.** `4`, `oil`, `False`. Position 1 is the second item, and `"Oil"` with a capital O isn't an exact match.

## After class · 2 min

Note how many reached `3.1 passed` and how many got all three exit-ticket lines. The usual slip is `items[1]` answered as `soap`; if many make it, open Lecture 4 with one minute on counting from 0.
