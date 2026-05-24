# UVM RAL + Coverage Closure Demo

> A complete UVM testbench demonstrating **Register Abstraction Layer (RAL)** modeling, frontdoor/backdoor register access, and **functional coverage closure** on a synthetic DUT.

---

## Overview

This project implements a production-grade UVM environment targeting a synthesizable APB peripheral with 16 mapped registers. It demonstrates industry-standard RAL usage patterns and shows coverage closure methodology from initial run to 99%+ convergence.

**Key concepts demonstrated:**
- RAL model generated from IP-XACT / custom Python script
- Frontdoor access via APB UVC · Backdoor access via DPI-C
- `uvm_reg_hw_reset_seq` + `uvm_reg_bit_bash_seq` built-ins
- Custom register sequences for field-level constrained writes
- Functional coverage: per-register, per-field, cross-coverage bins
- Exclusion management with waivers

---

## Repository Structure

```
uvm-ral-coverage/
├── rtl/
│   └── apb_regs.sv           # Synthesizable DUT (APB register bank)
├── tb/
│   ├── env/
│   │   ├── apb_uvc/          # APB agent (driver, monitor, sequencer)
│   │   ├── reg_model/        # RAL model (.sv) + predictor
│   │   └── coverage/         # Functional covergroups
│   ├── sequences/
│   │   ├── reg_reset_seq.sv
│   │   ├── reg_stress_seq.sv
│   │   └── reg_corner_seq.sv
│   └── top/
│       └── tb_top.sv
├── scripts/
│   ├── run_sim.sh
│   └── gen_ral.py            # RAL model generator from CSV spec
├── docs/
│   └── coverage_closure_report.md
└── Makefile
```

---

## DUT: APB Register Bank

16-register peripheral with mix of RW, RO, W1C, and reserved fields. Includes:
- Control/status registers
- Interrupt enable / status (W1C)
- Configuration fields with reset values

---

## Coverage Model

| Covergroup | Bins | Target |
|---|---|---|
| `cg_reg_access` | Per-register read/write | 100% |
| `cg_field_values` | Corner bins per field width | 95% |
| `cg_interrupt_flow` | Set → mask → clear sequence | 100% |
| `cg_reset_check` | Reset value per register | 100% |

---

## How to Run

```bash
# Prerequisites: Questa or VCS, UVM 1.2
make compile
make sim SEED=1
make sim SEED=random RUNS=50   # regression
make coverage                  # merge + report
```

---

## Skills Demonstrated

`UVM` `RAL` `APB` `Constrained Random` `Functional Coverage` `Python` `DPI-C` `Coverage Closure`
