# MISRA-C Guidelines — Detailed Cheat Sheet

> **Target audience:** Embedded Systems & Safety-Critical Software Developers  
> **Language:** C (especially Bare‑Metal / MCU development)

---

## What is MISRA-C?

**MISRA-C (Motor Industry Software Reliability Association)** is a set of coding guidelines designed to:

- Improve **code safety and reliability**
    
- Avoid **undefined / unspecified / implementation-defined behavior** in C
    
- Make code **predictable and analyzable**
    
- Reduce runtime failures in **embedded and safety‑critical systems**
    

MISRA-C is widely used in:

- Automotive (AUTOSAR)
    
- Medical devices
    
- Aerospace
    
- Industrial controllers
    

---

## Common MISRA-C Versions

- MISRA-C:2004
    
- MISRA-C:2012 (most used in industry)
    
- MISRA-C:2023 (latest)
    

---

# Core MISRA-C Rules (Most Famous & Practical)

---

## 1. No Implicit Type Conversion

❌ Not recommended:

```c
uint8_t x = 300;
```

✅ Recommended:

```c
uint8_t x = (uint8_t)300;
```

**Reason:**

- Prevent overflow
    
- Prevent hidden truncation
    
- Make programmer intent explicit
    

---

## 2. Use Fixed-Width Integer Types

❌ Not recommended:

```c
int x;
```

✅ Recommended:

```c
uint32_t x;
```

**Reason:**

- `int` size is compiler-dependent
    
- Embedded systems require deterministic size
    

---

## 3. All Variables Must Be Initialized

❌

```c
int x;
if (x > 5)
```

✅

```c
int x = 0;
```

**Reason:**

- Uninitialized variables cause undefined behavior
    

---

## 4. No Use of Undefined Behavior

Examples of forbidden behavior:

```c
int x = 10 / 0;        // division by zero
*p = 5;                // NULL pointer dereference
x = i++ + i++;         // multiple side effects
```

**Reason:**

- Compiler may generate unpredictable code
    

---

## 5. Avoid Multiple Side Effects in One Expression

❌

```c
a = i++ + i++;
```

✅

```c
i++;
a = i;
i++;
a += i;
```

---

## 6. Always Use Braces `{}`

❌

```c
if (x > 0)
    y++;
    z++;
```

✅

```c
if (x > 0)
{
    y++;
}
```

**Reason:**

- Prevent logical errors
    

---

## 7. Every `if` Must Have an `else` (Recommended)

```c
if (status == OK)
{
    handle_ok();
}
else
{
    /* explicitly handled */
}
```

---

## 8. Every `switch` Must Have a `default`

```c
switch (state)
{
    case INIT:
        break;

    case RUN:
        break;

    default:
        break;
}
```

**Reason:**

- Protect against unexpected values
    

---

## 9. No `goto` Statement

❌

```c
goto error;
```

**Reason:**

- Breaks control flow readability
    

(Allowed only with strong justification)

---

## 10. No Dynamic Memory Allocation

❌

```c
malloc();
free();
```

✅

```c
static uint8_t buffer[128];
```

**Reason:**

- Fragmentation
    
- Non-deterministic timing
    

---

## 11. No Recursion

❌

```c
void func(void)
{
    func();
}
```

**Reason:**

- Stack overflow risk
    
- No predictability
    

---

## 12. One Return Statement per Function

❌

```c
if (x) return 1;
return 0;
```

✅

```c
int ret = 0;

if (x)
{
    ret = 1;
}

return ret;
```

---

## 13. Function Prototypes Are Mandatory

❌

```c
func();
```

✅

```c
void func(void);
```

---

## 14. No Magic Numbers

❌

```c
if (speed > 120)
```

✅

```c
#define MAX_SPEED 120U

if (speed > MAX_SPEED)
```

---

## 15. Explicit Boolean Expressions

❌

```c
if (x)
```

✅

```c
if (x != 0U)
```

**Reason:**

- Improves readability
    
- Avoids implicit conversions
    

---

## 16. Use `const` Whenever Possible

```c
const uint32_t MAX_SIZE = 64U;
```

**Reason:**

- Protect data from modification
    
- Enables compiler optimizations
    

---

## 17. Proper Use of `volatile`

Used for:

- Hardware registers
    
- ISR-shared variables
    

```c
volatile uint32_t * const GPIO_ODR = (uint32_t *)0x40020014;
```

---

## 18. Pointer Arithmetic Is Restricted

❌

```c
ptr = ptr + 5;
```

✅

```c
ptr = &array[5];
```

---

## 19. No Implicit Function Declarations

All functions must be declared before use.

---

## 20. Header Files Rules

- Header files contain **declarations only**
    
- No function definitions
    
- Use include guards
    

```c
#ifndef GPIO_H
#define GPIO_H

void GPIO_Init(void);

#endif
```

---

## 21. Avoid Macro Side Effects

❌

```c
#define SQR(x) x*x
```

✅

```c
#define SQR(x) ((x) * (x))
```

---

## 22. Use Standard Library Carefully

Some standard library functions are restricted or forbidden:

- printf (timing issues)
    
- strcpy / strcat (buffer overflow)
    

Preferred safer alternatives or custom drivers.

---

## 23. No Mixing Signed and Unsigned Types

❌

```c
uint32_t a;
int32_t b;
if (a > b)
```

✅

```c
if (a > (uint32_t)b)
```

---

## 24. Limit Scope of Variables

```c
void func(void)
{
    uint32_t i = 0U;   // local, limited scope
}
```

---

## 25. Use Enumerations Carefully

- Do not assume enum size
    
- Do not mix enums with integers
    

---

# Rule Classification

|Type|Meaning|
|---|---|
|Mandatory|Must never be violated|
|Required|Should not be violated|
|Advisory|Recommended|

---

# Important Industry Note

✔ No real project follows MISRA 100%  
✔ Violations must be documented using:

> **MISRA Deviation Report**

This is completely normal in industry.

---

# Final Summary

MISRA-C is not about making code longer.

It is about making code:

- Predictable
    
- Reviewable
    
- Safe
    
- Maintainable
    

If you master MISRA-C, you are ready for:

- Professional Embedded projects
    
- Automotive interviews
    
- Safety-critical firmware design
    

---

**Author target:** Embedded Systems Engineers