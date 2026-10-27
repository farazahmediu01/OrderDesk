# Lecture 1: Functions That Return

## How to use this handout

Today you will learn the difference between a function that **shows** a value (`print`) and a function that **gives a value back** (`return`). Then you will use returned values to build a shop bill.

- **You need:** your Python editor and a new file called `lecture01.py`.
- **Required:** Parts 1–4 and Questions 1–3.
- **Optional:** the Stretch question, only if you finish early.
- Every question shows the correct result and a **check** you paste in to test yourself.
- **Stuck?** Follow "When you're stuck" near the end before asking. Hints are at the very end, so you don't see them by accident.

## Words to know

| Word | Meaning | Example |
| --- | --- | --- |
| Function | A named set of steps you can run again and again | `def line_total(qty, price):` |
| Parameter | A name in the function that waits for a value | `qty`, `price` |
| Argument | The actual value you give when you use the function | `2`, `1200` |
| Call | Running a function | `line_total(2, 1200)` |
| Return value | The value a function gives back to the line that called it | `return qty * price` |
| `None` | Python's word for "nothing"; what a function gives back if it has no `return` | `a is None` |
| Default parameter | A parameter with a value it uses when you don't give one | `percent=0` |
| Keyword argument | An argument given by name | `apply_discount(4200, percent=5)` |
| Check (`assert`) | A line that stops with a message if a result is wrong | `assert change_due(5000, 4200) == 800` |

## Part 1: Warm-up (5 minutes)

Write your answer on paper **before** you run anything. Then run it and compare.

**Warm-up 1.** What does this print?

```python
def greet(name, greeting="Hello"):
    print(greeting, name)

greet("Sana")
greet("Bilal", greeting="Salam")
```

**Warm-up 2.** What does this print?

```python
price = 1200
if price > 1000:
    print("expensive")
else:
    print("affordable")
```

## Part 2: Worked example (10 minutes)

Two functions do the same calculation. One prints the answer; the other returns it.

```python
def line_total_print(qty, price):
    print(qty * price)

def line_total(qty, price):
    return qty * price

a = line_total_print(2, 1200)
b = line_total(2, 1200)
print("a is", a)
print("b is", b)
```

1. **Predict:** write down all three lines you expect to see.
2. **Run** it and compare with your prediction.
3. **Explain:** why is `a` equal to `None`?

**The difference, in one table**

|  | `print(...)` | `return ...` |
| --- | --- | --- |
| Where the value goes | To the screen | Back to the line that called the function |
| Can you store it in a variable? | No; you get `None` | Yes |
| Can you use it in more maths? | No | Yes |
| Think of it as | Saying the price out loud | Writing the price on the bill |

**What if?** Add these two lines to the end of your file. One of them causes an error. Predict which one, and why, before you run them.

```python
print(line_total(2, 1200) + line_total(1, 1800))
print(line_total_print(2, 1200) + line_total_print(1, 1800))
```

When you've seen the result, delete those two lines so the rest of your file can run.

## Part 3: Practice (30 minutes)

Write each function in `lecture01.py`, below the worked example. After each question, paste its check at the **bottom** of your file and run it.

- If the check passes, nothing is printed for that question. Move on.
- If it fails, you'll see `AssertionError` and a message saying which result is wrong. Fix your function and run again.

### Question 1 (required): change due

A customer pays with a note. Write `change_due(paid, total)` that **returns** how much change to give back.

| Call | Correct result |
| --- | --- |
| `change_due(5000, 4200)` | `800` |
| `change_due(1000, 1000)` | `0` |

```python
# Question 1 check
assert change_due(5000, 4200) == 800, "Q1: change_due(5000, 4200) should be 800"
assert change_due(1000, 1000) == 0, "Q1: change_due(1000, 1000) should be 0"
```

### Question 2 (required): discount with a default

Write `apply_discount(total, percent=0)` that **returns** the total after taking off `percent` percent. If no percent is given, there is no discount.

| Call | Correct result |
| --- | --- |
| `apply_discount(4200)` | `4200` |
| `apply_discount(4200, 10)` | `3780` |
| `apply_discount(4200, percent=5)` | `3990` |

If you print the result, Python may show `3780.0` instead of `3780`. That is fine: dividing gives a decimal number, and the check treats them as equal.

```python
# Question 2 check
assert apply_discount(4200) == 4200, "Q2: no discount given should return 4200"
assert apply_discount(4200, 10) == 3780, "Q2: 10% off 4200 should be 3780"
assert apply_discount(4200, percent=5) == 3990, "Q2: 5% off 4200 should be 3990"
```

