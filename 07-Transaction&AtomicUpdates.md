# Transactions & Atomic Updates

## Difference between Atomic and non-Atomic update

Atomic:-The update happens in one step inside MongoDB. It either fully succeeds or doesn’t happen at all.

ex:
await User.updateOne(
  { _id: id, balance: { $gte: 500 } },
  { $inc: { balance: -500 } }
);
<!-- MongoDB reads + updates in one operation
No other request can modify this document mid-update
No race condition
Final state is always correct -->

Non-Atomic:-The update is split into multiple steps (read → modify → write).
ex:
const user = await User.findById(id); // read
user.balance -= 500;                 // modify
await user.save();                   // write

<!-- What happens:
Two requests can read the same old value
One update can overwrite the other
Race condition possible
Incorrect final data -->

## Transaction
To update more than on collection safely

ex:deduct balance + create order

const session = await mongoose.startSession();
session.startTransaction();

try {
  await User.updateOne(
    { _id: userId },
    { $inc: { balance: -500 } },
    { session }
  );

  await Order.create(
    [{ userId, amount: 500 }],
    { session }
  );

  await session.commitTransaction();
} catch (e) {
  await session.abortTransaction();
}
