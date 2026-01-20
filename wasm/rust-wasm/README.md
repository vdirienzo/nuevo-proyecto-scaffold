# {{PROJECT_NAME}} - Rust WebAssembly Module

High-performance WebAssembly module built with Rust for cryptography, image processing, and data validation.

**Author:** {{AUTHOR}}

## Features

- **Cryptography**
  - SHA-256, SHA-512, BLAKE3 hashing
  - Password hashing with salts
  - XOR cipher (demonstration)
  - Fingerprint generation

- **Image Processing**
  - Resize with aspect ratio preservation
  - Grayscale conversion
  - Blur effects
  - Brightness/contrast adjustment
  - Rotation and flipping
  - Thumbnail generation

- **Data Validation**
  - Email validation
  - URL validation
  - Phone number validation
  - Credit card validation (Luhn algorithm)
  - Password strength checking
  - IPv4 address validation
  - Hex color validation
  - JSON validation

- **Utilities**
  - Hex encoding/decoding
  - Random byte generation
  - Base64 encoding
  - HTML sanitization

## Prerequisites

- [Rust](https://www.rust-lang.org/tools/install) 1.70+
- [wasm-pack](https://rustwasm.github.io/wasm-pack/installer/)
- [Node.js](https://nodejs.org/) 18+ (for web demo)

```bash
# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install wasm-pack
curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh
```

## Quick Start

### 1. Build the WASM Module

```bash
# Development build
wasm-pack build --target web

# Production build (optimized)
wasm-pack build --target web --release
```

This generates the `pkg/` directory with:
- `*.wasm` - Compiled WebAssembly binary
- `*.js` - JavaScript bindings
- `*.d.ts` - TypeScript definitions

### 2. Run the Demo Web App

```bash
cd www
npm install
npm start
```

Opens http://localhost:8080 with interactive demos.

## Usage Examples

### Browser (Vanilla JS)

```html
<script type="module">
  import init, * as wasm from './pkg/{{PROJECT_NAME}}_wasm.js';

  await init();

  // Hashing
  const encoder = new TextEncoder();
  const data = encoder.encode('hello world');
  const hash = wasm.sha256_hex(data);
  console.log('SHA-256:', hash);

  // Validation
  console.log(wasm.validate_email('test@example.com')); // true
  console.log(wasm.password_strength('MyP@ssw0rd')); // 3

  // Image processing
  const imageBytes = await fetch('image.jpg')
    .then(r => r.arrayBuffer())
    .then(b => new Uint8Array(b));

  const grayscale = wasm.grayscale(imageBytes);
  const blob = new Blob([grayscale], { type: 'image/png' });
</script>
```

### Node.js

```javascript
const wasm = require('./pkg/{{PROJECT_NAME}}_wasm.js');

wasm.init();

const hash = wasm.sha256_hex(Buffer.from('test'));
console.log(hash);
```

### React/Next.js

See `../../integration/react/` for React hooks and components.

## API Reference

### Cryptography

```rust
// Hashing
fn sha256(data: &[u8]) -> Vec<u8>
fn sha256_hex(data: &[u8]) -> String
fn sha512(data: &[u8]) -> Vec<u8>
fn sha512_hex(data: &[u8]) -> String
fn blake3_hash(data: &[u8]) -> Vec<u8>
fn blake3_hash_hex(data: &[u8]) -> String

// Password hashing
fn hash_password(password: &str, salt: &[u8], iterations: u32) -> Result<Vec<u8>>

// Utilities
fn xor_cipher(data: &[u8], key: &[u8]) -> Result<Vec<u8>>
fn fingerprint(data: &[u8]) -> String
```

### Image Processing

```rust
fn resize_image(data: &[u8], width: u32, height: u32, preserve_aspect: bool) -> Result<Vec<u8>>
fn grayscale(data: &[u8]) -> Result<Vec<u8>>
fn blur_image(data: &[u8], sigma: f32) -> Result<Vec<u8>>
fn adjust_brightness(data: &[u8], value: i32) -> Result<Vec<u8>>
fn adjust_contrast(data: &[u8], contrast: f32) -> Result<Vec<u8>>
fn rotate_image(data: &[u8], degrees: u32) -> Result<Vec<u8>>
fn flip_horizontal(data: &[u8]) -> Result<Vec<u8>>
fn flip_vertical(data: &[u8]) -> Result<Vec<u8>>
fn create_thumbnail(data: &[u8], max_size: u32) -> Result<Vec<u8>>
fn get_dimensions(data: &[u8]) -> Result<JsValue>
```

### Validation

```rust
fn validate_email(email: &str) -> bool
fn validate_url(url: &str) -> bool
fn validate_phone(phone: &str) -> bool
fn validate_credit_card(number: &str) -> bool
fn validate_username(username: &str) -> bool
fn validate_ipv4(ip: &str) -> bool
fn validate_hex_color(color: &str) -> bool
fn validate_json(json_str: &str) -> bool
fn password_strength(password: &str) -> u8
fn sanitize_html(html: &str) -> String
```

### Utilities

```rust
fn bytes_to_hex(bytes: &[u8]) -> String
fn hex_to_bytes(hex: &str) -> Result<Vec<u8>>
fn random_bytes(length: usize) -> Result<Vec<u8>>
fn is_ascii(text: &str) -> bool
```

## Testing

```bash
# Run Rust unit tests
cargo test

# Run browser-based WASM tests
wasm-pack test --headless --firefox
wasm-pack test --headless --chrome
```

## Performance

WASM provides near-native performance in the browser:

- **SHA-256**: ~500,000 ops/sec (1KB input)
- **Email validation**: ~2,000,000 ops/sec
- **Image resize**: ~50ms for 1920x1080 → 800x600

Run the performance benchmark in the demo app for your device's metrics.

## Memory Management

WebAssembly has linear memory shared with JavaScript:

```javascript
// Memory is automatically managed by wasm-bindgen
// No manual cleanup needed for most operations

// For large data, consider:
const result = wasm.process_large_data(bigData);
// Use result...
// JavaScript GC will clean up when result goes out of scope
```

## Build Optimization

### Size Optimization

```toml
# Cargo.toml
[profile.release]
opt-level = "s"  # Optimize for size
lto = true       # Link-time optimization
```

```bash
# Further compression
wasm-pack build --release --target web
wasm-opt -Os pkg/*.wasm -o pkg/*.wasm
```

### Performance Optimization

```toml
[profile.release]
opt-level = 3    # Maximum optimization
lto = true
codegen-units = 1
```

## Troubleshooting

### Build Errors

**Error:** `wasm-pack: command not found`
```bash
cargo install wasm-pack
```

**Error:** `rustc version too old`
```bash
rustup update
```

### Runtime Errors

**Error:** `TypeError: Cannot read property 'buffer' of undefined`
- Ensure `init()` is called before using WASM functions
- Check that WASM file is loaded correctly

**Error:** `Memory access out of bounds`
- Validate input data sizes
- Check for buffer overflows
- Use `.slice()` to copy data if needed

## Deployment

### NPM Package

```bash
cd pkg
npm publish
```

### CDN (unpkg)

```html
<script type="module">
  import init from 'https://unpkg.com/{{PROJECT_NAME}}-wasm/{{PROJECT_NAME}}_wasm.js';
  await init();
</script>
```

## Resources

- [Rust and WebAssembly Book](https://rustwasm.github.io/docs/book/)
- [wasm-bindgen Guide](https://rustwasm.github.io/docs/wasm-bindgen/)
- [MDN WebAssembly Docs](https://developer.mozilla.org/en-US/docs/WebAssembly)

## License

See LICENSE file in project root.

---

**Author:** {{AUTHOR}}
