# Currency Handling

## Core Rules

- **Store monetary values as integers in minor units** (e.g., cents, not dollars)
  - `$19.99` → store as `1999`
  - This eliminates floating-point precision issues

- **Use dedicated money libraries for calculations**:
  - TypeScript/JavaScript: `dinero.js`
  - Python: `decimal.Decimal` with appropriate precision
  - Go: Use integer arithmetic or `shopspring/decimal`
  - C#: `decimal` type (not `float` or `double`)

- **Always specify currency codes** using ISO 4217 (e.g., `USD`, `EUR`, `GBP`)
  - Never assume a default currency
  - Store currency code alongside every monetary value

- **Handle rounding and precision explicitly**:
  - Define rounding strategy per business context (HALF_UP, HALF_EVEN, etc.)
  - Document rounding behavior in code comments
  - Test edge cases: $0.00, negative amounts, max values, currency conversion

## Example Pattern (TypeScript)

```typescript
import { dinero, add, toDecimal } from 'dinero.js'
import { USD } from '@dinero.js/currencies'

// Create money values
const price = dinero({ amount: 1999, currency: USD }) // $19.99
const tax = dinero({ amount: 160, currency: USD })     // $1.60

// Safe arithmetic
const total = add(price, tax) // $21.59, no floating point issues

// Display
const display = toDecimal(total) // "21.59"
```
