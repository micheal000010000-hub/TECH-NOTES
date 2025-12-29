# Mutable vs Immutable Objects in Python

**Date:** 29/12/2026  

This note captures an important foundational concept in Python that often causes confusion while working with data structures, recursion, and backtracking: **mutable vs immutable objects**.

---

## 1. Types of Python Objects

In Python, objects can broadly be classified into **two categories** based on whether their internal state can change:

- **Mutable objects**
- **Immutable objects**

Understanding this distinction is crucial for writing correct and efficient Python programs.

---

## 2. Mutable Objects

**Mutable objects** are objects whose contents **can be changed after they are created**.  
When an operation is performed on a mutable object, the **data at the same memory address is modified**.

### Common mutable objects:
- `list`
- `set`
- `dict`

### Example:
```python
a = [1, 2, 3]
a.append(4)
print(a)   # [1, 2, 3, 4]
