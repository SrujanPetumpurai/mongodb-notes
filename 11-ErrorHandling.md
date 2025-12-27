# ErrorHandling

### 1.Basic(Try-catch)
```js
try {
  const user = await User.create({ email: 'a@b.com' });
} catch (err) {
  if (err instanceof Error) {
    console.log(err.message);
  }
}
```
### 2.Validation Error
```js
try {
  await User.create({}); // missing required fields
} catch (err: any) {
  if (err.name === 'ValidationError') {
    console.log(err.errors);
  }
}

```
### 3.Duplicate Key Error(unique index)
```js
try {
  await User.create({ email: 'a@b.com' });
} catch (err: any) {
  if (err.code === 11000) {
    console.log('Email already exists');
  }
}
```
### 4.Custom/Not-found Error
```js
const user = await User.findById(id);

if (!user) {
  throw new Error('User not found');
}
```
Should be handled by middleware:
```js
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  res.status(400).json({ message: err.message });
});
```
### 5.Cast Error(invalid ObjectId)
```js
try {
  await User.findById('123'); // invalid ObjectId
} catch (err: any) {
  if (err.name === 'CastError') {
    console.log('Invalid ID format');
  }
}
```

