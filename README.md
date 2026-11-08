# EtherShield: Overall Program & Task Breakdown

## 1. Program Overview

**EtherShield** is a practical blockchain cybersecurity laboratory .

The program focuses on developing, auditing, safely exploiting, and remediating Solidity smart contracts on a local Ethereum network using simulated funds.

### Main Objectives
- Develop vulnerable Solidity smart contracts.
- Understand and reproduce common blockchain security vulnerabilities.
- Execute controlled exploit scenarios in a local environment.
- Implement security fixes and mitigation techniques.
- Document vulnerabilities, attacks, and remediation strategies.

------------------------------------------------------------------------------------------------------------

## 2. Team Workload & Task Distribution

There are 6 vulnerabilities and 3 students. The vulnerabilities are grouped into 3 pairs, and each student rotates through three tasks:

**Build**: write the vulnerable contract that contains the bug.
**Attack**: run 2 attacks on that contract with automated tests to prove the bug works.
**Report**: write up the bug: what it is, how it was attacked, how much damage it does, and how to fix it.

Student	Pair 1: Reentrancy, Access Control	
Student Pair 2: tx.origin, Signature Replay	
Student Pair 3: Front-running, Denial of Service

Student 1	Build	Attack	Report
Student 2	Attack	Report	Build
Student 3	Report	Build	Attack

**Team organization:** 
Each student is responsible for developing, testing, and documenting their assigned vulnerabilities.
Each pair delivers 2 vulnerable contracts, 4 attacks (2 per bug), and 2 report sections.

--------------------------------------------------------------------------------------------------------------------------

## 3. The 6 Core Vulnerabilities & Attack Scenarios

### 3.1 Reentrancy

**Explanation:**  
The bank pays out money before updating the user's balance, allowing repeated withdrawals through recursive external calls.

**Attack 1: Repeated Withdrawal**
- Withdraw repeatedly until the vault is empty.

**Attack 2: Large Vault Drain**
- Drain a large vault using a small initial deposit.

**Proposed Mitigation:**
- Checks-Effects-Interactions (CEI).
- ReentrancyGuard.

### 3.2 Access Control

**Explanation:**  
Unprotected settings allow unauthorized users to modify critical contract configurations.

**Attack 1: Unauthorized Fee Modification**
- A random user changes the contract fee.

**Attack 2: Unauthorized Ownership Transfer**
- A random user takes full ownership of the contract.

**Proposed Mitigation:**
- Ownable.
- AccessControl.
- Role-based permissions.

### 3.3 tx.origin

**Explanation:**  
The contract relies on the initiator of the call chain rather than the direct caller, creating a vulnerability to malicious intermediary contracts.

**Attack 1: Phishing Contract**
- A phishing contract drains the wallet when the owner interacts with it.

**Attack 2: Fake Airdrop**
- A fake airdrop tricks the owner into making an unauthorized call.

**Proposed Mitigation:**
- Use `msg.sender` instead of `tx.origin` for authorization.
- Validate permissions at the direct caller level.

### 3.4 Signature Replay

**Explanation:**  
Valid cryptographic signatures can be captured and reused multiple times.

**Attack 1: Duplicate Reward Claim**
- Use the same signature twice to claim a reward.

**Attack 2: Cross-Contract Replay**
- Reuse a valid signature on another contract instance.

**Proposed Mitigation:**
- Nonce management.
- Signature expiration.
- EIP-712 domain separation.

### 3.5 Front-running / MEV

**Explanation:**  
Attackers monitor pending transactions and submit competing transactions with higher priority fees to execute first.

**Attack 1: Copying a Pending Answer**
- Copy a pending answer and claim the reward first.

**Attack 2: Automated Front-running**
- Use an automated bot to pay higher gas fees to win the race.

**Proposed Mitigation:**
- Commit-reveal mechanisms.
- Transaction ordering protections.
- Slippage limits where applicable.

### 3.6 Denial of Service (DoS)

**Explanation:**  
Flawed logic or failing external calls can block essential operations for everyone.

**Attack 1: Reverting Recipient**
- A recipient contract that always reverts stops payout execution.

**Attack 2: Unbounded Loop**
- An unbounded loop makes payouts too expensive in gas, blocking transactions.

**Proposed Mitigation:**
- Pull-payment architecture.
- Avoid unbounded loops.
- Handle external call failures safely.

---

## 4. Project Technologies

- **Solidity:** Smart contract development.
- **Ethereum:** Blockchain environment.
- **Local Ethereum Network:** Safe testing with simulated funds.
- **Remix IDE / Hardhat:** Development and testing.
- **Git & GitHub:** Version control and collaboration.

---

## 5. Expected Deliverables

At the end of the project, we should provide:

1. Six vulnerable Solidity smart contracts.
2. Twelve exploit scenarios (two per vulnerability).
3. Six corrected smart contract implementations.
4. Security tests demonstrating vulnerabilities and fixes.
5. Technical documentation and individual vulnerability reports.
6. A final project demonstration.

---
