---
name: coding-standards
description: Universal coding standards, best practices, and patterns for all projects. Includes database, API, and coding conventions.
origin: ECC
---

# Coding Standards & Best Practices

Universal coding standards applicable across all projects.

## When to Activate

- Starting a new project or module
- Designing database schemas and tables
- Creating or modifying API endpoints
- Writing database models and entities
- Reviewing code for quality and maintainability
- Refactoring existing code to follow conventions
- Enforcing naming, formatting, or structural consistency
- Setting up linting, formatting, or type-checking rules
- Onboarding new contributors to coding conventions
- Writing SQL queries or migrations

## Code Quality Principles

### 1. Readability First
- Code is read more than written
- Clear variable and function names
- Self-documenting code preferred over comments
- Consistent formatting

### 2. KISS (Keep It Simple, Stupid)
- Simplest solution that works
- Avoid over-engineering
- No premature optimization
- Easy to understand > clever code

### 3. DRY (Don't Repeat Yourself)
- Extract common logic into functions
- Create reusable components
- Share utilities across modules
- Avoid copy-paste programming

### 4. YAGNI (You Aren't Gonna Need It)
- Don't build features before they're needed
- Avoid speculative generality
- Add complexity only when required
- Start simple, refactor when needed

## TypeScript/JavaScript Standards

### Naming Conventions

**Follow these rules consistently across all code:**

```typescript
// ✅ GOOD: Correct naming conventions
class UserService {                          // PascalCase for classes
  private maxRetryCount: number = 3                // camelCase for properties

  async getUserById(userId: string) {             // camelCase for methods
    const isActive = true                          // camelCase for variables
    const API_BASE_URL = 'https://api.example.com' // UPPER_SNAKE_CASE for constants

    const DEFAULT_TIMEOUT_MS = 5000
    const MAX_CONNECTIONS = 10
  }
}

// ❌ BAD: Incorrect naming
class userService {                          // Wrong: should be PascalCase
  private MaxRetryCount: number = 3              // Wrong: should be camelCase

  async GetUserById(userId: string) {            // Wrong: should be camelCase
    const is_Active = true                      // Wrong: should be camelCase
    const api_base_url = 'https://api.example.com'  // Wrong: constants should be UPPER_SNAKE_CASE
  }
}
```

### Variable Naming (Detailed)

```typescript
// ✅ GOOD: Descriptive names, camelCase
const marketSearchQuery = 'election'
const isUserAuthenticated = true
const totalRevenue = 1000
const userProfile = getUserProfile()
const hasPermission = checkPermission()

// ❌ BAD: Unclear names
const q = 'election'
const flag = true
const x = 1000
const up = getUserProfile()
const perm = checkPermission()
```

### Function Naming

```typescript
// ✅ GOOD: Verb-noun pattern
async function fetchMarketData(marketId: string) { }
function calculateSimilarity(a: number[], b: number[]) { }
function isValidEmail(email: string): boolean { }

// ❌ BAD: Unclear or noun-only
async function market(id: string) { }
function similarity(a, b) { }
function email(e) { }
```

### Immutability Pattern (CRITICAL)

```typescript
// ✅ ALWAYS use spread operator
const updatedUser = {
  ...user,
  name: 'New Name'
}

const updatedArray = [...items, newItem]

// ❌ NEVER mutate directly
user.name = 'New Name'  // BAD
items.push(newItem)     // BAD
```

### Error Handling

```typescript
// ✅ GOOD: Comprehensive error handling
async function fetchData(url: string) {
  try {
    const response = await fetch(url)

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`)
    }

    return await response.json()
  } catch (error) {
    console.error('Fetch failed:', error)
    throw new Error('Failed to fetch data')
  }
}

// ❌ BAD: No error handling
async function fetchData(url) {
  const response = await fetch(url)
  return response.json()
}
```

### Async/Await Best Practices

```typescript
// ✅ GOOD: Parallel execution when possible
const [users, markets, stats] = await Promise.all([
  fetchUsers(),
  fetchMarkets(),
  fetchStats()
])

// ❌ BAD: Sequential when unnecessary
const users = await fetchUsers()
const markets = await fetchMarkets()
const stats = await fetchStats()
```

### Type Safety

```typescript
// ✅ GOOD: Proper types
interface Market {
  id: string
  name: string
  status: 'active' | 'resolved' | 'closed'
  created_at: Date
}

