# Wh00ps Leaked Too Much


## Description
I just learnt how to encrypt my messages using a symmetric cipher.
I hope no one can read it now.

Authors: teayah | applemangoorange

## Encryption Overview

- The encryption does not use AES directly on the plaintext. Instead:
- AES-ECB is used as a keystream generator
- Plaintext is encrypted using XOR with the keystream
- A 128-bit internal state (nonce) is maintained
- The initial nonce is the AES key itself
- On every round:
   1. The MSB of the nonce is appended to a bitstring shifts
   2. The nonce is updated as: `nonce = rol(nonce + int(shifts, 2), 3)`
- The final ciphertext and the value of shifts are written to out.txt

## Vulnerability Analysis
1. Stream Cipher Construction
    1. The scheme behaves as a stream cipher:
    2. ciphertext = plaintext ⊕ keystream
Any leakage of keystream state compromises confidentiality.

2. Internal State Leakage  
One bit of the nonce (its MSB) is leaked every round via shifts.
Since:  
   >The nonce update is deterministic
   >The nonce starts as the AES key
   >Enough bits are leaked  
This allows full recovery of the internal state.

3. Integer Encoding Bug
The shifts bitstring is written as: `int(shifts, 2)`

This conversion drops leading zero bits.  
When reconstructing shifts, the original bit-length must be restored, otherwise the leaked bits become misaligned with encryption rounds.

## Exploitation Strategy
1. Restore Shift Bit Alignment

Each keystream block leaks exactly one bit.  
>num_blocks = len(ciphertext) // 16

The recovered bitstring is left-padded with zeros to restore the correct length:

>raw = bin(int(shifts_hex, 16))[2:]
>raw = raw.zfill(num_blocks)
>shift_bits = list(map(int, raw))

2. Symbolic State Recovery with Z3
The nonce evolution is modeled exactly using Z3:
   - The initial nonce is a 128-bit symbolic variable
   - For each round:
        - Constrain the MSB to the leaked bit
        - Apply the same update logic as the challenge

Z3 efficiently solves this constraint system and recovers the unique AES key.

Recovered key: `914ce4a90083c4b90fd472fa7eb4e0fe`

3. Keystream Regeneration and Decryption

Using the recovered key:
   1. The keystream is regenerated exactly
   2. Ciphertext is XORed with the keystream
   3. The original message is recovered

The plaintext consists of random bytes with the flag embedded inside.

4. Flag Extraction
The decrypted plaintext is scanned at the byte level for the known flag format: `nite{...}`

## Solve
**Flag:** `nite{wh00ps_l34k3d_2_mUch}`