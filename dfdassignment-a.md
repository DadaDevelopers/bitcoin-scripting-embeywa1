# Bitcoin Scripting Assignment - Assignment A

## Given Script

This is the standard **P2PKH (Pay-to-Public-Key-Hash)** locking script used in most Bitcoin addresses.

## 1. Break down each opcode's purpose

- **OP_DUP**: Duplicates the top item on the stack (the Public Key). This is needed because we need the original Public Key later for signature verification.
- **OP_HASH160**: Takes the duplicated Public Key, computes SHA256 followed by RIPEMD-160, producing a 20-byte hash.
- **<PubKeyHash>**: Pushes the 20-byte hash that was embedded when the output was created (the recipient's address hash).
- **OP_EQUALVERIFY**: Compares the computed hash with the stored <PubKeyHash>. If they do **not** match, the script fails immediately.
- **OP_CHECKSIG**: Verifies that the provided signature is valid for the Public Key. Pushes `1` (true) on success or `0` (false) on failure.

## 2. Data Flow Diagram (Stack Execution)

```mermaid
flowchart TD
    A["Unlocking Script<br/><b>&lt;Signature&gt; &lt;PublicKey&gt;</b><br/>Stack: [sig, pubkey]"] 
    --> B["OP_DUP<br/>Duplicates top item"]
    B --> C["Stack: [sig, pubkey, pubkey]"]
    
    C --> D["OP_HASH160<br/>SHA256 + RIPEMD160"]
    D --> E["Stack: [sig, pubkey, hash160(pubkey)]"]
    
    E --> F["Push &lt;PubKeyHash&gt;"]
    F --> G["Stack: [sig, pubkey, hash160(pubkey), &lt;PubKeyHash&gt;]"]
    
    G --> H["OP_EQUALVERIFY<br/>Compare the two hashes<br/>(fails script if not equal)"]
    H --> I["Stack: [sig, pubkey]<br/>(only if hashes match)"]
    
    I --> J["OP_CHECKSIG<br/>Verify signature with pubkey"]
    J --> K["Success: [1] (true)<br/>Transaction accepted"]
    J --> L["Failure: [0] (false)<br/>Transaction rejected"]

    style A fill:#e3f2fd,stroke:#1976d2
    style K fill:#c8e6c9,stroke:#388e3c
    style L fill:#ffcdd2,stroke:#d32f2f