function getMarket(id: string): Promise<Market> {
  // Implementation
}

// ❌ BAD: Using 'any'
function getMarket(id: any): Promise<any> {
  // Implementation
}
```

## React Best Practices

### Component Structure

```typescript
// ✅ GOOD: Functional component with types
interface ButtonProps {
  children: React.ReactNode
  onClick: () => void
  disabled?: boolean
  variant?: 'primary' | 'secondary'
}

export function Button({
  children,
  onClick,
  disabled = false,
  variant = 'primary'
}: ButtonProps) {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`btn btn-${variant}`}
    >
      {children}
    </button>
  )
}

// ❌ BAD: No types, unclear structure
export function Button(props) {
  return <button onClick={props.onClick}>{props.children}</button>
}
```

### Custom Hooks

```typescript
// ✅ GOOD: Reusable custom hook
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value)
    }, delay)

    return () => clearTimeout(handler)
  }, [value, delay])

  return debouncedValue
}

// Usage
const debouncedQuery = useDebounce(searchQuery, 500)
```

### State Management

```typescript
// ✅ GOOD: Proper state updates
const [count, setCount] = useState(0)

// Functional update for state based on previous state
setCount(prev => prev + 1)

// ❌ BAD: Direct state reference
setCount(count + 1)  // Can be stale in async scenarios
```

### Conditional Rendering

```typescript
// ✅ GOOD: Clear conditional rendering
{isLoading && <Spinner />}
{error && <ErrorMessage error={error} />}
{data && <DataDisplay data={data} />}

// ❌ BAD: Ternary hell
{isLoading ? <Spinner /> : error ? <ErrorMessage error={error} /> : data ? <DataDisplay data={data} /> : null}
```

## Database Standards

### Database Selection
- Use **MySQL** as the default database for all projects

### Table Naming Conventions

```sql
-- ✅ GOOD: Singular form, descriptive
CREATE TABLE user (
  id BIGINT PRIMARY KEY,
  user_name VARCHAR(50),
  email VARCHAR(100)
);

CREATE TABLE order_item (
  id BIGINT PRIMARY KEY,
  order_id BIGINT,
  product_id BIGINT
);

-- ❌ BAD: Plural form
CREATE TABLE users (
  id BIGINT PRIMARY KEY
);

CREATE TABLE order_items (
  id BIGINT PRIMARY KEY
);
```

### Reserved Keyword Conflicts

When table names conflict with MySQL reserved keywords, prefix with `tbl_`:

```sql
-- ✅ GOOD: Added prefix for reserved keywords
CREATE TABLE tbl_user (
  id BIGINT PRIMARY KEY
);

CREATE TABLE tbl_order (
  id BIGINT PRIMARY KEY,
  order_date DATETIME
);

-- Common reserved keywords that need tbl_ prefix:
-- user, order, group, select, insert, update, delete, drop, etc.
```

### Column Naming Conventions

```sql
-- ✅ GOOD: Lowercase with underscores
CREATE TABLE example_table (
  id BIGINT PRIMARY KEY,
  user_name VARCHAR(50),
  email_address VARCHAR(100),
  created_at DATETIME,
  is_active BOOLEAN
);

-- ❌ BAD: Uppercase or camelCase
CREATE TABLE example_table (
  ID BIGINT PRIMARY KEY,
  userName VARCHAR(50),
  EmailAddress VARCHAR(100)
);
```

### Index Naming

```sql
-- Primary key: pk_{table}
-- Foreign key: fk_{table}_{referenced_table}
-- Unique key: uk_{table}_{column}
-- Index: idx_{table}_{column}

-- Examples
CONSTRAINT pk_user PRIMARY KEY (id),
CONSTRAINT fk_user_role FOREIGN KEY (role_id) REFERENCES role(id),
CONSTRAINT uk_user_email UNIQUE (email),
INDEX idx_user_created_at (created_at)
```

### ⚠️ CRITICAL Rules (MUST FOLLOW)

**1. No Foreign Keys**
- ❌ NEVER create foreign key constraints
- ✅ Handle relationships at application level
- Rationale: Foreign keys complicate deployment, testing, and can cause cascading deletion issues

```sql
-- ❌ BAD: Foreign key constraint
CREATE TABLE user (
  id BIGINT PRIMARY KEY,
  role_id BIGINT,
  CONSTRAINT fk_user_role FOREIGN KEY (role_id) REFERENCES role(id)
);

