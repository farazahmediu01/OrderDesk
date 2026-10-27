# Lecture 2: Decisions Inside Functions

**Goal:** students write functions that choose between several answers, and functions that answer yes or no using `and` and `or`.

**How this doc works:** six small pairs. For each one, you live-code the **example**, students predict, you run it, and then students immediately do **Your turn** on their own computers. Every "Your turn" ends with a check that prints `… passed`. The **answer** sits under each task for you; don't scroll to it while projecting.

**Prep (15 minutes):** read through once, and type each example yourself so you can live-code it without notes.

**If running late:** skip 2.1 and 2.3, and give them as homework. Never skip 1.2, 2.2 or the exit ticket.

## Warm-up · 3 min

Students write their answer before you run it.

```python
def label(price):
    if price > 1000:
        return "expensive"
    return "affordable"

print(label(1000))
```

Output: `affordable`. Ask who wrote `expensive`: 1000 is not *greater than* 1000. Today is all about rules like this.

## Part 1: Choosing between several answers

### 1.1 Example: discount tiers · 5 min

The shop gives 15% off from 10,000, 10% from 5,000, otherwise nothing. Live-code it, then ask: "What will each line print?"

```python
def discount_rate(total):
    if total >= 10000:
        return 15
    elif total >= 5000:
        return 10
    else:
        return 0

print(discount_rate(12000))
print(discount_rate(7000))
print(discount_rate(5000))
print(discount_rate(800))
```

Output: `15`, `10`, `10`, `0`.

**Point at:** Python checks the rules from top to bottom and stops at the **first** one that is true. `return` leaves the function immediately. And `5000` gets 10 because `>=` includes the boundary.

### Your turn 1.1: delivery by distance · 5 min

Delivery is free up to 3 km, costs 150 up to 10 km, and costs 300 beyond that. Write `zone_charge(km)`.

| Call | Result |
| --- | --- |
| `zone_charge(2)` | `0` |
| `zone_charge(3)` | `0` |
| `zone_charge(7)` | `150` |
| `zone_charge(10)` | `150` |
| `zone_charge(15)` | `300` |

```python
assert zone_charge(2) == 0, "zone_charge(2) should be 0"
assert zone_charge(3) == 0, "exactly 3 km should be 0"
assert zone_charge(7) == 150, "zone_charge(7) should be 150"
assert zone_charge(10) == 150, "exactly 10 km should be 150"
assert zone_charge(15) == 300, "zone_charge(15) should be 300"
print("1.1 passed")
```

