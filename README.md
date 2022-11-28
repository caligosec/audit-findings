
# Smart Contracts Audit Findings

## Unhandled return values of transfer and transferFrom

ERC20 implementations are not always consistent. Some implementations of transfer and transferFrom could return ‘false’ on failure instead of reverting. It is safer to wrap such calls into require() statements to these failures.

Recommendation: Check the return value and revert on 0/false or use OpenZeppelin’s SafeERC20 wrapper functions

Medium severity finding from https://lnkd.in/d-hbg-q4