-- ✅ GOOD: No foreign key, handle in application code
CREATE TABLE user (
  id BIGINT PRIMARY KEY,
  role_id BIGINT COMMENT 'Role ID, references role.id'
);
```

**2. All Columns Must Have Comment**
- ✅ EVERY column must have a COMMENT
- Comments should describe what the column stores
- Use clear, concise Chinese or English

```sql
-- ✅ GOOD: All columns have comments
CREATE TABLE user (
  id BIGINT PRIMARY KEY COMMENT '用户ID',
  user_name VARCHAR(50) NOT NULL COMMENT '用户名',
  email VARCHAR(100) NOT NULL COMMENT '邮箱地址',
  is_active BOOLEAN DEFAULT TRUE COMMENT '是否激活',
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
);

-- ❌ BAD: Missing comments
CREATE TABLE user (
  id BIGINT PRIMARY KEY,
  user_name VARCHAR(50) NOT NULL,
  email VARCHAR(100)
);
```

**3. Default Values Are Mandatory**
- ✅ Every column that can have a default value MUST have one
- Include: BOOLEAN, DATETIME, TIMESTAMP, numeric fields with common defaults
- Use sensible defaults: `0`, `''`, `TRUE`, `FALSE`, `CURRENT_TIMESTAMP`

```sql
-- ✅ GOOD: Proper defaults
CREATE TABLE user (
  id BIGINT PRIMARY KEY COMMENT '用户ID',
  is_active BOOLEAN DEFAULT TRUE COMMENT '是否激活',
  status TINYINT DEFAULT 1 COMMENT '状态 0=禁用 1=正常',
  login_count INT DEFAULT 0 COMMENT '登录次数',
  last_login_at DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '最后登录时间',
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
);

-- ❌ BAD: Missing defaults where applicable
CREATE TABLE user (
  id BIGINT PRIMARY KEY,
  is_active BOOLEAN COMMENT '是否激活',
  status TINYINT COMMENT '状态',
  last_login_at DATETIME COMMENT '最后登录时间'
);
```

**3.1 DEFAULT + NOT NULL Rule (CRITICAL)**
- ⚠️ **强制规则**: 所有可以设置 DEFAULT 值的字段，必须同时设置为 NOT NULL
- 这个规则是双向的：
  - 有 DEFAULT 的字段 → 必须是 NOT NULL
  - NOT NULL 的字段 → 必须有 DEFAULT
- 例外：主键字段(AUTO_INCREMENT)、真正需要 NULL 的可选字段(如 deleted_at)

```sql
-- ✅ GOOD: DEFAULT + NOT NULL 配合使用
CREATE TABLE user (
  id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID',
  user_name VARCHAR(50) NOT NULL DEFAULT '' COMMENT '用户名',
  email VARCHAR(100) NOT NULL DEFAULT '' COMMENT '邮箱地址',
  is_active TINYINT NOT NULL DEFAULT 1 COMMENT '是否激活 0=否 1=是',
  status TINYINT NOT NULL DEFAULT 0 COMMENT '状态',
  login_count INT NOT NULL DEFAULT 0 COMMENT '登录次数',
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  deleted_at DATETIME DEFAULT NULL COMMENT '删除时间(允许NULL表示未删除)'
);

-- ❌ BAD: 有 DEFAULT 但没有 NOT NULL
CREATE TABLE user (
  id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID',
  is_active TINYINT DEFAULT 1 COMMENT '是否激活',  -- 缺少 NOT NULL
  status TINYINT DEFAULT 0 COMMENT '状态',         -- 缺少 NOT NULL
  login_count INT DEFAULT 0 COMMENT '登录次数'     -- 缺少 NOT NULL
);
```

**4. NOT NULL Is Mandatory**
- ✅ Every column that can be NOT NULL must be NOT NULL
- Only allow NULL for truly optional fields
- Most fields should be NOT NULL by default

```sql
-- ✅ GOOD: Proper NOT NULL constraints
CREATE TABLE user (
  id BIGINT PRIMARY KEY COMMENT '用户ID',
  user_name VARCHAR(50) NOT NULL COMMENT '用户名',
  email VARCHAR(100) NOT NULL COMMENT '邮箱地址',
  phone VARCHAR(20) COMMENT '手机号(可选)',
  is_active BOOLEAN NOT NULL DEFAULT TRUE COMMENT '是否激活',
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
);

