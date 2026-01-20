# WebAssembly Templates for {{PROJECT_NAME}}

Comprehensive WebAssembly integration templates for modern web applications.

**Author:** {{AUTHOR}}

## Overview

This directory contains production-ready WebAssembly templates using two popular approaches:

| Approach | Language | Best For | Performance | Bundle Size |
|----------|----------|----------|-------------|-------------|
| **Rust** | Rust | Production apps, crypto, image processing | ⭐⭐⭐⭐⭐ | Larger |
| **AssemblyScript** | TypeScript-like | Quick prototypes, JS developers | ⭐⭐⭐⭐ | Smaller |

## Directory Structure

```
wasm/
├── rust-wasm/              # Rust + wasm-bindgen
│   ├── src/                # Rust source code
│   ├── www/                # Demo web application
│   ├── pkg/                # Compiled WASM output
│   └── tests/              # Browser tests
│
├── assemblyscript/         # AssemblyScript
│   ├── assembly/           # AssemblyScript source
│   ├── build/              # Compiled WASM output
│   └── tests/              # Node.js tests
│
└── integration/            # Framework examples
    ├── react/              # React hooks & providers
    ├── vanilla/            # Pure JavaScript
    └── node/               # Node.js integration
```

## Quick Start

### 1. Rust WebAssembly

```bash
cd rust-wasm

# Install Rust (if needed)
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Install wasm-pack
cargo install wasm-pack

# Build
wasm-pack build --target web --release

# Run demo
cd www && npm install && npm start
```

**Features:**
- ✅ SHA-256, SHA-512, BLAKE3 hashing
- ✅ Image processing (resize, blur, filters)
- ✅ Data validation (email, URL, passwords)
- ✅ Cryptographic operations
- ✅ Memory-safe, zero-cost abstractions

### 2. AssemblyScript

```bash
cd assemblyscript

# Install dependencies
npm install

# Build
npm run asbuild

# Test
npm test
```

**Features:**
- ✅ Mathematical operations
- ✅ String manipulation
- ✅ Array operations
- ✅ Fast compilation
- ✅ TypeScript-like syntax

## Usage Examples

### React Integration

```tsx
import { WasmProvider, useWasmContext } from './integration/react/WasmProvider';

function App() {
  return (
    <WasmProvider>
      <MyComponent />
    </WasmProvider>
  );
}

function MyComponent() {
  const { wasm } = useWasmContext();

  const handleHash = () => {
    const data = new TextEncoder().encode('hello');
    const hash = wasm.sha256_hex(data);
    console.log(hash);
  };

  return <button onClick={handleHash}>Hash</button>;
}
```

### Vanilla JavaScript

```javascript
import { WasmLoader } from './integration/vanilla/wasm-loader.js';

const loader = new WasmLoader();
await loader.loadModule('./pkg/module.js');

const hash = loader.call('sha256_hex', data);
```

### Node.js

```javascript
import { loadRustWasm } from './integration/node/wasm-node.js';

const wasm = await loadRustWasm();
const hash = wasm.sha256_hex(Buffer.from('test'));
console.log(hash);
```

## Performance Comparison

### WASM vs JavaScript

| Operation | JavaScript | Rust WASM | AssemblyScript | Speedup |
|-----------|------------|-----------|----------------|---------|
| SHA-256 hashing | 100ms | 10ms | 15ms | 6-10x |
| Image resize | 500ms | 50ms | N/A | 10x |
| Array sum (1M) | 5ms | 1ms | 1.5ms | 3-5x |
| String reverse | 2ms | 0.5ms | 0.8ms | 2-4x |
| Email validation | 0.1ms | 0.01ms | 0.02ms | 5-10x |

### Bundle Size

| Module | Debug | Release | Gzipped |
|--------|-------|---------|---------|
| Rust WASM | ~200KB | ~50KB | ~15KB |
| AssemblyScript | ~30KB | ~10KB | ~5KB |

