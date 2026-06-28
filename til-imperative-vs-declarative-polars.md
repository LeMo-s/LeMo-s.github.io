---
title: "TIL: Imperative vs. Declarative Programming with Polars"
---

# TIL: Imperative vs. Declarative Programming with Polars

This week i learned that switching from writing raw, pure Python scripts (using no external libraries) to **Polars** isn't just about a massive speed upgrade—it’s a fundamental shift in programming paradigms. It forced me to move from an **imperative** mindset to a **declarative** one.

To understand why this matters, I looked at a data pipeline I built to clean a fruit inventory dataset. The dataset has two columns (`products` and `amount`), a few duplicates, some messy whitespace, and exactly *one* row where a fruit has a `null` amount. Crucially, because it's a raw CSV, the `amount` column was read as text (**strings**) instead of numbers, meaning I couldn't perform math like calculating the total inventory amount without converting it first.

Here is how shifting from "how to do it manually" to "what I want the end result to be" completely changes how data gets processed.

---

## The Imperative Approach: "How" to do it (The Pure Python Mindset)

When you write pure Python without data libraries, you default to an imperative paradigm. You give the computer a literal, step-by-step checklist of instructions. You micro-manage the execution using `open()`, loops, and conditional statements. The computer executes each line immediately, completely blind to what step is coming next.

If I wrote this pipeline in pure Python, the checklist would look like this:
1. Open the file and read lines.
2. Loop through each line and `.split(',')`.
3. **Step 1:** Strip the whitespaces from the product string using `.strip()`.
4. **Step 2:** Convert the product string to lowercase using `.lower()`.
5. **Step 3:** Check if the amount string is empty or missing. If it is, skip/drop the line.
6. **Step 4:** Explicitly convert the remaining `amount` strings to integers using `int(amount)` so we can sum them up later.
7. **Step 5:** Add the processed row to a set or list to keep track of and remove duplicates.

### The Inefficiency
The computer blindly follows your loop orders. It wastes CPU cycles trimming whitespace and changing the case for that broken `null` row in Steps 1 and 2, only to completely throw that row away in Step 3 when it finally checks the amount. Furthermore, you have to write explicit exception handling (like `try/except`) to make sure your `int()` conversion doesn't crash the script on messy data.

---

## The Declarative Approach: "What" I want (The Polars Mindset)

In a declarative paradigm, you don't give a step-by-step loop checklist; you describe the **final result** you want. You tell Polars *what* the end goal is, and let its internal query engine figure out the most efficient way to build it.

By using Polars, the script handles the string cleaning and the integer type-casting simultaneously