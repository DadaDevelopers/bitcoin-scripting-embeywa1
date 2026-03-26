# Bitcoin Scripting Assignment - Assignment B

## Hashed Time-Lock Contract (HTLC) for Atomic Swap

**Scenario**:  
Alice and Bob want to do an atomic swap. Alice locks funds in an HTLC.  
- Alice can claim the funds **immediately** by revealing her secret preimage + her signature.  
- Bob can refund the funds **after 21 minutes** using only his signature.

### 1. Complete HTLC Redeem Script

```bitcoin
OP_IF
    # Alice's path (Hash Lock + Signature)
    OP_HASH160 <HashOfSecret> OP_EQUALVERIFY
    <Alice_PublicKey> OP_CHECKSIG
OP_ELSE
    # Bob's path (Time Lock + Signature)
    <Timeout> OP_CHECKLOCKTIMEVERIFY OP_DROP
    <Bob_PublicKey> OP_CHECKSIG
OP_ENDIF

```
Explanation:

OP_IF chooses the branch: 1 = Alice claims, 0 = Bob refunds.
Alice branch: Requires correct secret preimage (hashed with HASH160) + her signature.
Bob branch: Requires the transaction's locktime to be after the timeout + his signature.

2. Claiming Transaction Script (Alice claims)
Alice’s unlocking script (what she provides):

<Alice_Signature> <Secret_Preimage> 1

3. Refund Transaction Script (Bob gets refund after timeout)
Unlocking Script (scriptSig) provided by Bob:

<Bob_Signature> 0
 
4. Test with Sample Hash and Timeout
Sample values for testing:

Alice's secret preimage: mysecret12345
Hash of secret (embed this in the script): Compute HASH160("mysecret12345") → example: a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0 (in real tests use a real hash)
Timeout: 21 minutes
→ Use 126 blocks (assuming ~10 min per block) for relative timeout, or an absolute Unix timestamp.


```bitcoin

OP_IF
    OP_HASH160 a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0 OP_EQUALVERIFY
    02alicepublickey1234567890abcdef... OP_CHECKSIG
OP_ELSE
    126 OP_CHECKLOCKTIMEVERIFY OP_DROP
    02bobpublickeyabcdef1234567890... OP_CHECKSIG
OP_ENDIF

```
How to test:

Alice's claim: Provide secret + signature + 1
Bob's refund: After timeout passes, provide signature + 0


