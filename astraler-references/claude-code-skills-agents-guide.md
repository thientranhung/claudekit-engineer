# Claude Code Extensibility: Skills & Agents Deep Dive

> **Tác giả**: Claude AI  
> **Ngày tạo**: 23/12/2024  
> **Mục đích**: Hướng dẫn chi tiết về cơ chế hoạt động của Skills và Agents trong Claude Code

---

## Mục lục

1. [Tổng quan 4 Pillars](#1-tổng-quan-4-pillars)
2. [Skills — Kho kiến thức theo yêu cầu](#2-skills--kho-kiến-thức-theo-yêu-cầu)
3. [Agents — Chuyên gia được ủy quyền](#3-agents--chuyên-gia-được-ủy-quyền)
4. [So sánh Skills vs Agents](#4-so-sánh-skills-vs-agents)
5. [Trigger Patterns — Cách viết prompt để Claude nhận diện](#5-trigger-patterns--cách-viết-prompt-để-claude-nhận-diện)
6. [Ví dụ hoàn chỉnh](#6-ví-dụ-hoàn-chỉnh)
7. [Flow tổng hợp](#7-flow-tổng-hợp)
8. [Tips viết prompt hiệu quả](#8-tips-viết-prompt-hiệu-quả)

---

## 1. Tổng quan 4 Pillars

Trước khi đi sâu, hãy hiểu 4 trụ cột của Claude Code extensibility:

| Component | Purpose                                       | Location              |
|-----------|-----------------------------------------------|-----------------------|
| Commands  | User-invoked slash commands (/asdf:spec)      | .claude/commands/     |
| Skills    | Knowledge modules Claude loads on-demand      | .claude/skills/       |
| Agents    | Specialized personas for Task tool delegation | .claude/agents/       |
| Hooks     | Pre/Post tool execution automation            | .claude/settings.json |

---

## 2. Skills — Kho kiến thức theo yêu cầu

### 2.1 Skills là gì?

Skills là những module kiến thức mà Claude sẽ tự động load khi cần thiết. Nó giống như việc bạn đưa cho Claude một cuốn sổ tay chuyên môn để tham khảo.

### 2.2 Cấu trúc thư mục

```
.claude/skills/
├── react-patterns.md
├── database-design.md
└── api-conventions.md
```

Khi bạn hỏi Claude về React, nó sẽ tự nhận ra và load `react-patterns.md` để có thêm context về cách bạn muốn viết React trong project này.

### 2.3 Cấu trúc một skill file chuẩn

```markdown
# Tên Skill

## Trigger (Khi nào load)
- Từ khóa: react, component, hook, useState
- Context: Khi user hỏi về frontend, UI

## Knowledge (Kiến thức)
[Nội dung chính ở đây]
```

### 2.4 Ví dụ skill file hoàn chỉnh

**File: `.claude/skills/tailwind-conventions.md`**

```markdown
# Tailwind CSS Conventions

## Trigger
Load skill này khi:
- User nhắc đến: styling, CSS, Tailwind, className, responsive
- Đang làm việc với file: *.tsx, *.jsx, *.html

## Conventions

### Class Order
Luôn sắp xếp theo thứ tự:
1. Layout (flex, grid, position)
2. Spacing (m-, p-)
3. Sizing (w-, h-)
4. Typography (text-, font-)
5. Colors (bg-, text-)
6. Effects (shadow, opacity)

### Responsive
Mobile-first approach:
- Default: mobile
- sm: tablet
- lg: desktop

### Ví dụ đúng/sai
// ✅ Đúng
<div className="flex items-center p-4 w-full text-sm text-gray-700 bg-white shadow-md">

// ❌ Sai
<div className="shadow-md bg-white text-gray-700 flex p-4 w-full items-center text-sm">
```

### 2.5 Flow hoạt động của Skills

```
User: "Style cái button này cho đẹp"
         ↓
Claude phân tích: "style" → liên quan đến CSS/Tailwind
         ↓
Claude tìm trong .claude/skills/ → thấy tailwind-conventions.md
         ↓
Claude đọc skill → áp dụng conventions vào response
```

---

## 3. Agents — Chuyên gia được ủy quyền

### 3.1 Agents là gì?

Agents là những "persona" chuyên biệt mà Claude chính có thể gọi ra thông qua **Task tool**. Mỗi agent có một vai trò và khả năng riêng.

### 3.2 Cấu trúc thư mục

```
.claude/agents/
├── code-reviewer.md
├── test-writer.md
└── docs-generator.md
```

### 3.3 Cấu trúc agent file chuẩn

```markdown
# Agent Name

## Description
[Mô tả ngắn - Claude dùng để quyết định có gọi agent này không]

## When to Use
[Điều kiện cụ thể]

## System Prompt
[Prompt cho agent]

## Allowed Tools
[Tools agent được phép dùng]
```

### 3.4 Ví dụ agent file hoàn chỉnh

**File: `.claude/agents/test-writer.md`**

```markdown
# Test Writer Agent

## Description
Chuyên gia viết unit tests và integration tests.

## When to Use
- User yêu cầu viết test
- User yêu cầu tăng code coverage
- Sau khi viết function/component mới

## System Prompt
Bạn là một QA Engineer chuyên viết tests.

### Nguyên tắc:
1. Mỗi function cần ít nhất 3 test cases: happy path, edge case, error case
2. Dùng Jest + React Testing Library
3. Test name theo format: "should [expected behavior] when [condition]"
4. Arrange-Act-Assert pattern

### Output format:
- File test đặt cạnh file gốc: Component.test.tsx
- Group tests bằng describe()
- Comments giải thích test phức tạp

## Allowed Tools
- Read files
- Write files
- Run terminal commands (npm test)
```

### 3.5 Flow hoạt động của Agents

```
Bạn: "Review file auth.ts cho tôi"
     ↓
Claude chính nhận request
     ↓
Claude gọi Task tool → delegate cho code-reviewer agent
     ↓
Agent review và trả kết quả về Claude chính
     ↓
Claude tổng hợp và trả lời bạn
```

---

## 4. So sánh Skills vs Agents

| Khía cạnh | Skills | Agents |
|-----------|--------|--------|
| **Bản chất** | Tài liệu tham khảo | Nhân vật được ủy quyền |
| **Ai sử dụng** | Claude đọc để có thêm context | Claude giao việc cho agent |
| **Khi nào dùng** | Cần kiến thức/quy tắc | Cần thực hiện task chuyên biệt |
| **Ví dụ** | "Coding conventions của project" | "Chuyên gia viết unit test" |
| **Cách kích hoạt** | Tự động dựa trên keyword/context | Delegate qua Task tool |
| **Output** | Không có output riêng | Trả kết quả về Claude chính |

---

## 5. Trigger Patterns — Cách viết prompt để Claude nhận diện

### 5.1 Pattern 1: Keyword-based

```markdown
## Trigger Keywords
Primary: test, testing, unit test, coverage, jest
Secondary: spec, assertion, mock, stub
```

### 5.2 Pattern 2: Intent-based

```markdown
## Trigger Intents
- "Viết test cho..."
- "Test cái này..."
- "Cần coverage cho..."
- "Kiểm tra function..."
```

### 5.3 Pattern 3: Context-based

```markdown
## Trigger Context
Tự động activate khi:
- Vừa tạo file mới trong /src
- Vừa sửa logic trong function
- PR review mode
```

### 5.4 Pattern 4: Explicit call

User có thể gọi trực tiếp:

```
User: "@test-writer viết test cho auth.ts"
```

hoặc

```
User: "Dùng test-writer agent để cover file này"
```

---

## 6. Ví dụ hoàn chỉnh

### 6.1 Skill: API Design

**File: `.claude/skills/api-design.md`**

```markdown
# API Design Standards

## Trigger
- Keywords: API, endpoint, route, REST, request, response
- Files: /api/*, /routes/*, *.controller.ts

## Standards

### URL Convention
- Plural nouns: /users, /products
- Nested resources: /users/:id/orders
- Actions dùng verb: /auth/login, /reports/generate

### Response Format
{
  "success": true,
  "data": {},
  "meta": {
    "timestamp": "",
    "requestId": ""
  }
}

### Error Format
{
  "success": false,
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User does not exist",
    "details": {}
  }
}

### HTTP Status
- 200: Success
- 201: Created
- 400: Bad Request
- 401: Unauthorized
- 404: Not Found
- 500: Server Error
```

### 6.2 Agent: API Builder

**File: `.claude/agents/api-builder.md`**

```markdown
# API Builder Agent

## Description
Xây dựng API endpoints hoàn chỉnh theo chuẩn project.

## When to Use
- "Tạo API cho..."
- "Build endpoint..."
- "CRUD cho entity..."

## System Prompt
Bạn là Backend Developer chuyên xây dựng RESTful APIs.

### Workflow:
1. Đọc skill "api-design" để nắm conventions
2. Tạo route file
3. Tạo controller
4. Tạo validation schema (Zod)
5. Tạo types/interfaces
6. Gọi test-writer agent để viết tests

### Tech stack:
- Express.js
- Zod validation
- Prisma ORM
- TypeScript

## Allowed Tools
- Read/Write files
- Run commands
- Delegate to: test-writer, docs-generator

## Output Structure
Khi tạo API mới, luôn tạo đủ files:
/api/[resource]/
  ├── route.ts
  ├── controller.ts
  ├── validation.ts
  ├── types.ts
  └── [resource].test.ts
```

---

## 7. Flow tổng hợp

Khi user request: **"Tạo CRUD API cho products"**

```
User: "Tạo CRUD API cho products"
              ↓
┌─────────────────────────────────────┐
│  Claude Code (Main)                 │
│  1. Phân tích: "API" + "CRUD"       │
│  2. Load skill: api-design.md       │
│  3. Quyết định delegate             │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│  API Builder Agent                  │
│  1. Đọc conventions từ skill        │
│  2. Tạo route, controller, etc      │
│  3. Gọi test-writer agent           │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│  Test Writer Agent                  │
│  1. Đọc code vừa tạo                │
│  2. Viết test cases                 │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│  Claude Code (Main)                 │
│  Tổng hợp và trả kết quả cho user   │
└─────────────────────────────────────┘
```

### Ví dụ kết hợp cả hai trong thực tế

Bạn nói: *"Viết API endpoint cho user registration"*

```
1. Claude load skill "api-conventions.md" 
   → Biết project dùng Express, validation với Zod, response format chuẩn

2. Claude viết code theo conventions

3. Claude gọi agent "test-writer" 
   → Agent viết unit tests cho endpoint đó

4. Claude gọi agent "docs-generator"
   → Agent tạo API documentation

5. Claude trả về cho bạn: code + tests + docs
```

---

## 8. Tips viết prompt hiệu quả

| Mục đích | Cách viết |
|----------|-----------|
| Load skill cụ thể | Nhắc keyword trong trigger |
| Gọi agent trực tiếp | "@agent-name" hoặc "dùng [agent] để..." |
| Chạy workflow | Mô tả task phức tạp, Claude tự orchestrate |
| Override behavior | "Không cần test" hoặc "Skip documentation" |

### Ví dụ prompt patterns:

```
# Load skill tự động
"Style component này theo Tailwind conventions"
→ Claude tự load tailwind-conventions.md

# Gọi agent trực tiếp
"@code-reviewer check file auth.ts"
"Dùng test-writer agent viết test cho utils/"

# Workflow phức tạp
"Tạo complete CRUD cho products với tests và docs"
→ Claude orchestrate: skill + api-builder + test-writer + docs-generator

# Override
"Tạo API cho users, không cần tests"
→ Claude skip test-writer agent
```

---

## Kết luận

**Skills** = Kiến thức thụ động (Claude đọc để biết)  
**Agents** = Chuyên gia chủ động (Claude giao việc để làm)

Kết hợp cả hai giúp bạn:
- Duy trì coding conventions nhất quán
- Tự động hóa các task lặp đi lặp lại
- Scale workflow phức tạp
- Đảm bảo chất lượng code

---

*Tài liệu này được tạo từ cuộc trò chuyện giải thích về Claude Code extensibility.*
