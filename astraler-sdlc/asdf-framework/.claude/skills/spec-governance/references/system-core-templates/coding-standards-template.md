# Coding Standards Template

Use this template for `02-standards/coding-standards.md`.

---

```markdown
# Coding Standards

> **Version:** 1.0.0
> **Status:** Draft | Review | Approved
> **Last Updated:** YYMMDD

---

## 1. Overview

This document defines coding conventions for consistency and maintainability.

---

## 2. Project Structure

### Backend Structure

```
backend/
├── src/
│   ├── modules/           # Feature modules
│   │   └── [module]/
│   │       ├── [module].module.ts
│   │       ├── [module].controller.ts
│   │       ├── [module].service.ts
│   │       ├── dto/
│   │       └── entities/
│   ├── common/            # Shared utilities
│   │   ├── decorators/
│   │   ├── filters/
│   │   ├── guards/
│   │   └── interceptors/
│   ├── config/            # Configuration
│   └── database/          # Database module
├── prisma/
│   └── schema.prisma
└── test/
```

### Frontend Structure

```
frontend/
├── src/
│   ├── app/               # App configuration
│   ├── components/        # Reusable components
│   │   ├── ui/            # Base components
│   │   ├── layout/        # Layout components
│   │   └── shared/        # Shared components
│   ├── features/          # Feature modules
│   │   └── [feature]/
│   │       ├── components/
│   │       ├── hooks/
│   │       ├── api.ts
│   │       └── types.ts
│   ├── hooks/             # Global hooks
│   ├── lib/               # Utilities
│   ├── stores/            # State management
│   └── types/             # TypeScript types
└── public/
```

---

## 3. Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Classes | PascalCase | `UserService`, `AuthGuard` |
| Interfaces | PascalCase | `User`, `CreateUserDto` |
| Functions | camelCase | `getUserById`, `validateToken` |
| Variables | camelCase | `accessToken`, `userId` |
| Constants | SCREAMING_SNAKE | `MAX_RETRY_COUNT`, `API_BASE_URL` |
| Files | kebab-case | `user.service.ts`, `auth.guard.ts` |
| Folders | kebab-case | `user-management/`, `data-processing/` |
| Components | PascalCase | `UserCard.tsx`, `AuthForm.tsx` |
| Hooks | camelCase, use- prefix | `useAuth`, `useUserData` |

---

## 4. TypeScript Standards

### Strict Mode (Required)

```json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true
  }
}
```

### Type Definitions

```typescript
// ✅ GOOD: Explicit types
interface User {
  id: string;
  email: string;
  role: UserRole;
}

// ❌ BAD: Using `any`
const user: any = fetchUser();

// ✅ GOOD: Use unknown + type guard
const user: unknown = fetchUser();
if (isUser(user)) { /* ... */ }

// ✅ GOOD: Prefer interface over type for objects
interface Config {
  apiUrl: string;
  timeout: number;
}

// ✅ GOOD: Use type for unions/intersections
type Status = 'pending' | 'active' | 'archived';
```

---

## 5. Code Style

### Functions

```typescript
// ✅ GOOD: Single responsibility, clear naming
async function getUserById(id: string): Promise<User | null> {
  return await prisma.user.findUnique({ where: { id } });
}

// ❌ BAD: Too many responsibilities
async function handleUser(id: string, action: string, data?: any) {
  // Does everything...
}
```

### Error Handling

```typescript
// ✅ GOOD: Custom error classes
export class UserNotFoundError extends Error {
  constructor(userId: string) {
    super(`User ${userId} not found`);
    this.name = 'UserNotFoundError';
  }
}

// ✅ GOOD: Proper error handling
try {
  const user = await userService.findById(id);
  if (!user) throw new UserNotFoundError(id);
} catch (error) {
  if (error instanceof UserNotFoundError) {
    // Handle specific error
  }
  throw error;
}
```

---

## 6. Documentation

### JSDoc for Public APIs

```typescript
/**
 * Creates a new user account.
 *
 * @param dto - User creation data
 * @returns The created user
 * @throws UserAlreadyExistsError if email is taken
 *
 * @example
 * const user = await userService.create({
 *   email: 'user@example.com',
 *   password: 'secure123'
 * });
 */
async create(dto: CreateUserDto): Promise<User> {
  // ...
}
```

### When to Comment

- Complex algorithms or business logic
- Non-obvious code behavior
- TODOs with issue references

```typescript
// TODO(#123): Optimize query when user count > 10k
// HACK: Workaround for library bug, remove after v2.0
// FIXME: Race condition under high load
```

---

## 7. Import Order

```typescript
// 1. Node built-ins
import { readFile } from 'fs/promises';

// 2. External packages
import { Injectable } from '@nestjs/common';
import { PrismaClient } from '@prisma/client';

// 3. Internal aliases (@/)
import { UserService } from '@/modules/user';
import { AuthGuard } from '@/common/guards';

// 4. Relative imports
import { CreateUserDto } from './dto';
import { User } from './entities';
```

---

## 8. Testing Standards

### File Naming

```
component.test.ts       # Unit tests
component.spec.ts       # Integration tests
component.e2e-spec.ts   # E2E tests
```

### Test Structure (AAA Pattern)

```typescript
describe('UserService', () => {
  describe('create', () => {
    it('should create user with valid data', async () => {
      // Arrange
      const dto = { email: 'test@example.com', password: 'secure123' };

      // Act
      const result = await userService.create(dto);

      // Assert
      expect(result).toMatchObject({ email: dto.email });
      expect(result.id).toBeDefined();
    });

    it('should throw if email already exists', async () => {
      // ...
    });
  });
});
```

---

## 9. Git Conventions

### Commit Messages

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation
- `style`: Formatting
- `refactor`: Code change (no feature/fix)
- `test`: Tests
- `chore`: Maintenance

**Example:**
```
feat(auth): add password reset functionality

- Add forgot password endpoint
- Add reset token generation
- Add email notification

Closes #123
```

### Branch Naming

```
feat/user-authentication
fix/login-redirect-issue
docs/api-documentation
refactor/user-service
```

---

## 10. Anti-Patterns (AVOID)

| Anti-Pattern | Why | Instead |
|--------------|-----|---------|
| Magic numbers | Hard to understand | Use named constants |
| God classes | Hard to maintain | Single responsibility |
| Deep nesting | Hard to read | Early returns, extract functions |
| Commented code | Creates confusion | Delete or use version control |
| Hardcoded values | Not configurable | Use environment variables |

---

## 11. Open Questions

| # | Question | Impact | Status |
|---|----------|--------|--------|
| 1 | [Unresolved standard] | [Impact] | Open |

---

## 12. Changelog

### YYMMDD - v1.0.0 - Initial Draft
- Defined naming conventions
- Set TypeScript standards
- Established testing patterns
```

---

## Validation Rules

- [ ] Version header present
- [ ] Project structure documented
- [ ] Naming conventions defined
- [ ] TypeScript strict mode enforced
- [ ] Testing standards defined
- [ ] Git conventions documented
