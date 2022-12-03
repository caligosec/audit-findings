
# Smart Contracts Audit Findings

## Unhandled return values of transfer and transferFrom

ERC20 implementations are not always consistent. Some implementations of transfer and transferFrom could return ‘false’ on failure instead of reverting. It is safer to wrap such calls into require() statements to these failures.

Recommendation: Check the return value and revert on 0/false or use OpenZeppelin’s SafeERC20 wrapper functions

Medium severity finding from https://lnkd.in/d-hbg-q4

## Random task execution

In a scenario where a user takes a flash loan, _parseFLAndExecute() gives the flash loan wrapper contract (FLAaveV2, FLDyDx) the permission to execute functions on behalf of the user’s DSProxy. This execution permission is revoked only after the entire recipe execution is finished, which means that in case that any of the external calls along the recipe execution is malicious, it might call executeAction() back, i.e. Reentrancy Attack, and inject any task it wishes (e.g. take user’s funds out, drain approved tokens, etc)

Recommendation: A reentrancy guard (mutex) should be used to prevent such attack

Critical severity finding from https://lnkd.in/dQ4tqvRG

## Tokens with more than 18 decimal points will cause issues:

It is assumed that the maximum number of decimals for each token is 18. However uncommon, it is possible to have tokens with more than 18 decimals, as example YAMv2 has 24 decimals. This can result in broken code flow and unpredictable outcomes

Recommendation: Make sure the code won’t fail in case the token’s decimals are more than 18

Major severity finding from https://lnkd.in/digeFy6Z