-- ❌ BAD: Missing NOT NULL where applicable
CREATE TABLE user (
  id BIGINT PRIMARY KEY,
  user_name VARCHAR(50) COMMENT '用户名',
  email VARCHAR(100) COMMENT '邮箱地址',
  is_active BOOLEAN COMMENT '是否激活'
);
```

**5. NOT NULL Requires DEFAULT**
- ⚠️ **CRITICAL**: If a field is NOT NULL, it MUST have a DEFAULT value
- This prevents insert errors when columns are not explicitly specified
- The only exception: Primary key fields with AUTO_INCREMENT

```sql
-- ✅ GOOD: NOT NULL + DEFAULT
CREATE TABLE user (
  id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID',
  user_name VARCHAR(50) NOT NULL DEFAULT '' COMMENT '用户名',
  email VARCHAR(100) NOT NULL DEFAULT '' COMMENT '邮箱地址',
  phone VARCHAR(20) DEFAULT '' COMMENT '手机号',
  is_active BOOLEAN NOT NULL DEFAULT TRUE COMMENT '是否激活',
  status TINYINT NOT NULL DEFAULT 1 COMMENT '状态',
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
);

-- ❌ BAD: NOT NULL without DEFAULT (non-PK fields)
CREATE TABLE user (
  id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID',
  user_name VARCHAR(50) NOT NULL COMMENT '用户名',  -- 缺少 DEFAULT
  email VARCHAR(100) NOT NULL COMMENT '邮箱地址',  -- 缺少 DEFAULT
  is_active BOOLEAN NOT NULL DEFAULT TRUE COMMENT '是否激活'  -- 正确
);

-- ✅ CORRECT: PK with AUTO_INCREMENT doesn't need DEFAULT
id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '用户ID'  -- OK, 无需 DEFAULT
```

**6. Field Type Standards**

**6.1 Money/Amount Fields**
- ❌ NEVER use DECIMAL, FLOAT, or DOUBLE for money
- ✅ ALWAYS use BIGINT or INT to store amount in **cents/fen** (分)
- Application layer handles conversion to/from yuan (元)

```sql
-- ✅ GOOD: Store in cents/fen
amount BIGINT NOT NULL DEFAULT 0 COMMENT '订单金额(分)',
price INT NOT NULL DEFAULT 0 COMMENT '商品价格(分)',
discount INT DEFAULT 0 COMMENT '折扣金额(分)'

-- ❌ BAD: Floating point for money
amount DECIMAL(10,2) COMMENT '订单金额',
price FLOAT COMMENT '商品价格'
```

**6.2 No ENUM Types**
- ❌ NEVER use ENUM type
- ✅ Use TINYINT + COMMENT to define enumerated values
- Rationale: ENUM is inflexible, hard to modify, and not portable

```sql
-- ✅ GOOD: TINYINT + COMMENT
status TINYINT NOT NULL DEFAULT 1 COMMENT '状态 0=禁用 1=正常 2=冻结',
user_type TINYINT NOT NULL DEFAULT 0 COMMENT '用户类型 0=普通 1=VIP 2=SVIP',
order_status TINYINT NOT NULL DEFAULT 0 COMMENT '订单状态 0=待支付 1=已支付 2=已发货 3=已完成 4=已取消'

-- ❌ BAD: ENUM type
status ENUM('disabled', 'active', 'frozen'),
user_type ENUM('normal', 'vip', 'svip')
```

**6.3 Gender Field**
- ✅ Use TINYINT for gender
- Standard mapping: `0 = 女性`, `1 = 男性`
- Allow NULL for unspecified/unknown

```sql
-- ✅ GOOD
gender TINYINT DEFAULT NULL COMMENT '性别 0=女 1=男 NULL=未知'