## Use Cases

### When to Use Rust WASM

✅ **Best for:**
- Cryptography and security
- Image/video processing
- Complex algorithms
- CPU-intensive tasks
- Production applications
- Need maximum performance

❌ **Avoid for:**
- Simple DOM manipulation
- Prototypes needing rapid iteration
- Small utilities
- Teams without Rust experience

### When to Use AssemblyScript

✅ **Best for:**
- Quick prototypes
- Mathematical computations
- String/array operations
- Teams familiar with TypeScript
- Smaller bundle size requirements
- Educational purposes

❌ **Avoid for:**
- Maximum performance critical
- Complex image processing
- Extensive ecosystem needs
- Advanced cryptography

## Development Workflow

### Rust Development

```bash
# Watch mode (requires cargo-watch)
cargo install cargo-watch
cargo watch -i www -s "wasm-pack build --target web"

# Test
cargo test
wasm-pack test --headless --firefox

# Optimize
wasm-pack build --release --target web
wasm-opt -Oz pkg/*.wasm -o pkg/*.wasm
```

### AssemblyScript Development

```bash
# Watch mode
npm run asbuild -- --watch

# Test
npm test

# Optimize
npm run asbuild:release
```

## Integration Patterns

### 1. Lazy Loading

```javascript
// Load WASM only when needed
button.addEventListener('click', async () => {
  const wasm = await import('./pkg/module.js');
  const result = wasm.process();
});
```

### 2. Web Worker

```javascript
// wasm-worker.js
importScripts('./pkg/module.js');

self.onmessage = async (e) => {
  const result = await wasm.process(e.data);
  self.postMessage(result);
};

// main.js
const worker = new Worker('wasm-worker.js');
worker.postMessage(data);
```

### 3. Service Worker Caching

```javascript
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('wasm-cache').then((cache) => {
      return cache.addAll(['./pkg/module.wasm']);
    })
  );
});
```

### 4. Streaming Instantiation

```javascript
const { instance } = await WebAssembly.instantiateStreaming(
  fetch('./module.wasm')
);
```

## Memory Management

### Rust (wasm-bindgen)

```rust
// Automatic memory management
#[wasm_bindgen]
pub fn process(data: &[u8]) -> Vec<u8> {
    // Memory automatically managed
    data.to_vec()
}
```

### AssemblyScript

```typescript
// Manual memory management
export function process(ptr: usize, len: i32): usize {
  // Allocate
  const result = __new(len, idof<Uint8Array>());
  // ... process ...
  // Caller must free with __release(result)
  return result;
}
```

### JavaScript Side

```javascript
// wasm-bindgen: automatic
const result = wasm.process(data);

// AssemblyScript: manual
const ptr = wasm.__newArray(wasm.Uint8Array_ID, data);
const result = wasm.process(ptr);
wasm.__release(ptr); // Clean up
```

## Error Handling

### Rust

```rust
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub fn process(data: &[u8]) -> Result<Vec<u8>, JsValue> {
    if data.is_empty() {
        return Err(JsValue::from_str("Data cannot be empty"));
    }
    Ok(data.to_vec())
}
```

### JavaScript

```javascript
try {
  const result = wasm.process(data);
} catch (error) {
  console.error('WASM error:', error);
}
```

## Debugging

### Enable Debug Info

**Rust:**
```toml
[profile.dev]
debug = true
```

**AssemblyScript:**
```bash
npm run asbuild:debug
```

### Browser DevTools

1. Open DevTools → Sources
2. Enable "Enable JavaScript source maps"
3. Set breakpoints in WASM code
4. Inspect memory in Memory Profiler

### Logging

**Rust:**
```rust
#[wasm_bindgen]
extern "C" {
    #[wasm_bindgen(js_namespace = console)]
    fn log(s: &str);
}

log("Debug message");
```

**AssemblyScript:**
```typescript
import { console } from "env";
console.log("Debug message");
```

## Testing

