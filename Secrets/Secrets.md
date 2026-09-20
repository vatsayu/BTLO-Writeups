# BTLO – Secrets

**Platform:** Blue Team Labs Online (BTLO)  
**Category:** Incident Response  
**Difficulty:** Easy  
**Points:** 10  
**Status:** ✅ Completed  
**Date:** September 2026  

---

## Scenario

You’re a senior cyber security engineer and during your shift, we have intercepted/noticed a high privilege actions from unknown source that could be identified as malicious. We have got you the ticket that made these actions.  
You are the one who created the secret for these tickets. Please fix this and submit the low privilege ticket so we can make sure that you deserve this position.

**Ticket (JWT):**
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmbGFnIjoiQlRMe180X0V5ZXN9IiwiaWF0Ijo5MDAwMDAwMCwibmFtZSI6IkdyZWF0RXhwIiwiYWRtaW4iOnRydWV9.jbkZHll_W17BOALT95JQ17glHBj9nY-oWhT1uiahtv8
```

---

## Theory – What is a JWT?

**JWT (JSON Web Token)** is a compact, URL-safe way to represent claims between two parties. It is commonly used for authentication and authorization.

A JWT has **three parts** separated by dots (`.`):

```
Header.Payload.Signature
```

| Part | Purpose |
|------|---------|
| **Header** | Contains the token type (`JWT`) and the signing algorithm (`HS256`, `RS256`, etc.) |
| **Payload** | Contains the claims (data) – user info, roles, expiry, custom flags, etc. |
| **Signature** | Ensures the token has not been tampered with. Created by signing `Header + Payload` with a secret (for HS256) or private key (for RS256) |

### HS256 (used in this challenge)
- Symmetric algorithm
- Same secret is used to **sign** and **verify** the token
- If the secret is weak, it can be brute-forced

### Why this matters for Blue Team
- Attackers who obtain a weak JWT secret can forge tokens and escalate privileges
- Always use strong, long, random secrets
- Monitor for tokens with unexpected claims (`admin: true`, etc.)

---

## Tools Used

- [jwt.io](https://jwt.io) – Decode / Encode / Sign JWTs
- CyberChef (optional) – Base64 decoding
- Hashcat – Brute-force the HS256 secret
- Terminal (Kali)

---

## Investigation & Answers

### Question 1
**Can you identify the name of the token?**  
(Format: String)

**Method:**
1. Paste the token into [jwt.io](https://jwt.io)
2. The interface clearly labels it as a JWT
3. Header shows `"typ": "JWT"`

**Answer:** `JWT`

---

### Question 2
**What is the structure of this token?**  
(Format: Section.Section.Section)

**Method:**
- JWT standard structure is always three Base64url-encoded parts separated by dots

**Answer:** `Header.Payload.Signature`

---

### Question 3
**What is the hint you found from this token?**  
(Format: String)

**Method:**
1. Decode the **Payload** section on jwt.io (or with base64)
2. Payload content:
```json
{
  "flag": "BTL{_4_Eyes}",
  "iat": 90000000,
  "name": "GreatExp",
  "admin": true
}
```
3. The flag contains the hint `_4_Eyes`

**Answer:** `_4_Eyes`

---

### Question 4
**What is the Secret?**  
(Format: String)

**Method:**
The signature is invalid until we know the secret used to sign the token. Since the algorithm is **HS256**, we can brute-force a weak secret.

```bash
# Save the token
echo 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmbGFnIjoiQlRMe180X0V5ZXN9IiwiaWF0Ijo5MDAwMDAwMCwibmFtZSI6IkdyZWF0RXhwIiwiYWRtaW4iOnRydWV9.jbkZHll_W17BOALT95JQ17glHBj9nY-oWhT1uiahtv8' > jwt.txt

# Brute-force with Hashcat (mode 16500 = JWT)
hashcat -m 16500 jwt.txt -a 3 ?a?a?a?a
```

**Hashcat output (cracked):**
```
...jbkZHll_W17BOALT95JQ17glHBj9nY-oWhT1uiahtv8:bT!0
Status...........: Cracked
```

**Answer:** `bT!0`

> Note: The secret is case-sensitive (`bT!0` – capital T).

---

### Question 5
**Can you generate a new verified signature ticket with a low privilege?**  
(Format: String.String.String)

**Method:**
1. Go to [jwt.io](https://jwt.io) → switch to **JWT Encoder** (or use the Debugger)
2. Keep the same **Header**:
```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```
3. In the **Payload**, change only the admin value:
```json
{
  "flag": "BTL{_4_Eyes}",
  "iat": 90000000,
  "name": "GreatExp",
  "admin": false
}
```
4. In the **Secret** field, enter exactly: `bT!0`
5. jwt.io generates a new valid signed token

**Answer:**
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmbGFnIjoiQlRMe180X0V5ZXN9IiwiaWF0Ijo5MDAwMDAwMCwibmFtZSI6IkdyZWF0RXhwIiwiYWRtaW4iOmZhbHNlfQ.nMXNFvttCvtDcpswOQA8u_LpURwv6ZrCJ-ftIXegtX4
```

---

## Summary of Findings

| # | Question | Answer |
|---|----------|--------|
| 1 | Token name | JWT |
| 2 | Structure | Header.Payload.Signature |
| 3 | Hint | _4_Eyes |
| 4 | Secret | bT!0 |
| 5 | Low-privilege token | `eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmbGFnIjoiQlRMe180X0V5ZXN9IiwiaWF0Ijo5MDAwMDAwMCwibmFtZSI6IkdyZWF0RXhwIiwiYWRtaW4iOmZhbHNlfQ.nMXNFvttCvtDcpswOQA8u_LpURwv6ZrCJ-ftIXegtX4` |

---

## Key Takeaways

- JWTs signed with **HS256** and a weak secret can be cracked offline with Hashcat (mode 16500)
- Always treat the secret as case-sensitive
- Privilege escalation via JWT is a common real-world issue — changing `"admin": true` to `false` (or vice-versa) after cracking the secret is a classic attack
- Blue Team detection ideas:
  - Monitor for unexpectedly privileged claims
  - Use strong, long, random secrets (never short or guessable)
  - Prefer asymmetric algorithms (RS256) when possible
  - Implement short token lifetime + proper revocation

---

## Commands Reference

```bash
# Save token
echo 'TOKEN_HERE' > jwt.txt

# Crack JWT secret (4-char brute force)
hashcat -m 16500 jwt.txt -a 3 ?a?a?a?a

# Show cracked result later
hashcat -m 16500 jwt.txt --show
```

---

## References

- [jwt.io](https://jwt.io) – Official JWT debugger
- Hashcat mode 16500 – JWT (HS256)
- JWT RFC 7519

---

**Challenge completed and documented.**  
Ready to upload to: https://github.com/vatsayu/BTLO-Writeups
