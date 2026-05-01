# Task Manager — Backend Developer Assignment

## Overview

Full-stack task manager demonstrating REST API development with JWT authentication, role-based access control, and CRUD operations. Built as a backend developer assignment project.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Auth**: JWT (jsonwebtoken) + bcryptjs password hashing
- **Storage**: In-memory (Maps) — no database required
- **Validation**: Zod (`zod/v4`) via OpenAPI-generated schemas
- **API codegen**: Orval (from OpenAPI spec)
- **Frontend**: React + Vite + TanStack Query + shadcn/ui + wouter

## Architecture

### Backend (`artifacts/api-server`)
- Routes under `/api/auth/*` — registration, login, get current user
- Routes under `/api/tasks/*` — full CRUD, protected by JWT middleware
- In-memory storage in `src/lib/store.ts` (users Map + tasks Map)
- JWT middleware in `src/middlewares/auth.ts`
- Role-based access: users see own tasks, admins see all tasks

### Frontend (`artifacts/task-manager`)
- Login and Signup pages with form validation
- Dashboard showing tasks with create/delete functionality
- JWT stored in localStorage, automatically injected via custom-fetch
- Admin users see all tasks with userId labels

## API Endpoints

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | /api/auth/register | No | Register user (username, password, role?) |
| POST | /api/auth/login | No | Login, returns JWT |
| GET | /api/auth/me | JWT | Get current user |
| GET | /api/tasks | JWT | List tasks (own or all if admin) |
| POST | /api/tasks | JWT | Create task |
| GET | /api/tasks/:id | JWT | Get single task |
| PUT | /api/tasks/:id | JWT | Update task |
| DELETE | /api/tasks/:id | JWT | Delete task |

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from OpenAPI spec

**Note after codegen:** Remove the `export * from "./generated/types";` line from `lib/api-zod/src/index.ts` to avoid duplicate exports.

See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details.
