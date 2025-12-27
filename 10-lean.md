# Lean Query
lean() returns plain js object instead of full mongoose document.
-It's faster than normal query 
-No methods/virtuals
-No change tracking

### Usage
```js
const user = await User.find({}).lean()
```