**Answer (don't project).** Common mistake: `<` instead of `<=` fails the "exactly 3 km" check.

```python
def zone_charge(km):
    if km <= 3:
        return 0
    elif km <= 10:
        return 150
    else:
        return 300
```

### 1.2 Example: order matters · 3 min

Swap the first two rules and ask: "Now what does 12,000 get?"

```python
def discount_rate_wrong(total):
    if total >= 5000:
        return 10
    elif total >= 10000:
        return 15
    else:
        return 0

print(discount_rate_wrong(12000))
```

Output: `10`.

**Point at:** every bill of 5000 or more stops at the first rule, so the 15% rule can never run. Put the biggest or strictest rule first.

### Your turn 1.2: fix the bug · 4 min

This function is meant to give `gold` from 5000, `silver` from 1000, and `new` otherwise, but `customer_level(8000)` gives `silver`. Copy it, find the bug and fix it.

```python
def customer_level(spent):
    if spent >= 1000:
        return "silver"
    elif spent >= 5000:
        return "gold"
    else:
        return "new"
```

```python
assert customer_level(8000) == "gold", "8000 should be gold"
assert customer_level(3000) == "silver", "3000 should be silver"
assert customer_level(500) == "new", "500 should be new"
print("1.2 passed")
```

**Answer (don't project).** Swap the two rules so the 5000 check comes first.

```python
def customer_level(spent):
    if spent >= 5000:
        return "gold"
    elif spent >= 1000:
        return "silver"
    else:
        return "new"
```

## Part 2: Yes-or-no answers

### 2.1 Example: a function that answers yes or no · 3 min

A condition is already `True` or `False`, so a function can return it directly. Ask: "What will these print?"

```python
def is_big_order(total):
    return total >= 5000

print(is_big_order(7000))
print(is_big_order(1200))
```

Output: `True`, `False`.

**Point at:** `True` and `False` have no quotes. They are not text.

### Your turn 2.1: is it far? · 3 min

Write `is_far(km)` that returns `True` when the distance is **more than** 10 km.

| Call | Result |
| --- | --- |
| `is_far(15)` | `True` |
| `is_far(10)` | `False` |
| `is_far(4)` | `False` |

```python
assert is_far(15) == True, "15 km is far"
assert is_far(10) == False, "exactly 10 km is not far"
assert is_far(4) == False, "4 km is not far"
print("2.1 passed")
```

**Answer (don't project).**

```python
def is_far(km):
    return km > 10
```

### 2.2 Example: `and`, both must be true · 5 min

The shop delivers today only if the customer is within 10 km **and** the order comes before 5 pm. Ask students to predict all four lines.

```python
def can_deliver_today(km, before_5pm):
    return km <= 10 and before_5pm

print(can_deliver_today(5, True))
print(can_deliver_today(5, False))
print(can_deliver_today(15, True))
print(can_deliver_today(15, False))
```

Output: `True`, `False`, `False`, `False`.

**Point at:** with `and`, one "no" makes the whole answer "no". Only the first line has both rules true.

### Your turn 2.2: can this customer buy on credit? · 5 min

The shopkeeper gives credit (udhaar) only to **regular** customers who owe **less than 5000**. Write `can_give_credit(owed, is_regular)`.

| Call | Result |
| --- | --- |
| `can_give_credit(2000, True)` | `True` |
| `can_give_credit(2000, False)` | `False` |
| `can_give_credit(5000, True)` | `False` |
| `can_give_credit(6000, False)` | `False` |

```python
assert can_give_credit(2000, True) == True, "regular, owes 2000: yes"
assert can_give_credit(2000, False) == False, "not regular: no"
assert can_give_credit(5000, True) == False, "owes exactly 5000: no"
assert can_give_credit(6000, False) == False, "neither rule met: no"
print("2.2 passed")
```

**Answer (don't project).** An `if`/`else` that returns `True` and `False` is also correct. Common mistake: `or` instead of `and` fails the "not regular" check.

```python
def can_give_credit(owed, is_regular):
    return is_regular and owed < 5000
```

### 2.3 Example: `or`, one is enough · 3 min

A big order **or** a new customer needs the owner's approval. Ask for predictions.

```python
def needs_approval(total, is_new):
    return total > 20000 or is_new

print(needs_approval(25000, False))
print(needs_approval(3000, True))
print(needs_approval(3000, False))
```

Output: `True`, `True`, `False`.

**Point at:** with `or`, one "yes" is enough. The answer is "no" only when both are "no".

### Your turn 2.3: free delivery · 3 min

Delivery is free if the bill is 3000 or more, **or** the customer is regular. Write `free_delivery(total, is_regular)`.

| Call | Result |
| --- | --- |
| `free_delivery(3500, False)` | `True` |
| `free_delivery(1000, True)` | `True` |
| `free_delivery(1000, False)` | `False` |

```python
assert free_delivery(3500, False) == True, "3500 is enough"
assert free_delivery(1000, True) == True, "regulars always free"
assert free_delivery(1000, False) == False, "1000 and not regular pays"
print("2.3 passed")
```

**Answer (don't project).**

```python
def free_delivery(total, is_regular):
    return total >= 3000 or is_regular
```

## Part 3: Putting it together

### 3.1 Example: one function using another · 3 min

Bring back `apply_discount` from Lecture 1 and combine it with `discount_rate`. Ask: "What will 7000 cost?"

```python
def apply_discount(total, percent=0):
    return total - total * percent / 100

def final_price(total):
    return apply_discount(total, discount_rate(total))

print(final_price(12000))
print(final_price(7000))
print(final_price(800))
```

Output: `10200.0`, `6300.0`, `800.0`. The `.0` appears because the discount uses division.

**Point at:** read the inner call first. `discount_rate(7000)` gives 10, so this becomes `apply_discount(7000, 10)`.

### Your turn 3.1: checkout · 5 min

Write `checkout_total(total, km)`: the final price plus the delivery charge for the distance. Use `final_price` and your `zone_charge` from 1.1.

| Call | Result |
| --- | --- |
| `checkout_total(12000, 2)` | `10200` |
| `checkout_total(7000, 7)` | `6450` |
| `checkout_total(800, 15)` | `1100` |

```python
assert checkout_total(12000, 2) == 10200, "12000 at 2 km should be 10200"
assert checkout_total(7000, 7) == 6450, "7000 at 7 km should be 6450"
assert checkout_total(800, 15) == 1100, "800 at 15 km should be 1100"
print("3.1 passed")
```

**Answer (don't project).** If a student gets `6300` for the second call, delivery wasn't added.

```python
def checkout_total(total, km):
    return final_price(total) + zone_charge(km)
```

## Exit ticket · 5 min

Students answer on their own, without running it: what do the two lines print, and why?

```python
def can_return(days, has_receipt):
    return days <= 7 and has_receipt

print(can_return(3, False))
print(can_return(10, True) or can_return(2, True))
```

**Answer.** `False`, then `True`. The first is `False` because `and` needs both, and there's no receipt. In the second, `can_return(10, True)` is `False`, but `can_return(2, True)` is `True`, and `or` needs only one.

## After class · 2 min

Note how many students reached `3.1 passed`, and how many got both exit-ticket lines right. If fewer than half got the exit ticket, open Lecture 3 with a quick redo of 2.2.
