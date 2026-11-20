<p align="center">
  <a href="https://www.rust-lang.org/">
    <img src="https://www.rust-lang.org/logos/rust-logo-512x512.png" alt="Rust programming language logo" width="112" />
  </a>
</p>

<h1 align="center">Ferrispector</h1>

<p align="center">
  <strong>Rust-powered pattern inspection for malware research and security analysis.</strong>
</p>

<p align="center">
  <a href="https://github.com/CyberStill-GmbH/ferrispector/actions/workflows/tests.yaml"><img src="https://github.com/CyberStill-GmbH/ferrispector/actions/workflows/tests.yaml/badge.svg?branch=main" alt="Tests" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-BSD--3--Clause-blue.svg" alt="BSD-3-Clause License" /></a>
  <img src="https://img.shields.io/badge/Rust-1.95%2B-CE422B?logo=rust&logoColor=white" alt="Rust 1.95 or newer" />
  <a href="https://github.com/VirusTotal/yara-x"><img src="https://img.shields.io/badge/based%20on-YARA--X-informational" alt="Based on YARA-X" /></a>
</p>

---

## Overview

**Ferrispector** is a Rust-based pattern inspection project focused on malware research, digital forensics, and security engineering. It is an **independent fork of [YARA-X](https://github.com/VirusTotal/yara-x)**, the pattern-matching engine developed by VirusTotal.

Ferrispector provides a foundation for describing and identifying files using textual patterns, hexadecimal signatures, regular expressions, and expressive rule conditions. The project currently retains the proven YARA-X codebase and tooling while establishing its own identity and direction for future development.

> **Project status:** Ferrispector is an early-stage fork. Its underlying packages, command-line executable (`yr`), and portions of its documentation still use YARA-X naming. Ferrispector is not a separately published crate or a new detection engine at this stage.

## Capabilities

The inherited YARA-X codebase provides:

- **Flexible pattern matching** — text strings, binary signatures, regular expressions, and logical conditions.
- **Rule-based inspection** — reusable signatures with metadata, tags, and expressive matching logic.
- **Command-line tooling** — scan targets, compile rules, check syntax, and format rules.
- **Rust-first implementation** — a systems-language foundation designed for performance and memory safety.
- **Integration surfaces** — source components for a Rust library, C API, Python bindings, and WebAssembly.
- **Security workflows** — a foundation for malware triage, file inspection, and detection engineering.

These are capabilities inherited from YARA-X; they should not be interpreted as Ferrispector-specific enhancements.

## Getting started

### Requirements

- **Rust 1.95.0 or newer**, as specified in the workspace manifest
- Cargo and Git
- An environment with the build dependencies required by the selected workspace components

### Build from source

```bash
git clone https://github.com/CyberStill-GmbH/ferrispector.git
cd ferrispector
cargo build --release -p yara-x-cli
```

The current command-line executable retains its upstream name:

```bash
./target/release/yr --help
```

On Windows, run `./target/release/yr.exe` (or `.\target\release\yr.exe` in PowerShell).

### Your first inspection

Save the following as `example.yar`:

```yara
rule ferrispector_demo : inspection {
    meta:
        description = "Basic demonstration rule"
        category = "example"

    strings:
        $marker = "FerrispectorTestString"
        $hex = { 46 65 72 72 69 73 }

    condition:
        $marker or $hex
}
```

Create a sample file and scan it:

```bash
printf 'FerrispectorTestString\n' > sample.txt
./target/release/yr scan example.yar sample.txt
```

A match indicates that the file contains at least one of the patterns defined in the rule. This example demonstrates signature matching, **not** a determination that a file is malicious.

## Repository layout

| Directory | Purpose |
| --- | --- |
| `lib/` | Core pattern-matching library |
| `cli/` | Command-line interface (`yr`) |
| `parser/` | Rule parsing components |
| `capi/` | C integration layer |
| `py/` | Python bindings |
| `js-wasm/` | JavaScript / WebAssembly integration |
| `docs/` | Documentation source |

## Project direction

Ferrispector is intended as a practical base for security-focused inspection workflows. Potential areas of independent development include improved analyst ergonomics, rule-management tooling, and integration with endpoint or SOC detection pipelines.

**These are directions under consideration, not features claimed to be implemented.** Changes and milestones will be tracked in this repository as they are developed.

## Fork author and maintainer

**[César Adrián Guevara Salcedo (@CyberStill-GmbH)](https://github.com/CyberStill-GmbH)** maintains the Ferrispector fork and its independent development direction.

Ferrispector preserves the original YARA-X contributors' attribution. This maintainer credit refers to the fork, not authorship of the inherited upstream engine.

## Documentation and compatibility

Because Ferrispector is currently based on YARA-X, the upstream documentation remains the primary reference for its inherited engine and rule syntax:

- [Writing YARA-X rules](https://virustotal.github.io/yara-x/docs/writing_rules/anatomy-of-a-rule/)
- [YARA-X versus YARA](https://virustotal.github.io/yara-x/docs/intro/yara-x-vs-yara/)
- [Rule-level differences](https://virustotal.github.io/yara-x/docs/writing_rules/differences-with-yara/)

Not all YARA rules are necessarily compatible with YARA-X without adjustments. Refer to the upstream compatibility documentation when migrating rules.

## Contributing

Bug reports, documentation improvements, and development proposals can be submitted through [Ferrispector Issues](https://github.com/CyberStill-GmbH/ferrispector/issues). For source conventions and build checks, see [CONTRIBUTING.md](CONTRIBUTING.md).

**Note:** The current contribution guide is inherited from YARA-X and references upstream contribution and CLA requirements. Those instructions have not yet been adapted into a separate Ferrispector contributor policy.

## Acknowledgements and license

Ferrispector is derived from **[YARA-X](https://github.com/VirusTotal/yara-x)** by VirusTotal and its contributors. We acknowledge their work and preserve the upstream authorship and license notices. Ferrispector is **not affiliated with or endorsed by VirusTotal, Google, or the Rust Project**.

The source code is distributed under the **[BSD 3-Clause License](LICENSE)**, subject to the notices in the repository. See [AUTHORS](AUTHORS) and [CONTRIBUTORS](CONTRIBUTORS) for inherited attribution.

The Rust logo shown above belongs to the Rust Foundation and is used to indicate the implementation language; it is not a Ferrispector product mark. See the [Rust media guide and logo licensing information](https://rust-lang.org/policies/media-guide).
