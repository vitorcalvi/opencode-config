---
name: opencode-templates
description: Code templates and snippets for rapid development
---

# OpenCode Templates Skill

## What I Do
- Generate code templates for common patterns
- Provide boilerplate code for projects
- Create starter files and configurations
- Suggest best practice implementations
- Accelerate development with pre-built snippets

## When to Use
Use when:
- User asks for code templates
- New project scaffolding is needed
- Boilerplate code is requested
- Common pattern implementation is needed
- Starting a new component/service
- Setup configuration files

## Template Categories

### 1. Project Templates

#### Node.js Project
```json
{
  "name": "nodejs-starter",
  "files": [
    "package.json",
    ".gitignore",
    "src/index.js",
    "README.md"
  ],
  "dependencies": {
    "express": "^4.18.0",
    "dotenv": "^16.0.0"
  }
}
```

```javascript
// src/index.js
import express from 'express';
import dotenv from 'dotenv';

dotenv.config();

const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.json());

app.get('/', (req, res) => {
  res.json({ message: 'Hello World!' });
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

#### React Component
```javascript
import React, { useState, useEffect } from 'react';

const Component = ({ initialData }) => {
  const [data, setData] = useState(initialData);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetchData();
  }, []);

  const fetchData = async () => {
    setLoading(true);
    try {
      const response = await fetch('/api/data');
      const result = await response.json();
      setData(result);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      {data && <pre>{JSON.stringify(data, null, 2)}</pre>}
    </div>
  );
};

export default Component;
```

#### Python Flask App
```python
from flask import Flask, jsonify, request
from functools import wraps
import os

app = Flask(__name__)

# Error handler
@app.errorhandler(404)
def not_found(error):
    return jsonify({'error': 'Not found'}), 404

@app.errorhandler(500)
def internal_error(error):
    return jsonify({'error': 'Internal server error'}), 500

# Routes
@app.route('/api/health', methods=['GET'])
def health():
    return jsonify({'status': 'healthy'})

@app.route('/api/data', methods=['GET', 'POST'])
def data():
    if request.method == 'POST':
        data = request.get_json()
        return jsonify({'received': data}), 201
    return jsonify({'message': 'GET request received'})

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=int(os.getenv('PORT', 5000)))
```

### 2. API Templates

#### REST API Endpoint (Express)
```javascript
// Create a RESTful API endpoint with validation
import express from 'express';
import { body, validationResult } from 'express-validator';

const router = express.Router();

// POST /api/users
router.post('/users',
  [
    body('email').isEmail().normalizeEmail(),
    body('name').trim().isLength({ min: 2, max: 50 })
  ],
  async (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }

    try {
      const { email, name } = req.body;
      const user = await createUser(email, name);
      res.status(201).json(user);
    } catch (error) {
      res.status(500).json({ error: 'Internal server error' });
    }
  }
);

export default router;
```

#### GraphQL Schema
```graphql
type User {
  id: ID!
  email: String!
  name: String!
  createdAt: DateTime!
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  content: String!
  author: User!
  createdAt: DateTime!
}

type Query {
  me: User
  user(id: ID!): User
  users(limit: Int, offset: Int): [User!]!
}

type Mutation {
  createUser(email: String!, name: String!): User!
  updateUser(id: ID!, name: String): User!
  deleteUser(id: ID!): Boolean!
}
```

### 3. Database Templates

#### Prisma Schema
```prisma
// schema.prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(uuid())
  email     String   @unique
  name      String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  posts     Post[]
}

