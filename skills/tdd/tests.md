# Test-quality examples

## Observable behavior

Prefer a caller-visible result over an internal helper assertion:

```typescript
const user = await createUser({ name: 'Alice' });
expect((await getUser(user.id)).name).toBe('Alice');
```

A database read is appropriate when database persistence is itself the contract. An internal
method call is not a contract merely because a spy can observe it. Likewise, a payment request's
amount or count can be the exact behavior a caller needs guaranteed.

## Boundary effects and positive controls

To verify that an empty cart never charges, observe the payment request through a recording test
double while running the real checkout logic. Include a valid-cart case that does charge, proving
that the observation can detect the effect. Assert the promised amount and count. Do not replace
checkout with a mock and then assert what that mock was configured to return.

Choose doubles for the control the test needs, rather than a rule about which dependency category
must be mocked. If setup requires recreating substantial application logic, reconsider the test
interface or use real collaborators in an integration test.

## Independent oracles

This expectation cannot detect a deterministic calculation bug:

```typescript
const expected = calculateTotal(items);
expect(calculateTotal(items)).toBe(expected);
```

A worked example provides an independent expectation:

```typescript
expect(calculateTotal([{ price: 10 }, { price: 5 }])).toBe(15);
```

An independent reference model or calculation can also be useful. Repeating the production
algorithm is weaker because the same misunderstanding can appear on both sides; it is not
necessarily an assertion that can never fail.

## One behavior and DAMP

A transfer test can assert both the sender's debit and the recipient's credit. Those assertions
establish one behavior. Keep relevant amounts and expectations visible in the scenario; extract
setup only when it improves readability without hiding the contract.

For a cancellation race, control readiness with a barrier and observe the promised result and
absence of later publication. A sleep followed by an incidental internal-state check is weaker.
