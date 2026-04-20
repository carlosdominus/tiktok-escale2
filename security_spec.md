# Security Specification for Tiktok Escale

## Data Invariants
- A **User** profile must match the `request.auth.uid`.
- Users cannot set their own `role` to `admin`.
- A **Sale** must be owned by the user who created it (`userId == request.auth.uid`).
- **Sales** status can only be set to `paid` by an admin (or server).
- **Webhook Logs** are strictly for administrative eyes only.
- All IDs must be valid alphanumeric strings.
- All string values must have reasonable size limits.

## The Dirty Dozen Payloads

| # | Entity | Action | Malicious Intent | Expected result |
|---|--------|--------|------------------|-----------------|
| 1 | User | Create | Self-assigned "admin" role | REJECTED |
| 2 | User | Update | Escalate from "user" to "admin" | REJECTED |
| 3 | User | Update | Modify another user's profile | REJECTED |
| 4 | Sale | Create | Create sale for different `userId` | REJECTED |
| 5 | Sale | Update | Self-approve status to "paid" | REJECTED |
| 6 | Sale | Read | Access sales of another user | REJECTED |
| 7 | Sale | Create | Inject 1MB string into `packageId` | REJECTED |
| 8 | Sale | Create | Invalid ID format (non-alphanumeric) | REJECTED |
| 9 | Sale | Delete | Delete transaction history | REJECTED |
| 10 | WebhookLog | Read | View internal payment logs | REJECTED |
| 11 | WebhookLog | Create | Inject fake payment log | REJECTED |
| 12 | Sale | Update | Change `userId` of an existing sale | REJECTED |

## Test Runner (firestore.rules.test.ts)
```typescript
import {
  assertFails,
  assertSucceeds,
  initializeTestEnvironment,
} from "@firebase/rules-unit-testing";
import { doc, setDoc, getDoc, updateDoc, deleteDoc } from "firebase/firestore";

// Tests will be implemented here
```