model Post {
  id        String   @id @default(uuid())
  title     String
  content   String?
  published Boolean  @default(false)
  authorId  String
  author    User     @relation(fields: [authorId], references: [id])
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

#### MongoDB Schema
```javascript
// models/User.js
import mongoose from 'mongoose';

const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    trim: true
  },
  name: {
    type: String,
    required: true,
    trim: true
  },
  password: {
    type: String,
    required: true,
    select: false
  },
  role: {
    type: String,
    enum: ['user', 'admin'],
    default: 'user'
  }
}, {
  timestamps: true
});

userSchema.index({ email: 1 });

export default mongoose.model('User', userSchema);
```

### 4. Testing Templates

#### Jest Test Suite
```javascript
import { describe, it, expect, beforeEach, afterEach } from '@jest/globals';
import request from 'supertest';
import app from '../src/app';

describe('API Endpoints', () => {
  let server;

  beforeEach(() => {
    server = app.listen();
  });

  afterEach((done) => {
    server.close(done);
  });

  describe('GET /api/health', () => {
    it('should return healthy status', async () => {
      const response = await request(app).get('/api/health');
      expect(response.status).toBe(200);
      expect(response.body).toEqual({ status: 'healthy' });
    });
  });

  describe('POST /api/users', () => {
    it('should create a new user', async () => {
      const userData = {
        email: 'test@example.com',
        name: 'Test User'
      };

      const response = await request(app)
        .post('/api/users')
        .send(userData);

      expect(response.status).toBe(201);
      expect(response.body).toHaveProperty('id');
      expect(response.body.email).toBe(userData.email);
    });

    it('should return 400 for invalid email', async () => {
      const userData = {
        email: 'invalid-email',
        name: 'Test User'
      };

      const response = await request(app)
        .post('/api/users')
        .send(userData);

      expect(response.status).toBe(400);
      expect(response.body).toHaveProperty('errors');
    });
  });
});
```

#### Pytest Test
```python
import pytest
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

class TestUserEndpoints:
    def test_health_check(self):
        response = client.get("/api/health")
        assert response.status_code == 200
        assert response.json() == {"status": "healthy"}

    def test_create_user(self):
        user_data = {
            "email": "test@example.com",
            "name": "Test User"
        }
        response = client.post("/api/users", json=user_data)
        assert response.status_code == 201
        data = response.json()
        assert "id" in data
        assert data["email"] == user_data["email"]

    def test_invalid_email(self):
        user_data = {
            "email": "invalid",
            "name": "Test User"
        }
        response = client.post("/api/users", json=user_data)
        assert response.status_code == 400
```

### 5. Configuration Templates

#### Docker Compose
```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgresql://user:pass@db:5432/mydb
    depends_on:
      - db
      - redis

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

#### Kubernetes Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:1.0.0
        ports:
        - containerPort: 3000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: my-secrets
              key: database-url
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
```

### 6. Utility Templates

#### Logger Utility
```javascript
// utils/logger.js
import winston from 'winston';

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

export default logger;
```

#### Error Handler Middleware
```javascript
// middleware/errorHandler.js
export const errorHandler = (err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  const message = err.message || 'Internal Server Error';

  if (process.env.NODE_ENV === 'development') {
    return res.status(statusCode).json({
      success: false,
      error: {
        message,
        stack: err.stack
      }
    });
  }

  return res.status(statusCode).json({
    success: false,
    error: {
      message
    }
  });
};

export const notFound = (req, res, next) => {
  const error = new Error(`Not Found - ${req.originalUrl}`);
  res.status(404);
  next(error);
};
```

### 7. Authentication Templates

#### JWT Middleware
```javascript
// middleware/auth.js
import jwt from 'jsonwebtoken';
import { promisify } from 'util';

const verifyAsync = promisify(jwt.verify);

export const auth = async (req, res, next) => {
  try {
    const token = req.headers.authorization?.split(' ')[1];

    if (!token) {
      return res.status(401).json({ error: 'No token provided' });
    }

    const decoded = await verifyAsync(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(401).json({ error: 'Invalid token' });
  }
};
```

#### OAuth2 Configuration
```javascript
// config/oauth.js
import passport from 'passport';
import { Strategy as GoogleStrategy } from 'passport-google-oauth20';

passport.use(new GoogleStrategy({
  clientID: process.env.GOOGLE_CLIENT_ID,
  clientSecret: process.env.GOOGLE_CLIENT_SECRET,
  callbackURL: '/auth/google/callback'
},
async (accessToken, refreshToken, profile, done) => {
  try {
    let user = await findOrCreateUser(profile);
    return done(null, user);
  } catch (error) {
    return done(error, null);
  }
}));

export default passport;
```

### 8. CI/CD Templates

#### GitHub Actions
```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'

    - name: Install dependencies
      run: npm ci

    - name: Run tests
      run: npm test

    - name: Run linter
      run: npm run lint

    - name: Upload coverage
      uses: codecov/codecov-action@v3
```

#### GitLab CI
```yaml
# .gitlab-ci.yml
stages:
  - test
  - build
  - deploy

test:
  stage: test
  image: node:18
  script:
    - npm ci
    - npm test
  coverage: '/Coverage: \d+\.\d+%/'
  artifacts:
    reports:
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml

build:
  stage: build
  image: docker:latest
  script:
    - docker build -t my-app:$CI_COMMIT_SHA .
  only:
    - main

deploy:
  stage: deploy
  image: alpine/k8s:1.28.3
  script:
    - kubectl set image deployment/my-app my-app=my-app:$CI_COMMIT_SHA
  only:
    - main
```

## Template Generation Workflow

### 1. Analyze Request
```
1. Understand the use case
2. Identify programming language
3. Determine framework requirements
4. Check for existing patterns
5. Select appropriate template
```

### 2. Customize Template
```
1. Replace placeholder values
2. Adjust to project structure
3. Add specific requirements
4. Include best practices
5. Add necessary dependencies
```

### 3. Generate Output
```
1. Create file structure
2. Generate code files
3. Provide usage instructions
4. Include dependencies
5. Document configuration
```

## Usage Examples

### Generate React Component
```
"Create a React component with hooks"
"Generate a reusable Button component"
"Create a form with validation"
"Generate a data table component"
```

### Generate API Endpoint
```
"Create a REST API endpoint"
"Generate GraphQL schema for User"
"Create authentication middleware"
"Generate API documentation"
```

### Generate Configuration
```
"Create Docker Compose file"
"Generate Kubernetes deployment"
"Create GitHub Actions workflow"
"Generate webpack configuration"
```

### Generate Tests
```
"Create Jest test suite"
"Generate pytest test cases"
"Create integration tests"
"Create test fixtures"
```

## Best Practices

1. **Customizable** - Templates should be adaptable
2. **Well-documented** - Clear comments and usage
3. **Modern syntax** - Use latest language features
4. **Type-safe** - Include type definitions
5. **Security-conscious** - No hardcoded secrets
6. **Tested** - Verify template works
7. **Maintained** - Keep templates updated

## Safety Pattern

1. **No secrets** - Never include real credentials
2. **Placeholder values** - Use example data
3. **Document assumptions** - Explain what's needed
4. **Review required** - User should verify
5. **Version awareness** - Match project versions
6. **Framework compatibility** - Check dependencies

## Template Storage

### Local Templates
```bash
~/.config/opencode/templates/
├── react/
│   ├── component.js
│   ├── hooks.js
│   └── page.js
├── api/
│   ├── express.js
│   ├── fastapi.py
│   └── graphql.js
├── config/
│   ├── docker-compose.yml
│   └── kubernetes.yml
└── tests/
    ├── jest.js
    └── pytest.py
```

### Remote Templates
- GitHub repositories
- npm packages
- Custom template registries

## Output Format

```markdown
## Template: [Name]

### Description
[Brief description]

### Files Created
- `path/to/file1.js`
- `path/to/file2.js`

### Dependencies
```bash
npm install package1 package2
```

### Usage
[How to use the template]

### Customization Points
1. [Point 1]
2. [Point 2]

### Example
```[language]
[Code example]
```
```
