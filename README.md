
# textpatch

A Go library and CLI tool for applying line-based text patches described in the [Codex Patch Format](doc/codex_patch_format.md).

## Features

- Apply insert, delete, and replace operations to files or in-memory strings/lines.
- Use as a standalone CLI or as a Go package from your code.

---

## Codex Patch Format

The patch is a JSON array of operations. Each operation is an object:
- `line`: 0-based line number to operate on
- `type`: `"insert"`, `"delete"`, or `"replace"`
- `content`: `[string]` (required for insert/replace)

See [doc/codex_patch_format.md](doc/codex_patch_format.md) for full specification and examples.

---

## Usage as Go Module

Import the package:
```go
import "textpatch"
```

### Functions

- `PatchLines(inputLines []string, patchJson string) ([]string, error)`
- `PatchText(input string, patchJson string) (string, error)`
- `PatchFile(inputFile string, patchJson string, outputFile ...string) error`

#### Example

```go
import "textpatch"

lines := []string{"a", "b", "c"}
patch := `[{"line":1,"type":"replace","content":["B"]}]`
out, err := textpatch.PatchLines(lines, patch)
// out: []string{"a", "B", "c"}
```

---

## Command Line Tool

### Build

```bash
go build -o textpatch
```

### Usage

```
textpatch -i <inputfile> [-o <outputfile>] -p <patchfile>
```
- `-i <inputfile>`: Path to input text file
- `-o <outputfile>`: Path to output file (optional; if omitted, input is overwritten)
- `-p <patchfile>`: Path to JSON file containing patch operations

Example:

```bash
textpatch -i example.txt -p patch.json
textpatch -i input.txt -o output.txt -p patch.json
```

---

## Testing

Unit tests are located in [`textpatch/patch_test.go`](textpatch/patch_test.go).

Run tests via:

```bash
go test ./textpatch
```
or for verbose output:
```bash
go test -v ./textpatch
```

---

## References

- [Codex Patch Format Spec](doc/codex_patch_format.md)

## License

This project is licensed under the [European Union Public Licence v1.2 (EUPL)](https://joinup.ec.europa.eu/collection/eupl/eupl-text-eupl-12).
