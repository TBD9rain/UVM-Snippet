# UVM-Snippet

A [UltiSnips](https://github.com/SirVer/ultisnips) snippet library for building
**UVM (Universal Verification Methodology)** testbenches in SystemVerilog, plus
companion snippets for QuestaSim simulation/report scripts and a Makefile-driven
simulation flow.

The snippets expand into ready-to-fill skeletons for every layer of a UVM
environment — transactions, sequences, drivers, monitors, agents, scoreboards,
reference models, coverage collectors, environments and tests — so a complete,
consistently-structured testbench can be scaffolded in minutes.

## Features

- **Full UVM component set** — one snippet per verification component, all
  following a single naming and structure convention.
- **Building-block macros** — TLM-1 ports/exports/imps, `uvm_config_db` get/set,
  factory `create`, field-automation macros and `uvm_info/warning/error/fatal`
  reporting.
- **General SystemVerilog helpers** — class/interface skeletons, `rand`/`randc`
  variables, `mailbox`, `fork`/`for`/`assert`, `function`/`task`, functional
  coverage (`covergroup`/`coverpoint`/`cross`/`bins`) and `bind`.
- **Simulation flow** — QuestaSim UVM simulation and report `tcl` scripts and a
  QuestaSim 2026.2 Makefile (Windows) with a Lattice PMI (Diamond/Radiant)
  source-library hook for compiling PMI-based designs.

## Requirements

- [Vim](https://www.vim.org/) (or Neovim) with the
  [UltiSnips](https://github.com/SirVer/ultisnips) plugin installed.
- A completion/trigger key configured for UltiSnips (see `:help UltiSnips`).

## Installation

UltiSnips loads snippet files from directories listed in
`g:UltiSnipsSnippetDirectories`. Point it at (or symlink/copy this repository
into) one of those directories. For example:

```vim
" Load snippets from a custom directory
let g:UltiSnipsSnippetDirectories = ['UltiSnips', 'path/to/UVM-Snippet']
```

Snippet files are grouped by filetype directory (`systemverilog/`, `tcl/`) and
top-level Makefile snippets (`make.snippets`), matching UltiSnips' per-filetype
loading. The SystemVerilog snippets `extends verilog`, so any Verilog snippets in
your setup remain available.

## Repository Structure

```
UVM-Snippet/
├── systemverilog/      SystemVerilog / UVM snippets (one file per component)
├── tcl/                QuestaSim simulation & report script snippets
└── make.snippets       QuestaSim UVM simulation Makefile
```

### SystemVerilog snippets (`systemverilog/`)

| File                   | Trigger(s)                         | Provides |
| ---------------------- | ---------------------------------- | -------- |
| `basic.snippets`       | `class`, `port`, `imp`, `config`, `field`, `msg`, `function`, `covergroup`, `bind`, … | UVM class/object skeletons, TLM-1 communication, `uvm_config_db`, factory create, field-automation macros, message macros, and general SystemVerilog constructs |
| `interface.snippets`   | `interface`                        | Interface with clocking block and driver/monitor/DUT modports |
| `package.snippets`     | `package` / `pkg`                  | UVM component package with ordered component includes |
| `transaction.snippets` | `Txn` / `Transaction`, `do_`       | Transaction item plus `do_copy`/`do_compare`/`convert2string`/`do_print` reloads |
| `sequence.snippets`    | `Seq` / `Sequence`                 | UVM sequence and virtual sequence |
| `sequencer.snippets`   | `Sqr` / `Sequencer`                | UVM sequencer and virtual sequencer |
| `driver.snippets`      | `Drv` / `Driver`                   | UVM driver |
| `monitor.snippets`     | `Mon` / `Monitor`                  | UVM monitor |
| `agent.snippets`       | `Agt` / `Agent`                    | UVM agent |
| `model.snippets`       | `Mdl` / `RefMdl` / `Model`         | UVM reference model |
| `scoreboard.snippets`  | `Scb` / `Scoreboard`               | UVM scoreboard |
| `faultinjector.snippets` | `FI` / `FaultInjector`           | Fault injector component |
| `coverage.snippets`    | `Cov` / `CoverageCollector`        | UVM coverage collector |
| `config.snippets`      | `Cfg` / `Config`                   | UVM configuration object |
| `environment.snippets` | `Env` / `Environment`              | UVM environment and integrated environment |
| `test.snippets`        | `Test`                             | UVM test, extendable base test, and scoreboard-verification test |
| `testbench.snippets`   | `testbench`                        | Top-level UVM testbench module |

### Simulation snippets (`tcl/`, `make.snippets`)

| File                          | Trigger      | Provides |
| ----------------------------- | ------------ | -------- |
| `tcl/questa_2024.2_sim.snippets` | `simulate` | QuestaSim 2024.2.1 UVM simulation script |
| `tcl/questa_10.6c_sim.snippets`  | `simulate` | QuestaSim 10.6c UVM simulation script |
| `tcl/questa_rpt.snippets`        | `report`   | QuestaSim UVM report script |
| `make.snippets`                  | `makefile` | QuestaSim 2026.2 UVM simulation Makefile (Windows), with a Lattice PMI (Diamond/Radiant) source-library hook |

## Component Naming Convention

The component snippets and the `package` snippet share one short-name convention,
so a generated package wires the pieces together out of the box:

| Short name | Component            |
| ---------- | -------------------- |
| `Config`   | Configuration object |
| `Txn`      | Transaction          |
| `Sqr`      | Sequencer            |
| `Drv`      | Driver               |
| `Mon`      | Monitor              |
| `Agt`      | Agent                |
| `RefMdl`   | Reference model      |
| `Scb`      | Scoreboard           |
| `ScbFI`    | Scoreboard fault injector |
| `Cov`      | Coverage collector   |
| `Env`      | Environment          |
| `Seq`      | Sequence             |
| `Test`     | Test                 |

## Usage

1. Open a SystemVerilog source file (or a `tcl`/Makefile for the simulation
   snippets).
2. Type a snippet trigger (for example `Drv` for a driver, or `Txn` for a
   transaction) and expand it with your UltiSnips trigger key.
3. Tab through the placeholders to fill in names, parameters and body.

A typical bottom-up flow: scaffold the `interface`, then `Txn` → `Sqr`/`Seq` →
`Drv`/`Mon` → `Agt` → `RefMdl`/`Scb`/`Cov` → `Env` → `Test`, collect them with the
`package` snippet, wrap everything in a `testbench` top module, and drive it with
the QuestaSim `simulate`/`report` scripts or the generated Makefile.

## Author

TBD9rain

---

*This repository is a Vim/UltiSnips snippet collection; it does not itself compile
or simulate any RTL. The generated code targets the UVM class library and, for the
simulation flow, Siemens QuestaSim.*
