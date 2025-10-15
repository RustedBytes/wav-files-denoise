# wav-files-denoise

A Rust CLI tool for recursively denoising WAV audio files using the nnnoiseless noise reduction algorithm. It processes folders (including subfolders), validates files to ensure they match the expected format (WAVE, Microsoft PCM, 16-bit, mono, 16kHz), and outputs denoised versions to a specified directory while preserving the original folder structure.

## Features

- **Recursive Processing**: Scans input directories and all subdirectories for `.wav` files.
- **Format Validation**: Uses the `hound` crate to verify files are mono, 16-bit PCM at 16kHz sample rate; skips invalid files.
- **Noise Reduction**: Integrates with [nnnoiseless](https://github.com/Rikorose/DeepFilterNet) for high-quality denoising.
- **Preserved Structure**: Outputs maintain the relative path structure from the input.
- **Flexible Configuration**: Optional paths for nnnoiseless executable and custom model files.
- **Error Handling**: Robust logging for skipped files and failures.

## Installation

### From Source

1. Clone the repository:
   ```bash
   git clone https://github.com/RustedBytes/wav-files-denoise.git
   cd wav-files-denoise
   ```

2. Build and install:
   ```bash
   cargo install --path .
   ```

Ensure you have [nnnoiseless](https://github.com/Rikorose/DeepFilterNet) installed and available in your PATH (or specify via `--nnnoiseless-path`).

## Usage

```bash
wav-files-denoise [OPTIONS] <INPUT_DIR> <OUTPUT_DIR>
```

### Arguments

- `<INPUT_DIR>`: The input directory containing WAV files (processed recursively).
- `<OUTPUT_DIR>`: The output directory for denoised files.

### Options

- `--nnnoiseless-path <PATH>`: Path to the nnnoiseless executable (defaults to `nnnoiseless` in PATH).
- `--model-path <PATH>`: Path to a custom model file (optional; uses nnnoiseless built-in model if omitted).
- `-h, --help`: Print help information.
- `-V, --version`: Print version information.

### Denoising Command

The tool executes: `nnnoiseless --model=<model> <input.wav> <output.wav>` (model flag omitted if not provided).

## Examples

### Basic Usage

Denoise all valid WAV files in `input/` and save to `output/`:

```bash
wav-files-denoise input/ output/
```

### With Custom Paths

```bash
wav-files-denoise --nnnoiseless-path /usr/local/bin/nnnoiseless --model-path ./weights.rnn input/ output/
```

### Output

The tool prints progress:
```
Denoising complete: 42 files processed, 3 skipped.
```
Invalid files are logged to stderr, e.g.:
```
Skipping invalid WAV file: input/subdir/invalid.wav
```

## Development

This project uses Rust 2021 edition and follows idiomatic conventions. Run tests with:

```bash
cargo test
```

Build with:

```bash
cargo build --release
```

### Dependencies

- `clap`: CLI argument parsing.
- `hound`: WAV file validation.
- `walkdir`: Recursive directory traversal.
- `anyhow`: Error handling.

See `Cargo.toml` for full details.

## Contributing

Contributions are welcome! Please:

1. Fork the repo and create a feature branch.
2. Add tests for new features.
3. Ensure code passes `cargo fmt` and `cargo clippy`.
4. Open a Pull Request.

Report issues via [GitHub Issues](https://github.com/RustedBytes/wav-files-denoise/issues).

## License

MIT License. See [LICENSE](LICENSE) for details.