### Question 3 (required): the final bill

The shop delivers. Delivery is **free when the bill is 3000 or more**, and costs **200** otherwise. Write two functions:

1. `delivery_charge(total)` **returns** `0` or `200`.
2. `final_bill(total, percent=0)` **returns** the bill after the discount, plus delivery. Use your `apply_discount` and `delivery_charge` functions inside it. Decide delivery on the bill **after** the discount.

| Call | Correct result | Why |
| --- | --- | --- |
| `delivery_charge(3000)` | `0` | Exactly 3000 counts as "3000 or more" |
| `delivery_charge(2999)` | `200` | Under 3000 |
| `final_bill(4200)` | `4200` | No discount; free delivery |
| `final_bill(2500)` | `2700` | No discount; 200 delivery |
| `final_bill(3200, 10)` | `3080` | 10% off gives 2880, which is under 3000, so add 200 |

```python
# Question 3 check
assert delivery_charge(3000) == 0, "Q3: 3000 or more is free delivery"
assert delivery_charge(2999) == 200, "Q3: under 3000 costs 200"
assert final_bill(4200) == 4200, "Q3: final_bill(4200) should be 4200"
assert final_bill(2500) == 2700, "Q3: final_bill(2500) should be 2700"
assert final_bill(3200, 10) == 3080, "Q3: final_bill(3200, 10) should be 3080"
print("All core checks passed")
```

**When all three checks are in your file and you see `All core checks passed`, the required work is done.**

### Stretch (optional): a receipt line

Only if you finished Questions 1–3. Write `receipt_line(item, qty, price)` that **returns** a line of text for the receipt. Use your `line_total` function from Part 2.

| Call | Correct result |
| --- | --- |
| `receipt_line("rice 5kg", 2, 1200)` | `"rice 5kg x2 = 2400"` |

```python
# Stretch check (paste only if you wrote receipt_line)
assert receipt_line("rice 5kg", 2, 1200) == "rice 5kg x2 = 2400", "Stretch: check spaces and the x"
print("Stretch check passed")
```

## Part 4: Show what you understood (5 minutes)

**Exit ticket.** Without running it: what does this print, and why? Answer in one or two sentences, the way your teacher asks (on paper or digitally).

```python
def double(n):
    print(n * 2)

x = double(5)
print(x)
```

**Explain your code, your way.** Your teacher may ask you to explain `final_bill`. You can choose either:

- Explain it out loud, line by line, or
- Write a comment above each line of `final_bill` saying what that line does, and show your teacher.

## When you're stuck

Go through these steps in order. Spend about a minute on each.

1. **Re-read the results table** for the question. Which call gives the wrong result?
2. **Run your file and read the last line of the error first.** It usually names the problem.
3. **Print the value you're unsure about**, for example `print(apply_discount(4200, 10))`.
4. **Open Hint 1** for that question below. Still stuck? Open Hint 2.
5. **Explain your code to the person next to you.** Saying it out loud often shows the mistake.
6. **Still stuck after that?** Raise your hand.

**What common errors mean**

| You see | It usually means |
| --- | --- |
| `AssertionError: Q1: …` | Your function runs but gives a wrong result. The message says which call. |
| A number printed, then `AssertionError` | You used `print` where you need `return`. |
| `TypeError: … 'NoneType' …` | You used a function that has no `return` inside a calculation. |
| `NameError: name '…' is not defined` | A name is misspelt, or that function isn't written yet. |
| `IndentationError` | The spaces at the start of a line are wrong. |

## Hints

Open only the hint you need, and try again after each one.

**Question 1**

- Hint 1: the change is the money left after the total is paid.
- Hint 2: it takes one line: `return` the paid amount minus the total.

**Question 2**

- Hint 1: work out the discount amount first: total × percent ÷ 100.
- Hint 2: `return` the total minus that amount. Writing `percent=0` in the `def` line is what makes the percent optional.

**Question 3: `delivery_charge`**

- Hint 1: use `if` and `else`, with a different `return` in each.
- Hint 2: the condition is `total >= 3000`, not `>`, because exactly 3000 is free.

**Question 3: `final_bill`**

- Hint 1: first store the discounted bill in a variable, for example `discounted = apply_discount(total, percent)`.
- Hint 2: then `return` the discounted bill plus the delivery charge on that discounted bill.

**Stretch**

- Hint 1: text and numbers can't be joined with `+` directly. Turn a number into text with `str()`.
- Hint 2: build the line piece by piece: the item, then `" x"`, then the quantity as text, then `" = "`, then the line total as text.