-- ❌ BAD
gender ENUM('female', 'male'),
gender VARCHAR(10),
gender BOOLEAN
```

**6.4 Time/DateTime Fields**
- ✅ Database: Use **DATETIME** type
- ✅ API传输: Use **ISO8601** format (e.g., `2024-01-15T10:30:00Z`)
- ❌ NEVER use TIMESTAMP type
- ❌ NEVER store as VARCHAR

```sql
-- ✅ GOOD: Database uses DATETIME
created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
deleted_at DATETIME DEFAULT NULL COMMENT '删除时间'

-- ❌ BAD: Using TIMESTAMP or VARCHAR
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
created_at VARCHAR(50)
```

```typescript
// ✅ GOOD: API uses ISO8601
{
  "created_at": "2024-01-15T10:30:00+08:00",
  "updated_at": "2024-01-15T12:00:00Z"
}

// ❌ BAD: Other formats
{
  "created_at": "2024-01-15 10:30:00",
  "created_at": 1705290600
}
```

**6.5 Status Fields**
- ✅ Priority: Use TINYINT for status fields
- ✅ Always add COMMENT explaining each value
- Common pattern: 0 = inactive/disabled, 1 = active/enabled

```sql
-- ✅ GOOD: TINYINT + detailed COMMENT
status TINYINT NOT NULL DEFAULT 0 COMMENT '状态 0=禁用 1=启用 2=审核中 3=已拒绝',
is_active TINYINT NOT NULL DEFAULT 1 COMMENT '是否激活 0=否 1=是',
audit_status TINYINT NOT NULL DEFAULT 0 COMMENT '审核状态 0=待审核 1=已通过 2=已拒绝',
payment_status TINYINT NOT NULL DEFAULT 0 COMMENT '支付状态 0=待支付 1=已支付 2=已退款 3=已取消'

-- ❌ BAD: INT or ENUM
status INT DEFAULT 0 COMMENT '状态',
status ENUM('disabled', 'enabled')
```

**6.6 Timestamp Fields (Required)**
- ✅ **EVERY table must have `created_at` and `updated_at` fields**
- Both must be NOT NULL with DEFAULT CURRENT_TIMESTAMP
- Use ON UPDATE CURRENT_TIMESTAMP for `updated_at`

```sql
-- ✅ GOOD: Required timestamp fields
created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'

-- ❌ BAD: Missing timestamps or wrong types
created_at BIGINT COMMENT '创建时间戳',
created_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
-- 缺少 updated_at
```

### Complete Example (Following All Rules)

```sql
-- ✅ GOOD: Follows all database standards
CREATE TABLE tbl_order (
  id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT '订单ID',
  order_no VARCHAR(50) NOT NULL DEFAULT '' COMMENT '订单号',
  user_id BIGINT NOT NULL DEFAULT 0 COMMENT '用户ID',
  total_amount BIGINT NOT NULL DEFAULT 0 COMMENT '订单总金额(分)',
  discount_amount BIGINT DEFAULT 0 COMMENT '折扣金额(分)',
  actual_amount BIGINT NOT NULL DEFAULT 0 COMMENT '实付金额(分)',
  status TINYINT NOT NULL DEFAULT 0 COMMENT '订单状态 0=待支付 1=已支付 2=已发货 3=已完成 4=已取消',
  payment_status TINYINT NOT NULL DEFAULT 0 COMMENT '支付状态 0=未支付 1=已支付 2=已退款',
  gender TINYINT DEFAULT NULL COMMENT '用户性别 0=女 1=男 NULL=未知',
  created_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  updated_at DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',

  UNIQUE KEY uk_order_no (order_no),
  INDEX idx_user_id (user_id),
  INDEX idx_status (status),
  INDEX idx_created_at (created_at)
) COMMENT='订单表';
```

**Key Points**:
- `id`: PRIMARY KEY + AUTO_INCREMENT → ✅ 无需 DEFAULT
- `total_amount`, `actual_amount`: BIGINT 存储分 → ✅ 规则6.1
- `status`, `payment_status`: TINYINT + 详细注释 → ✅ 规则6.2, 6.5
- `gender`: TINYINT with NULL option → ✅ 规则6.3
- `created_at`, `updated_at`: DATETIME → ✅ 规则6.4, 6.6
- All NOT NULL fields have DEFAULT → ✅ 规则5

### Migration Checklist

Before committing a database migration, verify:
- [ ] No foreign key constraints
- [ ] Every column has a COMMENT
- [ ] All possible columns have DEFAULT values
- [ ] All non-nullable columns are set to NOT NULL
- [ ] NOT NULL columns must have DEFAULT values (except PK with AUTO_INCREMENT)
- [ ] DEFAULT columns must be NOT NULL (双向规则 → 规则3.1)
- [ ] Money fields use BIGINT/INT (cents/fen) → ✅ 规则6.1
- [ ] No ENUM types, use TINYINT + COMMENT → ✅ 规则6.2
- [ ] Gender uses TINYINT (0=女, 1=男) → ✅ 规则6.3
- [ ] Time fields use DATETIME → ✅ 规则6.4
- [ ] Status fields use TINYINT + COMMENT → ✅ 规则6.5
- [ ] Table has created_at and updated_at → ✅ 规则6.6
- [ ] Table name follows naming conventions (singular, tbl_ prefix for conflicts)
- [ ] Column names use lowercase with underscores
- [ ] Indexes follow naming conventions

## API Design Standards

### REST API Conventions

```
GET    /api/users                # List all users
GET    /api/users/:id            # Get specific user
POST   /api/users                # Create new user
PUT    /api/users/:id            # Update user (full)
PATCH  /api/users/:id            # Update user (partial)
DELETE /api/users/:id            # Delete user

