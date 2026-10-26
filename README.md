The plaintext spec lives in the backend. On verification the Rust program recomputes that spec hash and hashes the delivered result. Express releases escrow only when the spec hash matches the chain and the result hash matches the committed output. A mismatch is logged and the funds stay locked. Admin is the only party who triggers that check.

## What each role reviews

| Role | Look here | What is being tested |
|---|---|---|
| Solidity | `contracts/contracts/EscrowMarketplace.sol` and `contracts/test/EscrowMarketplace.test.js` | Escrow state, the spec-hash commitment, who may release or refund |
| Rust | `verifier/src/main.rs` | Canonical hashing and accept/reject of a result |
| Backend | `backend/src/routes/verifyDelivery.js`, `backend/src/verifier.js` | Calling the verifier, comparing it to the chain, then releasing |
| Frontend | `frontend/` | Publishing a spec, funding a job, and the admin check |

The Solidity names stay close to the original escrow: `seller` is the provider, `buyer` is the requester, and a listing is still a `Voucher`. The interface calls them provider, requester, and job.