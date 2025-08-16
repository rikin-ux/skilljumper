# SkillJumper Development Setup Guide

## Prerequisites

Before setting up SkillJumper for development, ensure you have the following tools installed:

### Required Software

| Tool | Version | Purpose | Installation |
|------|---------|---------|-------------|
| **Node.js** | 18.17+ | Runtime environment | [nodejs.org](https://nodejs.org/) |
| **npm** | 9.0+ | Package manager | Included with Node.js |
| **Git** | 2.30+ | Version control | [git-scm.com](https://git-scm.com/) |
| **React Native CLI** | Latest | Mobile development | `npm install -g @react-native-community/cli` |
| **TypeScript** | 5.0+ | Type safety | `npm install -g typescript` |

### Mobile Development

#### iOS Development (macOS only)
- **Xcode** 14.0+ (from App Store)
- **iOS Simulator** (included with Xcode)
- **CocoaPods** 1.11+: `sudo gem install cocoapods`

#### Android Development
- **Android Studio** (latest stable)
- **Android SDK** (API level 31+)
- **Java Development Kit** 11+

### Backend Development
- **PostgreSQL** 14+ (local development)
- **Redis** 6.0+ (caching and sessions)
- **Docker** 20.0+ (containerization)
- **Docker Compose** 2.0+

## Quick Start

### 1. Clone the Repository

```bash
# Clone the main repository
git clone https://github.com/skilljumper/algorithm.git
cd skilljumper

# Install dependencies
npm install

# Set up git hooks for code quality
npm run prepare
```

### 2. Environment Configuration

```bash
# Copy environment template
cp .env.example .env.local

# Edit with your local settings
nano .env.local
```

#### Environment Variables

```bash
# .env.local
# Database Configuration
DATABASE_URL="postgresql://skilljumper:password@localhost:5432/skilljumper_dev"
REDIS_URL="redis://localhost:6379"

# API Configuration
API_BASE_URL="http://localhost:3000"
API_SECRET_KEY="your-dev-secret-key-here"

# Authentication
OAUTH_CLIENT_ID="dev_client_id"
OAUTH_CLIENT_SECRET="dev_client_secret"
JWT_SECRET="your-jwt-secret-here"

# Algorithm Configuration
ALGORITHM_VERSION="1.0.0-dev"
ALGORITHM_DEBUG_MODE="true"
RECOMMENDATION_CACHE_TTL="300"

# Mobile App Configuration
MOBILE_API_ENDPOINT="http://localhost:3000/api/v1"
ENABLE_FLIPPER="true"
LOG_LEVEL="debug"

# External Services (Optional for local dev)
SENTRY_DSN="your-sentry-dsn"
ANALYTICS_API_KEY="your-analytics-key"
```

### 3. Database Setup

```bash
# Start PostgreSQL and Redis with Docker
docker-compose up -d postgres redis

# Run database migrations
npm run db:migrate

# Seed with development data
npm run db:seed

# Verify database connection
npm run db:test
```

### 4. Start Development Servers

```bash
# Terminal 1: Start backend API
npm run dev:api

# Terminal 2: Start mobile app (Metro bundler)
npm run dev:mobile

# Terminal 3: Start iOS simulator
npm run ios

# Terminal 4: Start Android emulator
npm run android
```

## Project Structure Walkthrough

```
skilljumper/
├── 📱 mobile/                    # React Native mobile app
│   ├── src/
│   │   ├── components/           # Reusable UI components
│   │   │   ├── Quest/           # Quest-related components
│   │   │   ├── Profile/         # User profile components
│   │   │   └── Common/          # Shared components
│   │   ├── screens/             # App screens and navigation
│   │   │   ├── Quest/           # Quest flow screens
│   │   │   ├── Profile/         # Profile management
│   │   │   └── Onboarding/      # Initial setup
│   │   ├── services/            # API clients and local services
│   │   │   ├── api/             # Backend API client
│   │   │   ├── storage/         # Local data persistence
│   │   │   └── algorithm/       # Local algorithm engine
│   │   ├── state/               # Global state management
│   │   │   ├── user/            # User profile state
│   │   │   ├── quest/           # Quest-related state
│   │   │   └── algorithm/       # Algorithm state
│   │   └── utils/               # Helper functions
│   ├── android/                 # Android-specific code
│   ├── ios/                     # iOS-specific code
│   └── __tests__/              # Mobile app tests
│
├── 🔧 backend/                   # Core Services Platform
│   ├── src/
│   │   ├── api/                 # API routes and controllers
│   │   │   ├── auth/            # Authentication endpoints
│   │   │   ├── users/           # User management
│   │   │   ├── quests/          # Quest management
│   │   │   └── analytics/       # Progress tracking
│   │   ├── services/            # Business logic layer
│   │   │   ├── QuestEngine/     # Quest recommendation service
│   │   │   ├── UserService/     # User management service
│   │   │   └── AnalyticsService/ # Analytics and insights
│   │   ├── models/              # Database models
│   │   ├── middleware/          # Express middleware
│   │   └── utils/               # Backend utilities
│   ├── migrations/              # Database migrations
│   ├── seeds/                   # Development data
│   └── __tests__/              # Backend tests
│
├── 🧠 algorithm/                 # Core Algorithm Engine
│   ├── src/
│   │   ├── QuestEngine.ts       # Main algorithm implementation
│   │   ├── UserModel.ts         # Learning model and preferences
│   │   ├── QuestDatabase.ts     # Quest storage and retrieval
│   │   ├── types/               # TypeScript interfaces
│   │   │   ├── User.ts          # User-related types
│   │   │   ├── Quest.ts         # Quest-related types
│   │   │   └── Algorithm.ts     # Algorithm-specific types
│   │   ├── adapters/            # External service adapters
│   │   └── utils/               # Algorithm utilities
│   └── __tests__/              # Algorithm tests
│
├── 📚 content/                   # Quest content and metadata
│   ├── quests/                  # Quest definitions
│   │   ├── organization/        # Organization skill quests
│   │   ├── social/              # Social skill quests
│   │   └── self-care/           # Self-care quests
│   ├── adaptations/             # Accessibility adaptations
│   └── schemas/                 # Content validation schemas
│
├── 🧪 tests/                     # Integration and E2E tests
│   ├── integration/             # Cross-service tests
│   ├── e2e/                     # End-to-end test suites
│   └── fixtures/                # Test data and mocks
│
├── 📖 docs/                      # Documentation
│   ├── api/                     # API documentation
│   ├── architecture/            # System design docs
│   └── research/                # Research and methodology
│
├── 🚀 deployment/                # Infrastructure and deployment
│   ├── docker/                  # Docker configurations
│   ├── kubernetes/              # K8s manifests
│   └── scripts/                 # Deployment scripts
│
└── 📋 Configuration Files
    ├── package.json             # Root dependencies
    ├── tsconfig.json            # TypeScript configuration
    ├── .eslintrc.js             # Code linting rules
    ├── .prettierrc              # Code formatting
    ├── jest.config.js           # Testing configuration
    ├── docker-compose.yml       # Local development services
    └── .github/                 # CI/CD workflows
```

## Development Workflow

### 1. Feature Development Process

```bash
# 1. Create feature branch
git checkout -b feature/quest-recommendation-improvements

# 2. Make changes and test locally
npm run test:unit
npm run test:integration

# 3. Run algorithm validation
npm run algorithm:validate

# 4. Check code quality
npm run lint
npm run type-check

# 5. Test mobile app on both platforms
npm run test:ios
npm run test:android

# 6. Commit with conventional commits
git commit -m "feat(algorithm): improve contextual scoring for ADHD users"

# 7. Push and create pull request
git push origin feature/quest-recommendation-improvements
```

### 2. Testing Strategy

#### Unit Tests
```bash
# Run all unit tests
npm run test:unit

# Run specific test suite
npm run test:unit -- algorithm/QuestEngine

# Run tests with coverage
npm run test:coverage

# Watch mode for development
npm run test:watch
```

#### Integration Tests
```bash
# Start test database
npm run db:test:start

# Run API integration tests
npm run test:integration:api

# Run algorithm integration tests
npm run test:integration:algorithm

# Run mobile app integration tests
npm run test:integration:mobile
```

#### End-to-End Tests
```bash
# Start all services
npm run dev:all

# Run E2E test suite
npm run test:e2e

# Run specific user journey
npm run test:e2e -- --spec "quest-completion-flow"

# Run tests on multiple devices
npm run test:e2e:devices
```

### 3. Algorithm Development

#### Local Algorithm Testing
```bash
# Test algorithm with sample data
npm run algorithm:test

# Validate algorithm performance
npm run algorithm:benchmark

# Test with different user profiles
npm run algorithm:test:profiles

# Generate algorithm performance report
npm run algorithm:report
```

#### Algorithm Configuration
```typescript
// algorithm/config/development.ts
export const algorithmConfig = {
  scoring: {
    contextualWeight: 0.4,
    personalityWeight: 0.3,
    historyWeight: 0.2,
    difficultyWeight: 0.1
  },
  
  adaptation: {
    maxAdaptationsPerQuest: 3,
    difficultyAdjustmentRange: 0.5,
    timeModificationLimits: [0.5, 2.0]
  },
  
  recommendation: {
    candidatePoolSize: 50,
    fallbackOptionsCount: 3,
    confidenceThreshold: 0.7
  },
  
  debug: {
    enableDetailedLogging: true,
    saveRecommendationTrace: true,
    enablePerformanceMetrics: true
  }
};
```

## Mobile App Development

### iOS Setup
```bash
# Navigate to iOS directory
cd mobile/ios

# Install CocoaPods dependencies
pod install

# Return to project root
cd ../..

# Start iOS simulator
npm run ios

# Or specify device
npm run ios -- --simulator="iPhone 14 Pro"
```

### Android Setup
```bash
# Start Android emulator (if not running)
emulator -avd Pixel_4_API_31

# Start Android app
npm run android

# Or specify device
npm run android -- --deviceId=emulator-5554
```

### Mobile Development Tips

#### Hot Reloading & Debugging
```bash
# Enable Flipper debugging (iOS/Android)
# Flipper will automatically detect the app

# React Native Debugger
npm install -g react-native-debugger
npm run debugger

# Enable fast refresh in development
# Already configured in metro.config.js
```

#### Testing on Physical Devices
```bash
# iOS - Deploy to connected device
npm run ios -- --device

# Android - Deploy to connected device
npm run android -- --variant=debug
```

## Backend Development

### API Server Setup
```bash
# Start development server with hot reload
npm run dev:api

# Start with debugging enabled
npm run dev:api:debug

# Check API health
curl http://localhost:3000/health
```

### Database Management
```bash
# Create new migration
npm run db:migration:create add_quest_adaptations

# Run pending migrations
npm run db:migrate

# Rollback last migration
npm run db:migrate:down

# Reset database (development only)
npm run db:reset

# View current database status
npm run db:status
```

### API Testing
```bash
# Run API tests
npm run test:api

# Test specific endpoint
npm run test:api -- --grep "quest recommendation"

# Load testing
npm run test:load

# API documentation generation
npm run docs:api:generate
```

## Code Quality & Standards

### Linting and Formatting
```bash
# Check and fix linting issues
npm run lint
npm run lint:fix

# Format code with Prettier
npm run format

# Type checking
npm run type-check

# All quality checks
npm run quality:check
```

### Git Hooks (Husky)
```bash
# Pre-commit hook runs automatically:
# - Lint staged files
# - Type check
# - Run relevant tests
# - Format code

# Pre-push hook runs:
# - Full test suite
# - Algorithm validation
# - Build verification
```

### Conventional Commits
```bash
# Use conventional commit format:
feat(algorithm): add contextual scoring for time constraints
fix(mobile): resolve quest completion animation issue
docs(api): update authentication flow documentation
test(backend): add integration tests for user service
refactor(algorithm): optimize quest filtering performance
```

## Debugging & Troubleshooting

### Common Issues

#### 1. Node Modules Issues
```bash
# Clear npm cache
npm cache clean --force

# Remove node_modules and reinstall
rm -rf node_modules package-lock.json
npm install

# Clear React Native cache
npx react-native start --reset-cache
```

#### 2. iOS Build Issues
```bash
# Clean iOS build
cd mobile/ios
xcodebuild clean
rm -rf build
cd ../..

# Reinstall pods
cd mobile/ios
pod deintegrate
pod install
cd ../..
```

#### 3. Android Build Issues
```bash
# Clean Android build
cd mobile/android
./gradlew clean
cd ../..

# Reset Metro cache
npx react-native start --reset-cache
```

#### 4. Database Connection Issues
```bash
# Check if PostgreSQL is running
docker-compose ps

# Reset database
npm run db:reset

# Check connection manually
psql postgresql://skilljumper:password@localhost:5432/skilljumper_dev
```

### Debugging Tools

#### Algorithm Debugging
```typescript
// Enable algorithm debugging in development
import { DebugQuestEngine } from '../algorithm/debug/DebugQuestEngine';

const engine = new DebugQuestEngine({
  enableTracing: true,
  logLevel: 'verbose',
  saveDecisionTree: true
});

// Get detailed recommendation explanation
const recommendation = await engine.selectOptimalQuest(criteria);
console.log(recommendation.debugInfo);
```

#### Mobile App Debugging
```bash
# React Native Debugger
npm run debugger

# Flipper debugging
# Open Flipper app and connect to running simulator

# Chrome DevTools
# Shake device/simulator and select "Debug"
```

#### Backend Debugging
```bash
# Debug mode with Node.js inspector
npm run dev:api:debug

# Open Chrome DevTools
# Navigate to chrome://inspect
```

## Performance Optimization

### Algorithm Performance
```bash
# Benchmark algorithm performance
npm run algorithm:benchmark

# Profile memory usage
npm run algorithm:profile

# Test with large datasets
npm run algorithm:stress-test
```

### Mobile App Performance
```bash
# Profile React Native performance
npm run profile:mobile

# Bundle size analysis
npm run analyze:bundle

# Memory leak detection
npm run test:memory-leaks
```

### Backend Performance
```bash
# Load testing
npm run test:load

# Database query optimization
npm run db:analyze

# API response time profiling
npm run profile:api
```

## Deployment Preparation

### Build Process
```bash
# Build all components
npm run build

# Build mobile app for production
npm run build:mobile:ios
npm run build:mobile:android

# Build backend for production
npm run build:backend

# Build algorithm package
npm run build:algorithm
```

### Docker Development
```bash
# Build development containers
docker-compose build

# Start full development environment
docker-compose up

# Run tests in containers
docker-compose run --rm api npm test
docker-compose run --rm mobile npm run test:unit
```

### Environment Testing
```bash
# Test production build locally
npm run test:production

# Test deployment scripts
npm run test:deployment

# Validate environment variables
npm run validate:env
```

## IDE Configuration

### VS Code Setup
```json
// .vscode/settings.json
{
  "typescript.preferences.includePackageJsonAutoImports": "on",
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "eslint.autoFixOnSave": true,
  "files.exclude": {
    "**/node_modules": true,
    "**/build": true,
    "**/dist": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/build": true,
    "**/dist": true
  }
}
```

### Recommended Extensions
- **TypeScript**: Enhanced TypeScript support
- **Prettier**: Code formatting
- **ESLint**: Code linting
- **React Native Tools**: React Native development
- **Docker**: Container management
- **GitLens**: Git integration
- **Jest**: Testing support

## Development Scripts Reference

### Package.json Scripts
```json
{
  "scripts": {
    // Development
    "dev": "concurrently \"npm run dev:api\" \"npm run dev:mobile\"",
    "dev:api": "nodemon backend/src/server.ts",
    "dev:mobile": "react-native start",
    "dev:all": "docker-compose up",
    
    // Building
    "build": "npm run build:backend && npm run build:algorithm",
    "build:backend": "tsc -p backend/tsconfig.json",
    "build:mobile:ios": "cd mobile && react-native run-ios --configuration Release",
    "build:mobile:android": "cd mobile && react-native run-android --variant=release",
    "build:algorithm": "tsc -p algorithm/tsconfig.json",
    
    // Testing
    "test": "jest",
    "test:unit": "jest --testPathPattern=unit",
    "test:integration": "jest --testPathPattern=integration",
    "test:e2e": "detox test",
    "test:coverage": "jest --coverage",
    "test:watch": "jest --watch",
    
    // Algorithm specific
    "algorithm:test": "jest algorithm/",
    "algorithm:benchmark": "node scripts/benchmark-algorithm.js",
    "algorithm:validate": "node scripts/validate-algorithm.js",
    
    // Database
    "db:migrate": "knex migrate:latest",
    "db:seed": "knex seed:run",
    "db:reset": "npm run db:migrate:down && npm run db:migrate && npm run db:seed",
    
    // Code Quality
    "lint": "eslint . --ext .ts,.tsx,.js,.jsx",
    "lint:fix": "eslint . --ext .ts,.tsx,.js,.jsx --fix",
    "format": "prettier --write .",
    "type-check": "tsc --noEmit",
    
    // Mobile specific
    "ios": "cd mobile && react-native run-ios",
    "android": "cd mobile && react-native run-android",
    "debugger": "open \"rndebugger://set-debugger-loc?host=localhost&port=8081\"",
    
    // Utilities
    "clean": "rimraf node_modules build dist",
    "prepare": "husky install",
    "validate:env": "node scripts/validate-environment.js"
  }
}
```

## Getting Help

### Development Support
- **Documentation**: Check `/docs` directory for detailed guides
- **Discord**: Join our [developer Discord](https://discord.gg/skilljumper-dev)
- **GitHub Issues**: Report bugs and request features
- **Stack Overflow**: Tag questions with `skilljumper`

### Common Commands Quick Reference
```bash
# Quick start (first time)
git clone https://github.com/skilljumper/algorithm.git
cd skilljumper
npm install
cp .env.example .env.local
npm run db:setup
npm run dev

# Daily development
npm run dev                    # Start all services
npm run test:unit             # Run unit tests
npm run algorithm:test        # Test algorithm
npm run ios                   # Start iOS app
npm run android              # Start Android app

# Before committing
npm run lint:fix              # Fix linting issues
npm run test                  # Run all tests
npm run type-check           # Check TypeScript

# Troubleshooting
npm run clean                 # Clean dependencies
npm run db:reset             # Reset database
npx react-native start --reset-cache  # Clear RN cache
```

---

This development setup guide provides everything needed to get SkillJumper running locally. For more specific topics, check the detailed documentation in the `/docs` directory.