# Path naming: lowercase with hyphens
GET    /api/security-groups      # ✅ GOOD
GET    /api/securityGroups       # ❌ BAD
GET    /api/securityGroups       # ❌ BAD

# Query parameters for filtering
GET /api/users?status=active&limit=10&offset=0
```

### Response Format

```typescript
// ✅ GOOD: Standardized response format
interface ApiResponse<T> {
  success: boolean
  message: string
  data?: T
  count?: number
}

// Success response
return {
  success: true,
  message: "查询成功",
  data: users,
  count: 100
}

// Error response
return {
  success: false,
  message: "参数错误",
  data: null,
  count: 0
}
```

### DateTime Format

**Date and time must follow ISO8601 format in all API responses:**

```typescript
// ✅ GOOD: ISO8601 format with timezone
{
  "created_at": "2024-01-15T10:30:00+08:00",
  "updated_at": "2024-01-15T12:00:00Z",
  "deleted_at": "2024-01-20T15:45:00.123Z"
}

// ❌ BAD: Other formats
{
  "created_at": "2024-01-15 10:30:00",
  "created_at": "2024/01/15 10:30:00",
  "created_at": 1705290600,
  "created_at": "2024-01-15T10:30:00"  // Missing timezone
}
```

**JavaScript/TypeScript handling:**

```typescript
// ✅ GOOD: Convert to ISO8601
function toISOString(date: Date): string {
  return date.toISOString()  // Returns: 2024-01-15T10:30:00.000Z
}

// ✅ GOOD: Parse from ISO8601
function fromISOString(isoString: string): Date {
  return new Date(isoString)
}

// ✅ GOOD: Database DATETIME to API ISO8601
function dateTimeToAPI(dbDateTime: string): string {
  return new Date(dbDateTime).toISOString()
}
```

### Input Validation

```typescript
import { z } from 'zod'

// ✅ GOOD: Schema validation
const CreateMarketSchema = z.object({
  name: z.string().min(1).max(200),
  description: z.string().min(1).max(2000),
  endDate: z.string().datetime(),
  categories: z.array(z.string()).min(1)
})

export async function POST(request: Request) {
  const body = await request.json()

  try {
    const validated = CreateMarketSchema.parse(body)
    // Proceed with validated data
  } catch (error) {
    if (error instanceof z.ZodError) {
      return {
        success: false,
        message: 'Validation failed',
        data: null,
        error: error.errors
      }
    }
  }
}
```

## File Organization

### Project Structure

```
src/
├── app/                    # Next.js App Router
│   ├── api/               # API routes
│   ├── markets/           # Market pages
│   └── (auth)/           # Auth pages (route groups)
├── components/            # React components
│   ├── ui/               # Generic UI components
│   ├── forms/            # Form components
│   └── layouts/          # Layout components
├── hooks/                # Custom React hooks
├── lib/                  # Utilities and configs
│   ├── api/             # API clients
│   ├── utils/           # Helper functions
│   └── constants/       # Constants
├── types/                # TypeScript types
└── styles/              # Global styles
```

### File Naming

```
components/Button.tsx          # PascalCase for components
hooks/useAuth.ts              # camelCase with 'use' prefix
lib/formatDate.ts             # camelCase for utilities
types/market.types.ts         # camelCase with .types suffix
```

## Comments & Documentation

### When to Comment

```typescript
// ✅ GOOD: Explain WHY, not WHAT
// Use exponential backoff to avoid overwhelming the API during outages
const delay = Math.min(1000 * Math.pow(2, retryCount), 30000)

