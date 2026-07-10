---
name: refactor
description: Refactor code to improve quality, apply SOLID principles, remove code smells, and suggest better patterns. Use when improving code structure or modernizing legacy code.
---

# Code Refactoring Skill

Improve code quality through systematic refactoring.

## Instructions

- The priorities are readibility and reusability. Behaviour should **always be preserved**.
- Do not over-refactor. Stop when a change no longer improves readability or reusability, when it introduces speculative abstraction, or when it risks changing observable behaviour. Prefer small, incremental refactors over large rewrites.
- Refactor parts of code which are applied multiple times and can be converted into functions or classes. Do not break up functions unless they are exceedingly complicated to read and parse. If multiple comments are available in a function to describe multiple steps, this is a good indication that the function can be split into multiple functions
- If multiple functions perform similar tasks which can be more easily controlled by function arguments, these are also good targets for refactoring
- Replace magic numbers:
**Before:**
```javascript
function calculateDiscount(price) {
    if (price > 100) {
        return price * 0.15;
    }
    return price * 0.05;
}
```
**After:**
```javascript
const DISCOUNT_THRESHOLD = 100;
const REGULAR_DISCOUNT_RATE = 0.05;
const PREMIUM_DISCOUNT_RATE = 0.15;

function calculateDiscount(price) {
    const rate = price > DISCOUNT_THRESHOLD
        ? PREMIUM_DISCOUNT_RATE
        : REGULAR_DISCOUNT_RATE;
    return price * rate;
}
```
- Simplify conditional logic both for error raising and when returning values based on conditions. For example:
**Before:**
```python
def get_user_status(user):
    if user.is_active:
        if user.subscription:
            if user.subscription.is_paid:
                return "premium"
            else:
                return "active"
        else:
            return "active"
    else:
        return "inactive"
```
**After:**
```python
def get_user_status(user):
    if not user.is_active:
        return "inactive"

    if user.subscription and user.subscription.is_paid:
        return "premium"

    return "active"
```
- Try to simplify data models and classes if they can be compartimentalised. Importantly, when extracting classes, consider not only semantic attributes but also how much reusability this enables
- Use and implement class inheritance and polymorphism to replace conditional type checks with polymorphic dispatch.
- Remove duplications. Example:
**Before:**
```python
def send_welcome_email(user):
    subject = "Welcome!"
    body = f"Hello {user.name}"
    send_email(user.email, subject, body)

def send_reset_email(user):
    subject = "Password Reset"
    body = f"Hello {user.name}, click here to reset"
    send_email(user.email, subject, body)
```
**After:**
```python
def send_user_email(user, subject, message_template):
    body = message_template.format(name=user.name)
    send_email(user.email, subject, body)

def send_welcome_email(user):
    send_user_email(user, "Welcome!", "Hello {name}")

def send_reset_email(user):
    send_user_email(user, "Password Reset",
                   "Hello {name}, click here to reset")
```
- Make use of the following SOLID principles (other principles have already been stated or should be disregarded):
  - The **Single Responsability Principle** states that each function/class should execute a straightforward set of steps. However, if a workflow is self-contained and does not increase the amount of code duplication, you need not attempt to correct this through this principle
  - The **Open/Closed Principle** can be freely applied, particularly through i) refactoring functions and ii) class inheritance/polymorphism
  - The **Interface Seggregation Principle** suggests modularity. This is in general a good rule to follow, but if there is no immediate and obvious application for modularity this principle should be relaxed
- Use idiomatic modern patterns. In Python, prefer list comprehensions, generator expressions, `match`/`case`, the walrus operator, decorators, and dataclasses. In JavaScript, prefer destructuring, arrow functions, optional chaining, template literals, and `async`/`await`. Choose the feature that most clearly expresses intent, not merely the newest one.
- When attempting significant refactors, create tests which focus on evaluating and preserving the current behaviour. During refactoring run these tests to avoid regressions

## When to Use This Skill

Use `/refactor` when:
- Code is difficult to understand
- Functions are too long
- Classes have too many responsibilities
- Code has duplication
- Preparing for new features
- Improving test coverage
- Modernizing legacy code
- Applying design patterns
