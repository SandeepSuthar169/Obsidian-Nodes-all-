
# 1. DRY — Don’t Repeat Yourself

Avoid duplicating logic. Put shared behavior in one place.

## ❌ Bad

```js
function calculateUserDiscount(price) { 
	 return price * 0.1;
}
function calculateAdminDiscount(price) { 
	 return price * 0.1;
}
```

Same logic repeated.

---

## ✅ Good

```js
function calculateDiscount(price, percentage) { 
	 return price * percentage;
}

const userDiscount = calculateDiscount(100, 0.1);
const adminDiscount = calculateDiscount(100, 0.1);
```

### Why it matters

- Easier updates
- Fewer bugs
- Cleaner codebase

---

# 2. KISS — Keep It Simple, Stupid

Prefer simple solutions over complicated ones.

## ❌ Overengineered

```js
function isEven(num) {  
	return Boolean(num % 2 === 0 ? true : false);
}
```

---

## ✅ Simple

```js
function isEven(num) {  
	return num % 2 === 0;
}
```

### Why it matters

- Easier to read
- Easier to debug
- Easier for teams to maintain

---

# 3. YAGNI — You Aren’t Gonna Need It

Don’t build features until they are actually needed.

## ❌ Premature abstraction

```js
function saveToDatabase(data, type, futureFeature, futureAnalytics) {                  console.log(data);
}
```

Unused parameters = unnecessary complexity.

---

## ✅ Build only what’s needed

```js
function saveToDatabase(data) { 
   console.log(data);
}
```

### Why it matters

- Faster development
- Less complexity
- Less dead code

---

# 4. LoD — Law of Demeter

A function should know as little as possible about internal structures.

> “Talk to friends, not strangers.”

## ❌ Tight coupling

```js
const city = user.address.location.city;
```

This depends on deep object structure.

---

## ✅ Encapsulated access

```js
function getUserCity(user) { 
   return user.city;   
}

const city = getUserCity(user);
```

### Why it matters

- Reduces coupling
- Easier refactoring
- Safer object changes

---

# 5. SRP — Single Responsibility Principle

A function/module should have one reason to change.

## ❌ Multiple responsibilities

```js
function processUser(user) {
  validateUser(user);
  saveUser(user);
  sendWelcomeEmail(user);
}
```

Validation, persistence, and email are mixed together.

---

## ✅ Split responsibilities

```js
function validateUser(user) {
  return user.email && user.name;
}

function saveUser(user) {
  console.log("Saving user...");
}

function sendWelcomeEmail(user) {
  console.log("Sending email...");
}
```

### Why it matters

- Easier testing
- Easier maintenance
- Better organization

---

# 6. OCP — Open/Closed Principle

Code should be:

- open for extension
- closed for modification

Add behavior without changing existing code.

## ❌ Modifying existing logic repeatedly

```js
function calculatePrice(type, price) {
  if (type === "regular") {
    return price;
  }

  if (type === "premium") {
    return price * 0.8;
  }

  if (type === "vip") {
    return price * 0.5;
  }
}
```

Every new type requires editing the function.

---

## ✅ Extend behavior

```js
const pricingStrategies = {
  regular: price => price,
  premium: price => price * 0.8,
  vip: price => price * 0.5,
};

function calculatePrice(type, price) {
  return pricingStrategies[type](price);
}
```

### Why it matters

- Easier feature additions
- Lower risk of breaking old code

---

# 7. LSP — Liskov Substitution Principle

Replacing one implementation with another should not break behavior.

Without classes, think of it as:

> “Different implementations should follow the same contract.”

## ✅ Good example

```js
function sendNotification(service, message) {
  service.send(message);
}

const emailService = {
  send(message) {
    console.log("Email:", message);
  }
};

const smsService = {
  send(message) {
    console.log("SMS:", message);
  }
};

sendNotification(emailService, "Hello");
sendNotification(smsService, "Hello");
```

Both implementations work the same way.

---

## ❌ Bad example

```js
const brokenService = {
  send() {
    throw new Error("Not supported");
  }
};
```

This violates the expected behavior.

### Why it matters

- Predictable systems
- Safer replacements
- Better abstractions

---

# 8. ISP — Interface Segregation Principle

Don’t force modules to depend on things they don’t use.

In JavaScript, this means:

- keep APIs small
- avoid giant utility objects

---

## ❌ Large unnecessary API

```js
const userService = {
  createUser() {},
  deleteUser() {},
  generatePDF() {},
  sendSMS() {},
};
```

A module needing only `createUser` now depends on everything.

---

## ✅ Smaller focused modules

```js
const userService = {
  createUser() {},
  deleteUser() {},
};

const notificationService = {
  sendSMS() {},
};

const reportService = {
  generatePDF() {},
};
```

### Why it matters

- Better modularity
- Lower coupling
- Easier testing

---

# 9. DIP — Dependency Inversion Principle

Depend on abstractions, not concrete implementations.

## ❌ Hardcoded dependency

```js
function processPayment(amount) {
  const stripe = {
    charge: value => console.log(`Charging ${value}`)
  };

  stripe.charge(amount);
}
```

Payment processor is tightly coupled.

---

## ✅ Inject dependency

```js
function processPayment(paymentProvider, amount) {
  paymentProvider.charge(amount);
}

const stripe = {
  charge(amount) {
    console.log(`Stripe charged ${amount}`);
  }
};

const paypal = {
  charge(amount) {
    console.log(`PayPal charged ${amount}`);
  }
};

processPayment(stripe, 100);
processPayment(paypal, 100);
```

### Why it matters

- Easier testing
- Easier swapping implementations
- More flexible architecture

---

# Quick Summary

|Principle|Main Idea|
|---|---|
|DRY|Avoid duplication|
|KISS|Prefer simplicity|
|YAGNI|Don’t build unnecessary features|
|LoD|Reduce coupling|
|SRP|One responsibility per module|
|OCP|Extend without modifying|
|LSP|Implementations should be replaceable|
|ISP|Keep interfaces small|
|DIP|Depend on abstractions|