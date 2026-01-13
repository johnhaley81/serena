# OCaml Setup Guide for Serena

This guide explains how to set up OCaml and Reason language support for Serena via `ocaml-lsp-server`.

---

## Prerequisites

### OCaml Version Requirements

- **OCaml < 5.1 or >= 5.1.1** is required
- **OCaml 5.1.0 is NOT supported** due to incompatibilities with ocaml-lsp-server
- For cross-file references: **OCaml 5.2+** with **ocaml-lsp-server >= 1.23.0**

### Required Software

Install the following on your system:

1. **OPAM** (OCaml Package Manager)
   - macOS: `brew install opam`
   - Ubuntu/Debian: `sudo apt install opam`
   - Fedora: `sudo dnf install opam`
   - Windows: See [OPAM on Windows](https://fdopen.github.io/opam-repository-mingw/installation/)

2. **ocaml-lsp-server**
   - Install via OPAM (see installation steps below)

3. **dune** (build system)
   - Required for project indexing and cross-file references

---

## Installation Steps

### 1. Initialize OPAM (if not already done)

```bash
opam init
eval $(opam env)
```

### 2. Create an OPAM switch with a compatible OCaml version

If you don't have a compatible OCaml version, create a new switch:

```bash
# For basic functionality (recommended for stability)
opam switch create serena-ocaml ocaml-base-compiler.4.14.2

# OR for cross-file references support
opam switch create serena-ocaml ocaml-base-compiler.5.2.0
```

Activate the switch:

```bash
opam switch serena-ocaml
eval $(opam env)
```

### 3. Install ocaml-lsp-server

```bash
opam install ocaml-lsp-server dune
```

For cross-file references support, ensure you have ocaml-lsp-server >= 1.23.0:

```bash
opam list ocaml-lsp-server  # Check version
```

### 4. Verify installation

```bash
opam exec -- ocamllsp --version
opam exec -- ocaml -version
```

---

## Project Setup

### Dune-based Projects

Serena works best with dune-based OCaml projects. Your project should have:

- A `dune-project` file in the root
- `dune` files in each directory containing OCaml code
- An `.opam` file for your package

### Cross-File References

For cross-file references to work (finding usages across multiple files):

1. Ensure you're using OCaml 5.2+ and ocaml-lsp-server >= 1.23.0
2. The index is built automatically via `dune build @ocaml-index` when Serena starts
3. For best results, run `dune build -w` in the background (enables dune RPC)

If cross-file references don't work, try manually building the index:

```bash
opam exec -- dune build @ocaml-index
```

---

## Supported File Types

Serena supports both OCaml and Reason:

- `.ml` - OCaml implementation files
- `.mli` - OCaml interface files
- `.re` - Reason implementation files
- `.rei` - Reason interface files

---

## Troubleshooting

### "OPAM is not installed" error

Ensure OPAM is installed and in your PATH:

```bash
which opam
```

If not found, install OPAM using the instructions above.

### "ocaml-lsp-server is not installed" error

Install it with:

```bash
opam install ocaml-lsp-server
```

### OCaml 5.1.0 incompatibility

OCaml 5.1.0 has known incompatibilities with ocaml-lsp-server. Create a switch with a different version:

```bash
opam switch create myswitch ocaml-base-compiler.4.14.2
opam switch myswitch
eval $(opam env)
opam install ocaml-lsp-server
```

### Cross-file references not working

1. Verify versions:
   ```bash
   opam exec -- ocaml -version   # Should be 5.2.0 or higher
   opam list ocaml-lsp-server    # Should be 1.23.0 or higher
   ```

2. Manually build the index:
   ```bash
   opam exec -- dune build @ocaml-index
   ```

3. Run dune in watch mode for best results:
   ```bash
   opam exec -- dune build -w
   ```

### Environment not activated

Always ensure your OPAM environment is activated:

```bash
eval $(opam env)
```

---

## Reference

- OPAM Documentation: [https://opam.ocaml.org/doc/](https://opam.ocaml.org/doc/)
- ocaml-lsp-server: [https://github.com/ocaml/ocaml-lsp](https://github.com/ocaml/ocaml-lsp)
- Project-wide occurrences: [https://discuss.ocaml.org/t/ann-project-wide-occurrences-in-merlin-and-lsp/14847](https://discuss.ocaml.org/t/ann-project-wide-occurrences-in-merlin-and-lsp/14847)