// Deliberately using mutation here for performance with large arrays
items.push(newItem)

// ❌ BAD: Stating the obvious
// Increment counter by 1
count++

// Set name to user's name
name = user.name
```

### JSDoc for Public APIs

```typescript
/**
 * Searches markets using semantic similarity.
 *
 * @param query - Natural language search query
 * @param limit - Maximum number of results (default: 10)
 * @returns Array of markets sorted by similarity score
 * @throws {Error} If OpenAI API fails or Redis unavailable
 *
 * @example
 * ```typescript
 * const results = await searchMarkets('election', 5)
 * console.log(results[0].name) // "Trump vs Biden"
 * ```
 */
export async function searchMarkets(
  query: string,
  limit: number = 10
): Promise<Market[]> {
  // Implementation
}
```

## Performance Best Practices

### Memoization

```typescript
import { useMemo, useCallback } from 'react'

// ✅ GOOD: Memoize expensive computations
const sortedMarkets = useMemo(() => {
  return markets.sort((a, b) => b.volume - a.volume)
}, [markets])

// ✅ GOOD: Memoize callbacks
const handleSearch = useCallback((query: string) => {
  setSearchQuery(query)
}, [])
```

### Lazy Loading

```typescript
import { lazy, Suspense } from 'react'

// ✅ GOOD: Lazy load heavy components
const HeavyChart = lazy(() => import('./HeavyChart'))

export function Dashboard() {
  return (
    <Suspense fallback={<Spinner />}>
      <HeavyChart />
    </Suspense>
  )
}
```

### Database Queries

```typescript
// ✅ GOOD: Select only needed columns
const { data } = await supabase
  .from('markets')
  .select('id, name, status')
  .limit(10)

// ❌ BAD: Select everything
const { data } = await supabase
  .from('markets')
  .select('*')
```

## Testing Standards

### Test Structure (AAA Pattern)

```typescript
test('calculates similarity correctly', () => {
  // Arrange
  const vector1 = [1, 0, 0]
  const vector2 = [0, 1, 0]

  // Act
  const similarity = calculateCosineSimilarity(vector1, vector2)

  // Assert
  expect(similarity).toBe(0)
})
```

### Test Naming

```typescript
// ✅ GOOD: Descriptive test names
test('returns empty array when no markets match query', () => { })
test('throws error when OpenAI API key is missing', () => { })
test('falls back to substring search when Redis unavailable', () => { })

// ❌ BAD: Vague test names
test('works', () => { })
test('test search', () => { })
```

## Code Smell Detection

Watch for these anti-patterns:

### 1. Long Functions
```typescript
// ❌ BAD: Function > 50 lines
function processMarketData() {
  // 100 lines of code
}

// ✅ GOOD: Split into smaller functions
function processMarketData() {
  const validated = validateData()
  const transformed = transformData(validated)
  return saveData(transformed)
}
```

### 2. Deep Nesting
```typescript
// ❌ BAD: 5+ levels of nesting
if (user) {
  if (user.isAdmin) {
    if (market) {
      if (market.isActive) {
        if (hasPermission) {
          // Do something
        }
      }
    }
  }
}

// ✅ GOOD: Early returns
if (!user) return
if (!user.isAdmin) return
if (!market) return
if (!market.isActive) return
if (!hasPermission) return

// Do something
```

### 3. Magic Numbers
```typescript
// ❌ BAD: Unexplained numbers
if (retryCount > 3) { }
setTimeout(callback, 500)

// ✅ GOOD: Named constants
const MAX_RETRIES = 3
const DEBOUNCE_DELAY_MS = 500

if (retryCount > MAX_RETRIES) { }
setTimeout(callback, DEBOUNCE_DELAY_MS)
```

**Remember**: Code quality is not negotiable. Clear, maintainable code enables rapid development and confident refactoring.
