# {{PROJECT_NAME}} - AssemblyScript WebAssembly Module

High-performance WebAssembly module written in AssemblyScript (TypeScript-like syntax).

**Author:** {{AUTHOR}}

## Features

- **Mathematics**
  - Basic arithmetic operations
  - Power, square root calculations
  - GCD, LCM algorithms
  - Fibonacci sequence
  - Statistical functions (average, standard deviation)
  - Vector operations (dot product, distance)

- **String Utilities**
  - String reversal and manipulation
  - Palindrome checking
  - Word counting
  - Case conversion
  - String truncation and padding
  - Levenshtein distance

- **Array Operations**
  - Sum, max, min calculations
  - Array reversal
  - Binary search
  - Sorting algorithms
  - Memory-efficient operations

## Prerequisites

- [Node.js](https://nodejs.org/) 18+
- npm or yarn

```bash
npm install
```

## Quick Start

### 1. Build the WASM Module

```bash
# Development build (includes debug info)
npm run asbuild:debug

# Production build (optimized)
npm run asbuild:release

# Build both
npm run asbuild
```

### 2. Run Tests

```bash
npm test
```

### 3. Use in Your Project

```javascript
import { instantiate } from '@assemblyscript/loader';
import { readFile } from 'fs/promises';

// Load WASM
const wasmModule = await instantiate(
  await readFile('./build/release.wasm')
);

const { exports } = wasmModule;

// Basic operations
console.log(exports.add(5, 3)); // 8
console.log(exports.multiply(4, 5)); // 20

// Math operations
console.log(exports.power(2, 10)); // 1024
console.log(exports.sqrt(144)); // 12
console.log(exports.fibonacci(10)); // 55

// String operations
const input = exports.__newString('hello world');
const result = exports.titleCase(input);
console.log(exports.__getString(result)); // "Hello World"

// Array operations
const arr = new Int32Array([1, 2, 3, 4, 5]);
const ptr = exports.__newArray(exports.Int32Array_ID, arr);
console.log(exports.sumArray(ptr)); // 15
```

## Browser Usage

```html
<script type="module">
  import instantiate from './build/release.js';

  const module = await instantiate();

  // Use exported functions
  console.log(module.add(10, 20));
  console.log(module.isPrime(17));

  // Strings
  const text = module.__newString('test');
  const reversed = module.reverseString(text);
  console.log(module.__getString(reversed));
</script>
```

## API Reference

### Basic Functions

```typescript
export function add(a: i32, b: i32): i32
export function multiply(a: i32, b: i32): i32
export function isPrime(n: i32): bool
export function factorial(n: i32): i64
```

### Math Module

```typescript
export function power(x: f64, n: i32): f64
export function sqrt(x: f64): f64
export function abs(x: f64): f64
export function gcd(a: i32, b: i32): i32
export function lcm(a: i32, b: i32): i32
export function fibonacci(n: i32): i64
export function isEven(n: i32): bool
export function isOdd(n: i32): bool
export function clamp(value: f64, min: f64, max: f64): f64
export function lerp(a: f64, b: f64, t: f64): f64
export function distance(x1: f64, y1: f64, x2: f64, y2: f64): f64
export function average(arr: Float64Array): f64
export function standardDeviation(arr: Float64Array): f64
export function dotProduct(a: Float64Array, b: Float64Array): f64
```

### String Module

```typescript
export function reverseString(str: string): string
export function isPalindrome(str: string): bool
export function countChar(str: string, char: string): i32
export function countWords(str: string): i32
export function titleCase(str: string): string
export function repeatString(str: string, times: i32): string
export function truncate(str: string, maxLength: i32): string
export function toLowerCase(str: string): string
export function toUpperCase(str: string): string
export function startsWith(str: string, prefix: string): bool
export function endsWith(str: string, suffix: string): bool
export function contains(str: string, search: string): bool
export function replaceAll(str: string, search: string, replacement: string): string
export function padStart(str: string, targetLength: i32, padChar: string): string
export function padEnd(str: string, targetLength: i32, padChar: string): string
export function levenshteinDistance(a: string, b: string): i32
```

### Array Operations

```typescript
export function sumArray(arr: Int32Array): i32
export function maxArray(arr: Int32Array): i32
export function reverseArray(arr: Int32Array): void
export function binarySearch(arr: Int32Array, target: i32): i32
export function bubbleSort(arr: Int32Array): void
```

## Memory Management

AssemblyScript manages memory differently from JavaScript:

```javascript
// Creating strings
const strPtr = exports.__newString("hello");
const strValue = exports.__getString(strPtr);

// Creating arrays
const arr = new Int32Array([1, 2, 3]);
const arrPtr = exports.__newArray(exports.Int32Array_ID, arr);

// Getting array back
const resultArr = exports.__getArray(arrPtr);

// Memory is automatically managed by AssemblyScript's runtime
// Manual cleanup rarely needed, but available if needed:
// exports.__release(ptr);
```

## Type Mappings

| AssemblyScript | JavaScript | Notes |
|----------------|------------|-------|
| `i32` | `number` | 32-bit integer |
| `i64` | `bigint` | 64-bit integer |
| `f32` | `number` | 32-bit float |
| `f64` | `number` | 64-bit float |
| `bool` | `number` | 0 or 1 |
| `string` | `string` | Via `__newString`/`__getString` |
| `Int32Array` | `Int32Array` | Via `__newArray`/`__getArray` |

## Performance

AssemblyScript compiles to highly optimized WebAssembly:

- **Math operations**: ~10x faster than JavaScript
- **String operations**: ~3-5x faster
- **Array operations**: ~5-10x faster
- **Zero-cost abstractions**: No runtime overhead

### Benchmarks

```bash
npm test
```

Example results:
- `isPrime()`: ~200,000 ops/sec
- `fibonacci(20)`: ~500,000 ops/sec
- `reverseString()`: ~1,000,000 ops/sec

## Optimization Tips

### 1. Use `unchecked` for Performance

```typescript
// Slower (bounds checking)
let sum = arr[i];

// Faster (no bounds checking)
let sum = unchecked(arr[i]);
```

### 2. Prefer `i32` over `i64` when possible

```typescript
// Faster on most platforms
function add(a: i32, b: i32): i32

// Slower (64-bit operations)
function add(a: i64, b: i64): i64
```

### 3. Avoid String Concatenation in Loops

```typescript
// Slow
let result = "";
for (let i = 0; i < 1000; i++) {
  result += "x";
}

// Fast
let parts: string[] = [];
for (let i = 0; i < 1000; i++) {
  parts.push("x");
}
let result = parts.join("");
```

### 4. Use Typed Arrays

```typescript
// Prefer typed arrays over regular arrays
let arr = new Int32Array(1000);
```

## Build Optimization

### Size Optimization

```json
{
  "release": {
    "optimizeLevel": 3,
    "shrinkLevel": 2,
    "converge": true
  }
}
```

### Speed Optimization

```json
{
  "release": {
    "optimizeLevel": 3,
    "shrinkLevel": 0
  }
}
```

## Debugging

### Enable Debug Build

```bash
npm run asbuild:debug
```

### Source Maps

Debug builds include source maps for easier debugging in browser DevTools.

### Logging

```typescript
import { console } from "env";

console.log("Debug message");
```

## Integration Examples

See `../../integration/` for examples with:
- React
- Vanilla JavaScript
- Node.js

## Troubleshooting

### "Cannot find module '@assemblyscript/loader'"

```bash
npm install @assemblyscript/loader
```

### "Memory access out of bounds"

- Check array indices
- Ensure proper memory allocation
- Use `unchecked()` carefully

### "Function not exported"

- Add `export` keyword to function
- Check `asconfig.json` export settings

## Comparison: AssemblyScript vs Rust

| Feature | AssemblyScript | Rust |
|---------|---------------|------|
| **Syntax** | TypeScript-like | Rust |
| **Learning Curve** | Easy | Steep |
| **Performance** | 90-95% of Rust | 100% |
| **Bundle Size** | Smaller | Larger |
| **Ecosystem** | Limited | Rich |
| **Use Case** | Quick WASM prototypes | Production apps |

## Resources

- [AssemblyScript Docs](https://www.assemblyscript.org/introduction.html)
- [AssemblyScript Book](https://www.assemblyscript.org/)
- [WebAssembly Specs](https://webassembly.github.io/spec/)

## License

See LICENSE file in project root.

---

**Author:** {{AUTHOR}}
