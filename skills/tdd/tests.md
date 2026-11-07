# Test examples

- `createUser` then `getUser` proves caller behavior; a database read is appropriate if persistence
  is the contract. Spying on an internal call alone does not make it a contract.
- Empty-cart checkout: real checkout + recording payment double; assert no charge and a valid-cart
  positive control with promised amount/count. Do not mock checkout itself. Choose doubles for
  needed control, not dependency category; substantial recreated application logic suggests a higher
  interface/real collaborators.
- `expect(calculateTotal(items)).toBe(calculateTotal(items))` cannot detect deterministic
  calculation errors. An independent literal example, `10 + 5 → 15`, or reference model can.
  Duplicating the production algorithm may copy its mistake without being strictly tautological.
- Debit + credit assertions can establish one transfer behavior. Keep scenario values visible;
  extract setup only for readability.
- Cancellation: control readiness with a barrier, observe promised result/no late publication;
  prefer this to sleeps and incidental internal state.
