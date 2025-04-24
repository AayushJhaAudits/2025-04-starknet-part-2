# 🛡️ Audit Report by Aayush Jha (aayushjhaaudits)

## Target Contract: `interface.cairo`
- Repo: [https://github.com/CodeHawks-Contests/2025-04-starknet-part-2](https://github.com/CodeHawks-Contests/2025-04-starknet-part-2)
- Severity: High

---

## Finding: ❗ Missing Access Control in `update_user_votes()` and `update_rewards()` Functions

### 🔍 Description

The `interface.cairo` contract lacks any form of access control on critical state-changing functions like:

- `update_user_votes()`
- `update_rewards()`

These functions allow any external account to modify internal user and reward states of the contract, potentially leading to:

- Unauthorized manipulation of staking/reward logic
- Fake or malicious vote weight updates
- Inaccurate accounting of user rewards

---

### 🧪 Proof of Concept (PoC)

```python
# Any malicious actor can call this directly
interface.update_user_votes(
    user=attacker_address,
    token=target_token,
    vote=MAX_UINT256
)

# Or update_rewards to fake an event
interface.update_rewards(
    user=attacker_address,
    token=target_token,
    reward_amount=1_000_000_000
)
```
💥 Impact
High – This can completely compromise the reward distribution and staking logic, and may allow economic manipulation across the system.