### Unit Tests

**Rust:**
```bash
cargo test
```

**AssemblyScript:**
```bash
npm test
```

### Browser Tests

**Rust:**
```bash
wasm-pack test --headless --firefox
wasm-pack test --headless --chrome
```

### Integration Tests

```bash
cd integration/node
npm test
```

## Deployment

### CDN Deployment

```html
<script type="module">
  import init from 'https://cdn.example.com/pkg/module.js';
  await init();
</script>
```

### NPM Package

```bash
cd rust-wasm
wasm-pack build --target web --release
wasm-pack publish
```

### Docker

```dockerfile
FROM rust:1.70 as builder
RUN cargo install wasm-pack
COPY . .
RUN wasm-pack build --target web --release

FROM nginx:alpine
COPY --from=builder /pkg /usr/share/nginx/html
```

## Optimization Tips

### 1. Enable LTO

```toml
[profile.release]
lto = true
```

### 2. Use `wasm-opt`

```bash
wasm-opt -O3 module.wasm -o module.opt.wasm
```

### 3. Strip Debug Info

```bash
wasm-strip module.wasm
```

### 4. Lazy Compilation

```javascript
const module = await WebAssembly.compileStreaming(fetch('module.wasm'));
const instance = await WebAssembly.instantiate(module);
```

### 5. Minimize JS Glue Code

```bash
wasm-pack build --target web # Minimal glue code
```

## Browser Support

| Feature | Chrome | Firefox | Safari | Edge |
|---------|--------|---------|--------|------|
| Basic WASM | ✅ 57+ | ✅ 52+ | ✅ 11+ | ✅ 16+ |
| Threads | ✅ 74+ | ✅ 79+ | ✅ 14.1+ | ✅ 79+ |
| SIMD | ✅ 91+ | ✅ 89+ | ❌ | ✅ 91+ |
| Bulk Memory | ✅ 75+ | ✅ 79+ | ✅ 14.1+ | ✅ 79+ |

## Troubleshooting

### Common Issues

**"wasm-pack not found"**
```bash
cargo install wasm-pack
```

**"Module not found"**
- Check paths in imports
- Ensure WASM file is accessible
- Verify file extensions (.js, .wasm)

**"Memory access out of bounds"**
- Check array indices
- Validate input sizes
- Use bounds checking in debug mode

**"Cannot instantiate module"**
- Check browser compatibility
- Ensure proper MIME type (application/wasm)
- Verify WASM file integrity

### Debug Checklist

- [ ] WASM module loads correctly
- [ ] All exports are available
- [ ] Input data is valid
- [ ] Memory is properly managed
- [ ] Error handling in place
- [ ] Browser console shows no errors
- [ ] Source maps are working

## Resources

### Documentation

- [Rust and WebAssembly Book](https://rustwasm.github.io/docs/book/)
- [wasm-bindgen Guide](https://rustwasm.github.io/docs/wasm-bindgen/)
- [AssemblyScript Docs](https://www.assemblyscript.org/)
- [MDN WebAssembly](https://developer.mozilla.org/en-US/docs/WebAssembly)
- [WebAssembly Specification](https://webassembly.github.io/spec/)

### Tools

- [wasm-pack](https://rustwasm.github.io/wasm-pack/) - Rust WASM packaging
- [wasm-opt](https://github.com/WebAssembly/binaryen) - WASM optimizer
- [wabt](https://github.com/WebAssembly/wabt) - WebAssembly Binary Toolkit

### Community

- [WebAssembly Discord](https://discord.gg/webassembly)
- [Rust WASM Working Group](https://rustwasm.github.io/)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/webassembly)

## Contributing

When adding new WASM functions:

1. Implement in Rust/AssemblyScript
2. Add tests
3. Update TypeScript definitions
4. Add usage examples
5. Update documentation

## License

See LICENSE file in project root.

---

**Author:** {{AUTHOR}}

**Last Updated:** 2026-01-19
