# Instance and Static Methods

## Instance Methods

- Method attached to a **single document**
- Used when logic depends on that document’s data
- `this` refers to the **document**

### Syntax
```js
schema.methods.methodName = function () {
  return this.someField;
};
```
Ex:
```js
userSchema.methods.checkEmailDomain = function () {
  return this.email.split('@')[1];
};
```
Usage:
```js
const user = await User.findOne();
user.checkEmailDomain();
```
## Static Methods

- Method attached to the **model**
- Used for collection-level logic
- `this` refers to the **model**

### Syntax
```js
schema.statics.methodName = function () {
  return this.find({});
};
```
Ex:
```js
userSchema.statics.findActiveUsers = function () {
  return this.find({ isActive: true });
};
```

Usage:
```js
await User.findActiveUsers();
```

