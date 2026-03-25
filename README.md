# assignment-4
Bitcoin Scripting

# Bitcoin Scripting Assignment

## Assignment A

### Given Script:
```
OP_DUP OP_HASH160 <PubKeyHash> OP_EQUALVERIFY OP_CHECKSIG
```

### Tasks:

1. Break down each opcode's purpose
2. Create a diagram showing data flow
3. Identify what happens if signature verification fails
4. Explain the security benefits of hash verification

---

## Assignment B

### Implement a Hashed Time-Lock Contract for atomic swap between Alice and Bob:

- Alice can claim with secret preimage within 21 minutes
- Bob gets refund after 21 minutes

### Tasks:

1. Complete the HTLC script
2. Create claiming transaction script
3. Create refund transaction script
4. Test with sample hash and timeout
