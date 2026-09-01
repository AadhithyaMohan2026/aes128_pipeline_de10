# Pipelined AES-128 Implementation on DE10-Lite

A fully synchronous, 10-stage fully pipelined AES-128 encryption core implemented in SystemVerilog for the Intel MAX 10 FPGA (DE10-Lite board). 

The pipeline encrypts continuous streaming 128-bit blocks with an initial latency of 10 clock cycles, achieving a sustained throughput of **1 block (128 bits) per clock cycle** once filled.

---

## Architectural Features

* **10-Stage Pipelined Architecture:** Dedicated hardware round registers between consecutive transformation stages allow maximum clock frequency and high throughput without multicycle stall overhead.
* **Full AES Transformations:**
  * **SubBytes:** 16-way parallel S-Box substitution lookup tables.
  * **ShiftRows:** Cyclic byte shifting per 4x4 state matrix specification.
  * **MixColumns:** $GF(2^8)$ matrix multiplication performed on rounds 1 through 9.
  * **AddRoundKey:** Bitwise 128-bit XOR stage driven by parallel key schedule expansion.
* **Tag & Valid Bit Propagation:** Block indices and valid handshake flags propagate synchronously alongside the datapath to ensure deterministic ordering.
* **Hardware On-Board Verification:** Integrated with on-chip RAM/MIF block feeding, control switches/buttons, and 7-segment display status decoders.

---
https://github.com/user-attachments/assets/8bdd1442-94a1-4b0e-a7c5-2b9405bb3e7a

## Project Directory Structure

```text
├── rtl/
│   ├── aes_sbox.sv            # 256-entry forward substitution lookup table
│   ├── aes_transforms.sv      # SubBytes, ShiftRows, MixColumns transformations
│   ├── aes_key_expansion.sv   # 10-round key expansion generator
│   ├── aes_pipe_round.sv      # Registered single pipeline round stage
│   ├── aes128_pipeline.sv     # 10-stage chained pipeline core
│   ├── aes_text_feeder.sv     # Input block framing & streaming control
│   ├── seg7_decoder.sv        # Seven-segment display hex decoder
│   └── de10_aes_top.sv        # Top-level DE10-Lite FPGA wrapper
├── constraints/
│   └── constraints.sdc        # 50 MHz clock & timing constraints
├── plaintext.mif              # Pre-loaded plaintext block vectors
├── de10_aes_top.qsf           # Quartus pin assignments & device settings
├── de10_aes_top.qpf           # Quartus project definition
└── README.md






