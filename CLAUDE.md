# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Turing Machine Simulator** web application for educational purposes. It supports both single-tape and multi-tape (up to 5) Turing machines with a visual simulation interface.

**Tech Stack:**
- **Backend:** Spring Boot 3.4.4 + MySQL + JPA + MyBatis
- **Frontend:** Angular 19 + TypeScript
- **Auth:** JWT-based authentication
- **Real-time:** WebSocket for chat features
- **Container:** Docker Compose for full stack deployment

## Commands

### Backend (Spring Boot)
```bash
# Build
mvn clean package

# Run (requires MySQL at localhost:3306)
mvn spring-boot:run

# Or run the JAR directly
java -jar target/WebPJ-0.0.1-SNAPSHOT.jar
```

### Frontend (Angular)
```bash
cd src/main/angular

# Install dependencies
npm install

# Development server (port 4200)
npm start
# or: ng serve --configuration development

# Production build
npm run build
# or: ng build --configuration production

# Watch mode
npm run watch

# Run tests
npm test
# or: ng test
```

### Docker (Full Stack)
```bash
# Start all services (MySQL + Backend + Frontend)
docker-compose up -d

# Development compose (with hot reload hints)
docker-compose -f docker-compose.dev.yml up -d

# Local development compose
docker-compose -f docker-compose.local.yml up -d
```

## Architecture

### Frontend (`src/main/angular/src/app/`)

**Core Components:**
- `components/turing-machine/` - Main Turing machine simulator with state machine execution logic
- `components/tape-display/` - Visual tape representation
- `components/control-panel/` - Control buttons (play, pause, step, reset)
- `components/transition-rules/` - Rule list display and management
- `components/add-rule/` - Rule creation/editing dialog
- `components/simulation/` - Simulation control panel
- `components/state-display/` - Current state visualization
- `components/free-mode/` / `learning-mode/` / `challenge-mode/` - Mode-specific pages

**Key Services:**
- `services/turing-machine.service.ts` - API client for machine CRUD operations
- `services/auth.guard.ts` - Route protection with JWT
- `services/ai-chat.service.ts` - AI assistant integration
- `services/toast.service.ts` - Toast notifications
- `service/indexeddb.service.ts` - Local persistence for tape state

**Key Models:**
- `models/tape.model.ts` - Tape class with head position, read/write operations
- `models/transitionrule.model.ts` - TransitionRule interface (supports single/multi-tape)

**Routing (`app.routes.ts`):**
- `/login` - Login/register page
- `/welcome` - Main welcome page
- `/free-mode`, `/learning-mode`, `/challenge-mode` - Three operation modes
- `/assignment-review`, `/teacher-excellent-assignments` - Teacher-only routes

### Backend (`src/main/java/com/example/webpj/`)

**Controllers:**
- `TuringMachineController` - Machine CRUD at `/api/machine`
- `AuthController` - Auth endpoints at `/api/auth`
- `ChallengeController` - Challenge questions at `/api/challenge`
- `AiChatController` - AI chat at `/api/ai`
- `ChatRoomController` - WebSocket chat at `/api/chatroom`

**Services:**
- `TuringMachineService` - Business logic for machine operations
- `UserService` - User management
- `ChallengeQuestionService` - Challenge question management

**Entities:**
- `TuringMachine` - Core entity with tape, state, configuration, rules
- `User` - User with role (student/teacher)
- `ChallengeQuestion` - Challenge questions with test cases

**Configuration:**
- `SecurityConfig.java` - Spring Security config with JWT filter
- `WebSocketConfig.java` - STOMP WebSocket setup
- `application.yml` - Database and AI endpoint settings

## Turing Machine Execution Model

The core execution happens in `turing-machine.component.ts`:

1. **Tape Model:** `Tape` class with `headPosition`, `read()`, `write()`, `headLeft()`, `headRight()`
2. **Single-tape execution:** Match `currentState + inputSymbol` against `transitionRules`
3. **Multi-tape execution:** Match `currentState + inputSymbols[]` array, apply `outputSymbols[]` and `moveDirections[]` in parallel
4. **Sync Actions:** Rule's `syncAction` field enables operations like `sum`, `multiply`, `carry` across tapes
5. **Deadlock detection:** Max 1000 steps, `deadlockDetected` flag

**Accept state:** `qAccept` stops execution automatically.

## Key Patterns

**API Communication:**
- Frontend sends auth via `Authorization: Bearer <token>` and `X-User-Name` headers
- All responses wrapped in `Result<T>` wrapper with `code`, `data`, `msg` fields
- API base URL configured in `environment.ts`

**State Persistence:**
- Server: MySQL via JPA/MyBatis
- Local: IndexedDB for offline tape state backup

**Multi-tape Rules:**
```typescript
{
  currentState: "q0",
  inputSymbols: ["0", "1"],      // Symbols read from each tape
  outputSymbols: ["1", "0"],     // Symbols to write
  moveDirections: ["R", "L"],    // Head movement per tape
  nextState: "q1",
  syncAction: "sum",             // Optional: cross-tape operation
  sourceTape: 0,
  targetTape: 1
}
```

## Database

- MySQL 8.0 container initialized with `turing_machine.sql` and `user.sql`
- JPA auto-updates schema (`ddl-auto: update`)
- Tables: `turing_machine`, `user`, `challenge_question`, `level`, `submission`

## Environment Variables

**Backend (`application.yml`):**
- `SPRING_DATASOURCE_URL` - MySQL connection string
- `SPRING_DATASOURCE_USERNAME/PASSWORD` - Database credentials
- `ai.endpoint` - AI API endpoint (智谱 GLM-4)
- `ai.api-key` - AI API key
