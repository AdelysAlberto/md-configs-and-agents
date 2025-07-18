# 📝 Logger Documentation - Onboarding Service

[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-24.3-green)](https://nodejs.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 🎯 Overview

The logger is a centralized logging system for Remesas365's Onboarding KYC microservice. It provides consistent, colorized, and structured formatting for all system logs, with full support for Spanish date formatting and multiple logging levels. Built with TypeScript strict mode and following SOLID principles.

## 📍 Location & Architecture

```
src/shared/utils/logger.ts
src/shared/utils/index.ts (barrel export)
```

Following **Screaming Architecture** principles, the logger resides in the shared utilities layer, making it accessible across all domain modules.

## 🚀 Installation & Setup

### Import Options

```typescript
// Barrel import (recommended)
import { logger } from '../shared/utils';

// Direct import with types
import { logger, type LogLevel, type LogData } from '../shared/utils/logger';

// Destructured import
import { logger } from '../shared/utils/logger';
```

### Auto-Configuration

The logger requires no additional configuration and comes pre-configured with:

- **Timezone**: `America/Mexico_City`
- **Locale**: Spanish (es-ES) for dates
- **Date Format**: "15 de julio de 2025, 14:30:25"
- **Output**: Colorized console with emoji indicators
- **Encoding**: UTF-8 with full Unicode support

## 📊 Log Levels & Visual Indicators

| Level | Emoji | Color | Use Case | When to Use |
|-------|-------|--------|----------|-------------|
| `error` | 🔴 | Red | Critical errors, exceptions | System failures, unhandled exceptions, API errors |
| `warn` | 🟡 | Yellow | Warnings, anomalous situations | Deprecated features, rate limits, retries |
| `info` | 🔵 | Cyan | General information, normal flow | User actions, successful operations, lifecycle events |
| `debug` | ⚪ | White | Debugging, technical details | Development debugging, detailed tracing |

## 🛠️ Core API Methods

### Primary Logging Methods

#### `logger.error(data: LogData)`
Records critical system errors and exceptions.

```typescript
logger.error({
  message: 'Failed to connect to MongoDB',
  error: err.message,
  collection: 'onboarding',
  userId: '12345',
  retryAttempt: 3,
  connectionString: 'mongodb://localhost:27017' // Safe to log without credentials
});

// Output:
// 🔴 [15 de julio de 2025, 14:30:25] ERROR Failed to connect to MongoDB
//    {
//      error: 'Connection timeout',
//      collection: 'onboarding',
//      userId: '12345',
//      retryAttempt: 3,
//      connectionString: 'mongodb://localhost:27017'
//    }
```

#### `logger.warn(data: LogData)`
Records warnings and situations requiring attention.

```typescript
logger.warn({
  message: 'Metamap token will expire soon',
  expiresIn: '2 hours',
  sessionId: 'sess_123',
  action: 'token_refresh_needed'
});

// Enhanced warning with context
logger.warn({
  message: 'KYC attempt limit approaching',
  userId: 'user_456',
  currentAttempts: 2,
  maxAttempts: 3,
  timeWindow: '24h'
});
```

#### `logger.info(data: LogData)`
Records general application flow information.

```typescript
logger.info({
  message: 'Onboarding process started successfully',
  userId: '12345',
  email: 'user@remesas365.com',
  sessionId: 'sess_789',
  ipAddress: req.ip,
  userAgent: req.get('User-Agent')
});

// Business logic success
logger.info({
  message: 'KYC verification completed',
  userId: '12345',
  provider: 'Metamap',
  verificationId: 'kyc_abc123',
  status: 'approved',
  documentsVerified: ['passport', 'selfie'],
  processingTime: '45s'
});
```

#### `logger.debug(data: LogData)`
Records detailed debugging information for development.

```typescript
logger.debug({
  message: 'Validating Zod schema',
  schema: 'startOnboardingSchema',
  payload: req.body,
  validationRules: ['email', 'userId', 'phoneNumber']
});

// Performance debugging
logger.debug({
  message: 'Database query executed',
  query: 'findUserByEmail',
  executionTime: '125ms',
  recordsFound: 1,
  indexUsed: 'email_idx'
});
```

## 🎯 Specialized Logging Methods

### `logger.server(port: number | string, environment?: string)`
Records server startup information with enhanced context.

```typescript
logger.server(3000, 'production');
// 🔵 [15 de julio de 2025, 14:30:25] INFO  🚀 Server started successfully
//    {
//      port: 3000,
//      environment: 'production',
//      timestamp: '15 de julio de 2025, 14:30:25',
//      nodeVersion: '24.3.0',
//      pid: 12345
//    }

// Enhanced server startup
logger.server(process.env.PORT || 3000, process.env.NODE_ENV);
```

### `logger.http(method: string, url: string, statusCode: number, responseTime?: number)`
Records HTTP requests with intelligent level selection based on status codes.

```typescript
// Successful request
logger.http('POST', '/api/v1/onboarding', 201, 150);
// 🔵 [15 de julio de 2025, 14:30:25] INFO  POST /api/v1/onboarding
//    { statusCode: 201, responseTime: '150ms' }

// Client error (4xx) - automatically logged as ERROR
logger.http('GET', '/api/v1/user/nonexistent', 404, 25);
// 🔴 [15 de julio de 2025, 14:30:25] ERROR GET /api/v1/user/nonexistent
//    { statusCode: 404, responseTime: '25ms' }

// Server error (5xx) - automatically logged as ERROR
logger.http('POST', '/api/v1/webhook', 500, 2000);
// 🔴 [15 de julio de 2025, 14:30:25] ERROR POST /api/v1/webhook
//    { statusCode: 500, responseTime: '2000ms' }

// Redirects (3xx) - automatically logged as WARN
logger.http('GET', '/api/v1/redirect', 301, 10);
// 🟡 [15 de julio de 2025, 14:30:25] WARN  GET /api/v1/redirect
//    { statusCode: 301, responseTime: '10ms' }
```

### `logger.database(operation: string, collection?: string, duration?: number)`
Records database operations with performance metrics.

```typescript
logger.database('find', 'onboarding', 45);
// ⚪ [15 de julio de 2025, 14:30:25] DEBUG 📊 Database operation: find
//    { collection: 'onboarding', duration: '45ms' }

// Complex database operation
logger.database('aggregation', 'user_analytics', 1250);
// ⚪ [15 de julio de 2025, 14:30:25] DEBUG 📊 Database operation: aggregation
//    { collection: 'user_analytics', duration: '1250ms' }
```

### `logger.external(service: string, endpoint: string, statusCode?: number, duration?: number)`
Records external API calls with service identification.

```typescript
logger.external('Metamap', '/sessions', 200, 320);
// 🔵 [15 de julio de 2025, 14:30:25] INFO  🌐 External call to Metamap
//    { endpoint: '/sessions', statusCode: 200, duration: '320ms' }

// Failed external call
logger.external('PaymentGateway', '/validate-card', 503, 5000);
// 🔴 [15 de julio de 2025, 14:30:25] ERROR 🌐 External call to PaymentGateway
//    { endpoint: '/validate-card', statusCode: 503, duration: '5000ms' }
```

## 📋 Type Definitions & Interfaces

### LogData Interface

```typescript
interface LogData {
  message: string;           // Primary message (required)
  [key: string]: unknown;    // Additional properties (optional)
}

// Extended usage examples
type ExtendedLogData = LogData & {
  userId?: string;
  sessionId?: string;
  requestId?: string;
  correlationId?: string;
  performance?: {
    duration: number;
    memory: number;
    cpu: number;
  };
};
```

### LogLevel Type

```typescript
type LogLevel = 'error' | 'warn' | 'info' | 'debug';

// Usage in conditional logging
const logLevel: LogLevel = process.env.LOG_LEVEL as LogLevel || 'info';
```

### Practical Examples

```typescript
// Basic logging
logger.info({ message: 'User authenticated successfully' });

// Rich context logging
logger.error({
  message: 'KYC validation failed',
  userId: '12345',
  kycProvider: 'Metamap',
  errorCode: 'INVALID_DOCUMENT',
  attempts: 3,
  lastAttempt: new Date().toISOString(),
  documentType: 'passport',
  rejectionReason: 'Poor image quality'
});

// Complex object logging
logger.debug({
  message: 'Webhook payload received',
  webhook: 'metamap',
  source: req.ip,
  headers: {
    'content-type': req.get('Content-Type'),
    'user-agent': req.get('User-Agent'),
    'x-forwarded-for': req.get('X-Forwarded-For')
  },
  payload: {
    id: 'kyc_123',
    status: 'approved',
    documents: ['passport', 'selfie'],
    metadata: {
      processingTime: 45000,
      confidence: 0.95,
      biometric: true
    }
  }
});

// Performance monitoring
logger.info({
  message: 'API endpoint performance metrics',
  endpoint: '/api/v1/onboarding',
  method: 'POST',
  responseTime: 245,
  memoryUsage: process.memoryUsage(),
  activeConnections: server.connections,
  timestamp: Date.now()
});
```

## 🎨 Output Format & Structure

### Log Entry Structure

```
{emoji} [{timestamp}] {LEVEL} {message}
{additional_data} (if present, formatted as JSON)
```

### Real-world Examples

```bash
# Successful operation
� [15 de julio de 2025, 14:30:25] INFO  Onboarding process completed successfully
{
  userId: 'usr_7f8e9d2c1b0a',
  sessionId: 'sess_abc123def456',
  kycStatus: 'approved',
  documents: ['passport', 'utility_bill', 'selfie'],
  processingTime: '2m 34s',
  provider: 'Metamap',
  riskScore: 'low'
}

# Error with full context
🔴 [15 de julio de 2025, 14:30:25] ERROR Failed to process Metamap webhook
{
  webhookId: 'wh_123456789',
  kycId: 'kyc_987654321',
  error: 'Invalid signature',
  expectedSignature: 'sha256=...',
  receivedSignature: 'sha256=...',
  payloadSize: 2048,
  retryCount: 3,
  willRetry: false
}

# Warning with actionable information
🟡 [15 de julio de 2025, 14:30:25] WARN  MongoDB connection pool approaching limits
{
  currentConnections: 8,
  maxConnections: 10,
  queuedRequests: 15,
  avgResponseTime: '150ms',
  action: 'consider_scaling_database'
}
```

## 🏗️ Middleware Integration

### Error Handler Integration

```typescript
import { logger } from '../shared/utils';
import type { NextFunction, Request, Response } from 'express';

export const errorHandler = (
  err: Error, 
  req: Request, 
  res: Response, 
  _next: NextFunction
) => {
  // Enhanced error logging with full context
  logger.error({
    message: 'Application error occurred',
    error: {
      name: err.name,
      message: err.message,
      stack: err.stack
    },
    request: {
      method: req.method,
      url: req.url,
      headers: req.headers,
      body: req.body,
      params: req.params,
      query: req.query
    },
    user: {
      id: req.user?.id,
      email: req.user?.email,
      ip: req.ip,
      userAgent: req.get('User-Agent')
    },
    statusCode: (err as any).statusCode || 500,
    timestamp: new Date().toISOString()
  });
  
  const statusCode = (err as any).statusCode || 500;
  res.status(statusCode).json({
    error: 'Internal Server Error',
    message: process.env.NODE_ENV === 'development' ? err.message : 'Something went wrong',
    requestId: req.id // If you have request ID middleware
  });
};
```

### Advanced Request Logger Middleware

```typescript
import { logger } from '../shared/utils';
import type { NextFunction, Request, Response } from 'express';

export const requestLogger = (req: Request, res: Response, next: NextFunction): void => {
  const start = Date.now();
  const requestId = crypto.randomUUID();
  
  // Add request ID for correlation
  req.id = requestId;
  
  // Skip health check endpoints to reduce noise
  const skipLogging = ['/api/v1/health', '/favicon.ico', '/robots.txt'].includes(req.url);
  
  if (!skipLogging) {
    // Log incoming request
    logger.info({
      message: 'Incoming request',
      request: {
        id: requestId,
        method: req.method,
        url: req.url,
        userAgent: req.get('User-Agent'),
        ip: req.ip,
        contentType: req.get('Content-Type'),
        contentLength: req.get('Content-Length')
      }
    });

    // Log response when finished
    res.on('finish', () => {
      const duration = Date.now() - start;
      const level = res.statusCode >= 400 ? 'error' : res.statusCode >= 300 ? 'warn' : 'info';
      
      logger[level]({
        message: 'Request completed',
        request: {
          id: requestId,
          method: req.method,
          url: req.url
        },
        response: {
          statusCode: res.statusCode,
          contentLength: res.get('Content-Length'),
          duration: `${duration}ms`
        },
        performance: {
          responseTime: duration,
          slow: duration > 1000 // Flag slow requests
        }
      });
    });

    // Log request timeout/abort
    res.on('close', () => {
      if (!res.headersSent) {
        logger.warn({
          message: 'Request aborted',
          requestId,
          method: req.method,
          url: req.url,
          duration: `${Date.now() - start}ms`
        });
      }
    });
  }

  next();
};
```

### Authentication Middleware with Logging

```typescript
export const authMiddleware = (req: Request, res: Response, next: NextFunction) => {
  const token = req.headers.authorization?.replace('Bearer ', '');
  
  if (!token) {
    logger.warn({
      message: 'Authentication failed - missing token',
      request: {
        method: req.method,
        url: req.url,
        ip: req.ip,
        userAgent: req.get('User-Agent')
      }
    });
    return res.status(401).json({ error: 'Access denied' });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET!);
    req.user = decoded;
    
    logger.debug({
      message: 'User authenticated successfully',
      userId: req.user.id,
      requestId: req.id
    });
    
    next();
  } catch (error) {
    logger.error({
      message: 'Authentication failed - invalid token',
      error: error.message,
      token: token.substring(0, 20) + '...', // Log partial token for debugging
      request: {
        method: req.method,
        url: req.url,
        ip: req.ip
      }
    });
    
    res.status(401).json({ error: 'Invalid token' });
  }
};
```

## 🧪 Testing & Mocking

### Logger Mocking for Unit Tests

```typescript
// vitest.config.ts or test setup
import { vi } from 'vitest';

const mockLogger = {
  error: vi.fn(),
  warn: vi.fn(),
  info: vi.fn(),
  debug: vi.fn(),
  server: vi.fn(),
  http: vi.fn(),
  database: vi.fn(),
  external: vi.fn(),
};

vi.mock('../shared/utils/logger', () => ({
  logger: mockLogger
}));

// Export for use in tests
export { mockLogger };
```

### Advanced Test Examples

```typescript
import { describe, it, expect, beforeEach } from 'vitest';
import { logger } from '../shared/utils';
import { startOnboardingService } from '../onboarding/services';
import { mockLogger } from '../__mocks__/logger';

describe('Onboarding Service Logging', () => {
  beforeEach(() => {
    // Clear all mock calls before each test
    Object.values(mockLogger).forEach(mock => mock.mockClear());
  });

  it('should log successful onboarding initiation', async () => {
    const payload = { userId: '123', email: 'test@test.com' };
    
    await startOnboardingService(payload);
    
    expect(mockLogger.info).toHaveBeenCalledWith({
      message: 'Onboarding process started successfully',
      userId: '123',
      email: 'test@test.com',
      sessionId: expect.any(String)
    });
  });

  it('should log errors with proper context', async () => {
    const invalidPayload = { userId: '', email: 'invalid-email' };
    
    await expect(startOnboardingService(invalidPayload)).rejects.toThrow();
    
    expect(mockLogger.error).toHaveBeenCalledWith({
      message: 'Onboarding validation failed',
      errors: expect.arrayContaining([
        expect.objectContaining({ field: 'userId' }),
        expect.objectContaining({ field: 'email' })
      ]),
      payload: invalidPayload
    });
  });

  it('should log performance metrics for slow operations', async () => {
    // Mock a slow operation
    vi.spyOn(Date, 'now')
      .mockReturnValueOnce(1000)  // Start time
      .mockReturnValueOnce(5000); // End time (4 seconds later)
    
    await startOnboardingService({ userId: '123', email: 'test@test.com' });
    
    expect(mockLogger.warn).toHaveBeenCalledWith({
      message: 'Slow onboarding operation detected',
      duration: 4000,
      threshold: 3000,
      userId: '123'
    });
  });
});
```

### Integration Test Logging

```typescript
import { describe, it, expect } from 'vitest';
import request from 'supertest';
import { app } from '../app';

describe('API Integration Logging', () => {
  it('should log successful API requests', async () => {
    const response = await request(app)
      .post('/api/v1/onboarding')
      .send({ userId: '123', email: 'test@test.com' })
      .expect(201);

    // Verify the logger was called (you can check actual log output in CI/CD)
    expect(response.status).toBe(201);
  });

  it('should log failed requests with error context', async () => {
    await request(app)
      .post('/api/v1/onboarding')
      .send({ invalid: 'data' })
      .expect(400);

    // Logger should have captured the validation error
  });
});
```

### Test Utilities for Log Verification

```typescript
// test/utils/logCapture.ts
export class LogCapture {
  private logs: Array<{ level: string; data: any }> = [];

  capture() {
    const originalMethods = {
      error: console.error,
      warn: console.warn,
      info: console.info,
      debug: console.debug
    };

    // Intercept console methods
    console.error = (...args) => this.logs.push({ level: 'error', data: args });
    console.warn = (...args) => this.logs.push({ level: 'warn', data: args });
    console.info = (...args) => this.logs.push({ level: 'info', data: args });
    console.debug = (...args) => this.logs.push({ level: 'debug', data: args });

    return () => {
      // Restore original methods
      Object.assign(console, originalMethods);
    };
  }

  getLogs(level?: string) {
    return level ? this.logs.filter(log => log.level === level) : this.logs;
  }

  findLog(predicate: (log: any) => boolean) {
    return this.logs.find(predicate);
  }

  clear() {
    this.logs = [];
  }
}
```

## 🎯 Best Practices & Guidelines

### ✅ Recommended Practices

```typescript
// ✅ Descriptive and specific messages
logger.error({
  message: 'Failed to validate document with Metamap',
  documentType: 'passport',
  validationErrors: errors,
  userId: 'usr_123',
  attemptNumber: 2,
  kycProvider: 'Metamap'
});

// ✅ Include relevant context for debugging
logger.info({
  message: 'KYC verification completed successfully',
  userId: user.id,
  kycProvider: 'Metamap',
  verificationId: result.id,
  documentsVerified: result.documents,
  processingDuration: timer.end(),
  riskAssessment: result.riskScore
});

// ✅ Use appropriate log levels
logger.warn({
  message: 'KYC attempt limit approaching threshold',
  userId: user.id,
  currentAttempts: 2,
  maxAttempts: 3,
  timeWindow: '24h',
  action: 'notify_user'
});

// ✅ Structured data for better analysis
logger.debug({
  message: 'Database query performance metrics',
  operation: 'findUserByEmail',
  collection: 'users',
  indexUsed: 'email_1',
  executionTime: 45,
  recordsExamined: 1,
  recordsReturned: 1,
  planSummary: 'IXSCAN { email: 1 }'
});

// ✅ Correlation IDs for request tracing
logger.info({
  message: 'Processing webhook from external service',
  correlationId: req.headers['x-correlation-id'],
  requestId: req.id,
  source: 'metamap',
  eventType: 'kyc_completed'
});
```

### ❌ Practices to Avoid

```typescript
// ❌ Generic, unhelpful messages
logger.error({ message: 'Error' });
logger.info({ message: 'Success' });

// ❌ Sensitive information (PII, credentials, tokens)
logger.info({
  message: 'User authenticated',
  password: user.password,        // ❌ Never log passwords
  socialSecurityNumber: user.ssn, // ❌ Never log SSNs
  creditCard: user.cardNumber,    // ❌ Never log financial data
  apiKey: process.env.SECRET_KEY  // ❌ Never log secrets
});

// ❌ Wrong log levels
logger.error({ message: 'User created successfully' }); // ❌ Should be info
logger.info({ message: 'Database connection failed' }); // ❌ Should be error
logger.debug({ message: 'System is shutting down' });   // ❌ Should be warn/info

// ❌ Excessive logging in hot paths
for (const item of millionItems) {
  logger.debug({ message: 'Processing item', item }); // ❌ Too verbose
}

// ❌ Logging inside catch blocks without re-throwing
try {
  await riskyOperation();
} catch (error) {
  logger.error({ message: 'Operation failed', error });
  // ❌ Error is swallowed, should re-throw or handle properly
}
```

### 🔐 Security Considerations

```typescript
// ✅ Safe logging with data sanitization
const sanitizeUserData = (user: any) => ({
  id: user.id,
  email: user.email?.replace(/(.{2}).*(@.*)/, '$1***$2'), // Partially mask email
  role: user.role,
  lastLogin: user.lastLogin
  // Exclude sensitive fields
});

logger.info({
  message: 'User profile updated',
  user: sanitizeUserData(user),
  updatedFields: ['email', 'phone'], // Log what changed, not the values
  requestId: req.id
});

// ✅ IP address logging with privacy considerations
logger.warn({
  message: 'Multiple failed login attempts detected',
  ipAddress: req.ip?.replace(/\.\d+$/, '.xxx'), // Partially mask IP
  attempts: failedAttempts,
  timeWindow: '5m',
  action: 'temporary_lockout'
});
```

### 📊 Performance Optimization

```typescript
// ✅ Conditional logging for expensive operations
if (process.env.LOG_LEVEL === 'debug') {
  logger.debug({
    message: 'Detailed request analysis',
    headers: req.headers,
    body: JSON.stringify(req.body, null, 2),
    memory: process.memoryUsage(),
    timestamp: process.hrtime.bigint()
  });
}

// ✅ Lazy evaluation for complex log data
logger.info({
  message: 'Operation completed',
  get expensiveData() {
    return computeExpensiveMetrics(); // Only computed if log level allows
  }
});

// ✅ Batching logs for high-throughput scenarios
class LogBuffer {
  private buffer: any[] = [];
  private flushInterval: NodeJS.Timeout;

  constructor(private flushSize = 100, private flushTime = 5000) {
    this.flushInterval = setInterval(() => this.flush(), flushTime);
  }

  add(logEntry: any) {
    this.buffer.push(logEntry);
    if (this.buffer.length >= this.flushSize) {
      this.flush();
    }
  }

  private flush() {
    if (this.buffer.length > 0) {
      logger.info({
        message: 'Batch log entries',
        count: this.buffer.length,
        entries: this.buffer
      });
      this.buffer = [];
    }
  }
}
```

## 🔧 Advanced Configuration & Customization

### Environment-based Configuration

```typescript
// src/shared/config/logger.config.ts
interface LoggerConfig {
  level: LogLevel;
  format: 'json' | 'console';
  timezone: string;
  enableColors: boolean;
  enableEmojis: boolean;
  enableStackTrace: boolean;
}

export const loggerConfig: LoggerConfig = {
  level: (process.env.LOG_LEVEL as LogLevel) || 'info',
  format: process.env.LOG_FORMAT === 'json' ? 'json' : 'console',
  timezone: process.env.TIMEZONE || 'America/Mexico_City',
  enableColors: process.env.ENABLE_LOG_COLORS !== 'false',
  enableEmojis: process.env.ENABLE_LOG_EMOJIS !== 'false',
  enableStackTrace: process.env.NODE_ENV === 'development'
};

// Environment-specific configurations
const configs = {
  development: {
    ...loggerConfig,
    level: 'debug' as LogLevel,
    enableColors: true,
    enableEmojis: true
  },
  testing: {
    ...loggerConfig,
    level: 'error' as LogLevel,
    format: 'json' as const,
    enableColors: false,
    enableEmojis: false
  },
  production: {
    ...loggerConfig,
    level: 'info' as LogLevel,
    format: 'json' as const,
    enableColors: false,
    enableEmojis: false,
    enableStackTrace: false
  }
};

export const getLoggerConfig = () => {
  const env = process.env.NODE_ENV || 'development';
  return configs[env as keyof typeof configs] || configs.development;
};
```

### Extended Logger Class

```typescript
// src/shared/utils/extendedLogger.ts
import { logger as baseLogger } from './logger';

class ExtendedLogger {
  // KYC-specific logging
  kyc(operation: string, data: {
    userId: string;
    provider: string;
    documentType?: string;
    status?: string;
    duration?: number;
  }) {
    baseLogger.info({
      message: `KYC ${operation}`,
      category: 'kyc',
      ...data,
      timestamp: new Date().toISOString()
    });
  }

  // Security-related logging
  security(event: string, data: {
    userId?: string;
    ipAddress?: string;
    userAgent?: string;
    severity: 'low' | 'medium' | 'high' | 'critical';
  }) {
    const level = data.severity === 'critical' ? 'error' 
                : data.severity === 'high' ? 'error'
                : data.severity === 'medium' ? 'warn' 
                : 'info';

    baseLogger[level]({
      message: `Security event: ${event}`,
      category: 'security',
      ...data,
      timestamp: new Date().toISOString()
    });
  }

  // Business metrics logging
  metric(name: string, value: number, tags: Record<string, string> = {}) {
    baseLogger.info({
      message: `Metric recorded: ${name}`,
      category: 'metric',
      metric: {
        name,
        value,
        tags,
        timestamp: Date.now()
      }
    });
  }

  // Audit trail logging
  audit(action: string, data: {
    userId: string;
    resource: string;
    resourceId?: string;
    changes?: Record<string, any>;
    ipAddress?: string;
  }) {
    baseLogger.info({
      message: `Audit: ${action}`,
      category: 'audit',
      ...data,
      timestamp: new Date().toISOString()
    });
  }
}

export const extendedLogger = new ExtendedLogger();
```

### Custom Log Formatters

```typescript
// src/shared/utils/logFormatters.ts
export class LogFormatter {
  static json(logEntry: any): string {
    return JSON.stringify({
      timestamp: logEntry.timestamp,
      level: logEntry.level.toUpperCase(),
      message: logEntry.message,
      ...(logEntry.data && { data: logEntry.data }),
      service: 'r365-onboarding',
      version: process.env.npm_package_version,
      environment: process.env.NODE_ENV,
      hostname: process.env.HOSTNAME || 'unknown'
    });
  }

  static structured(logEntry: any): string {
    const prefix = `[${logEntry.timestamp}] ${logEntry.level.toUpperCase().padEnd(5)}`;
    const message = logEntry.message;
    const data = logEntry.data ? `\n${JSON.stringify(logEntry.data, null, 2)}` : '';
    
    return `${prefix} ${message}${data}`;
  }

  static compact(logEntry: any): string {
    const time = new Date(logEntry.timestamp).toLocaleTimeString();
    const level = logEntry.level.charAt(0).toUpperCase();
    const message = logEntry.message;
    
    return `${time} ${level} ${message}`;
  }
}
```

## 📈 Observability & Monitoring Integration

### Structured Logging for External Systems

```typescript
// Integration with external logging services (DataDog, Splunk, etc.)
export const observabilityLogger = {
  // APM trace correlation
  trace(traceId: string, spanId: string, data: any) {
    logger.debug({
      message: 'Trace event',
      traceId,
      spanId,
      ...data,
      '@timestamp': new Date().toISOString()
    });
  },

  // Business KPIs
  kpi(metric: string, value: number, dimensions: Record<string, string>) {
    logger.info({
      message: 'KPI metric',
      metric,
      value,
      dimensions,
      '@timestamp': new Date().toISOString(),
      '@type': 'kpi'
    });
  },

  // Error tracking with correlation
  errorWithContext(error: Error, context: Record<string, any>) {
    logger.error({
      message: 'Application error with full context',
      error: {
        name: error.name,
        message: error.message,
        stack: error.stack,
        code: (error as any).code
      },
      context,
      '@timestamp': new Date().toISOString(),
      '@type': 'error',
      severity: 'error'
    });
  }
};
```

### Health Check & Diagnostics

```typescript
// src/shared/utils/diagnostics.ts
export class DiagnosticsLogger {
  static logSystemHealth() {
    const memUsage = process.memoryUsage();
    const cpuUsage = process.cpuUsage();
    
    logger.info({
      message: 'System health check',
      category: 'diagnostics',
      memory: {
        rss: `${Math.round(memUsage.rss / 1024 / 1024)}MB`,
        heapTotal: `${Math.round(memUsage.heapTotal / 1024 / 1024)}MB`,
        heapUsed: `${Math.round(memUsage.heapUsed / 1024 / 1024)}MB`,
        external: `${Math.round(memUsage.external / 1024 / 1024)}MB`
      },
      cpu: {
        user: cpuUsage.user,
        system: cpuUsage.system
      },
      uptime: process.uptime(),
      nodeVersion: process.version,
      timestamp: new Date().toISOString()
    });
  }

  static logDatabaseHealth(connectionStatus: 'connected' | 'disconnected' | 'error') {
    const level = connectionStatus === 'error' ? 'error' 
                : connectionStatus === 'disconnected' ? 'warn' 
                : 'info';

    logger[level]({
      message: 'Database health status',
      category: 'diagnostics',
      status: connectionStatus,
      timestamp: new Date().toISOString()
    });
  }
}
```

## 🚀 Performance Optimization Strategies

### Log Level Filtering

```typescript
// Compile-time log elimination for production
const isProduction = process.env.NODE_ENV === 'production';
const logLevel = process.env.LOG_LEVEL || 'info';

export const conditionalLogger = {
  debug: (data: LogData) => {
    if (!isProduction && ['debug'].includes(logLevel)) {
      logger.debug(data);
    }
  },
  info: (data: LogData) => {
    if (['debug', 'info'].includes(logLevel)) {
      logger.info(data);
    }
  },
  warn: (data: LogData) => {
    if (['debug', 'info', 'warn'].includes(logLevel)) {
      logger.warn(data);
    }
  },
  error: (data: LogData) => {
    logger.error(data); // Always log errors
  }
};
```

### Asynchronous Logging

```typescript
// Non-blocking log processing
import { Worker } from 'worker_threads';

class AsyncLogger {
  private worker: Worker;
  
  constructor() {
    this.worker = new Worker(`
      const { parentPort } = require('worker_threads');
      parentPort.on('message', (logData) => {
        // Process log in worker thread
        console.log(JSON.stringify(logData));
      });
    `, { eval: true });
  }

  log(data: any) {
    this.worker.postMessage(data);
  }
}
```

## 🐳 Docker & Container Logging

### Docker-optimized Logging

```typescript
// src/shared/utils/containerLogger.ts
const isContainer = process.env.DOCKER_CONTAINER === 'true';

export const containerLogger = {
  // JSON format for container log aggregation
  formatForContainer(logEntry: any): string {
    if (isContainer) {
      return JSON.stringify({
        '@timestamp': logEntry.timestamp,
        '@version': '1',
        level: logEntry.level.toUpperCase(),
        logger_name: 'r365-onboarding',
        message: logEntry.message,
        ...logEntry.data,
        container: {
          id: process.env.HOSTNAME,
          image: process.env.DOCKER_IMAGE,
          name: process.env.CONTAINER_NAME
        }
      });
    }
    return logEntry.formatted;
  },

  // Health check logging for container orchestration
  healthCheck(status: 'healthy' | 'unhealthy', checks: Record<string, boolean>) {
    logger.info({
      message: 'Container health check',
      status,
      checks,
      container_id: process.env.HOSTNAME,
      timestamp: new Date().toISOString()
    });
  }
};
```

### Kubernetes Integration

```yaml
# Example logging configuration for Kubernetes
apiVersion: v1
kind: ConfigMap
metadata:
  name: onboarding-logger-config
data:
  LOG_LEVEL: "info"
  LOG_FORMAT: "json"
  ENABLE_LOG_COLORS: "false"
  ENABLE_LOG_EMOJIS: "false"
  STRUCTURED_LOGGING: "true"
```

## � Migration & Upgrade Guide

### Migrating from Console.log

```typescript
// Before (anti-pattern)
console.log('User created:', user.id);
console.error('Error occurred:', error);

// After (recommended)
logger.info({
  message: 'User created successfully',
  userId: user.id,
  timestamp: new Date().toISOString()
});

logger.error({
  message: 'User creation failed',
  error: error.message,
  stack: error.stack,
  userId: user.id
});
```

### Upgrading Logger Configuration

```typescript
// Version 1.x configuration
const oldLogger = {
  level: 'info',
  format: 'simple'
};

// Version 2.x configuration (enhanced)
const newLogger = {
  level: process.env.LOG_LEVEL || 'info',
  format: process.env.LOG_FORMAT || 'structured',
  enableMetrics: true,
  enableTracing: true,
  correlationId: true,
  sanitization: {
    enabled: true,
    fields: ['password', 'token', 'ssn', 'creditCard']
  }
};
```

## 🔍 Troubleshooting Guide

### Common Issues & Solutions

#### Issue: Logs not appearing in production
```typescript
// Solution: Check log level configuration
const debugConfig = {
  LOG_LEVEL: process.env.LOG_LEVEL || 'info',
  NODE_ENV: process.env.NODE_ENV,
  actualLogLevel: logger.level // Add this to see current level
};

logger.error({
  message: 'Logger configuration debug',
  config: debugConfig
});
```

#### Issue: Too many logs affecting performance
```typescript
// Solution: Implement log sampling
class LogSampler {
  private counter = 0;
  private sampleRate: number;

  constructor(sampleRate = 0.1) { // 10% sampling
    this.sampleRate = sampleRate;
  }

  shouldLog(): boolean {
    this.counter++;
    return (this.counter % Math.floor(1 / this.sampleRate)) === 0;
  }
}

const sampler = new LogSampler(0.1);

// Only log 10% of debug messages in production
if (process.env.NODE_ENV !== 'production' || sampler.shouldLog()) {
  logger.debug({ message: 'Detailed debug info' });
}
```

#### Issue: Log format inconsistency
```typescript
// Solution: Use consistent log structure
interface StandardLogEntry {
  message: string;
  timestamp: string;
  level: LogLevel;
  service: string;
  version: string;
  environment: string;
  correlationId?: string;
  userId?: string;
  requestId?: string;
}

const createStandardLog = (
  level: LogLevel, 
  message: string, 
  additional: Record<string, any> = {}
): StandardLogEntry => ({
  message,
  timestamp: new Date().toISOString(),
  level,
  service: 'r365-onboarding',
  version: process.env.npm_package_version || '1.0.0',
  environment: process.env.NODE_ENV || 'development',
  ...additional
});
```

## 📖 API Reference Summary

### Quick Reference Table

| Method | Level | Use Case | Example |
|--------|-------|----------|---------|
| `logger.error()` | ERROR | System failures, exceptions | Database connection failed |
| `logger.warn()` | WARN | Warnings, degraded performance | Rate limit approaching |
| `logger.info()` | INFO | Business events, user actions | User registered successfully |
| `logger.debug()` | DEBUG | Development debugging | Request validation details |
| `logger.server()` | INFO | Server lifecycle | Server started on port 3000 |
| `logger.http()` | Dynamic | HTTP requests | POST /api/v1/users 201 150ms |
| `logger.database()` | DEBUG | Database operations | find users collection 45ms |
| `logger.external()` | Dynamic | External API calls | Metamap API call 200 320ms |

### TypeScript Definitions

```typescript
// Complete type definitions
export interface LogData {
  message: string;
  [key: string]: unknown;
}

export type LogLevel = 'error' | 'warn' | 'info' | 'debug';

export interface Logger {
  error(data: LogData): void;
  warn(data: LogData): void;
  info(data: LogData): void;
  debug(data: LogData): void;
  server(port: number | string, environment?: string): void;
  http(method: string, url: string, statusCode: number, responseTime?: number): void;
  database(operation: string, collection?: string, duration?: number): void;
  external(service: string, endpoint: string, statusCode?: number, duration?: number): void;
}
```

---

## 🎉 Conclusion

The Remesas365 Logger provides a robust, scalable, and developer-friendly logging solution that grows with your application. By following the patterns and practices outlined in this documentation, you'll have comprehensive observability into your Onboarding KYC microservice.

### Key Benefits
- 🎯 **Consistent Format**: Standardized across all services
- 🔍 **Rich Context**: Detailed information for debugging
- 🚀 **Performance Optimized**: Non-blocking and efficient
- 🔒 **Security Aware**: Built-in PII protection
- 🐳 **Container Ready**: Docker and Kubernetes compatible
- 📊 **Observable**: Integration-ready for monitoring systems

## 📞 Support & Contributing

**Documentation Version**: 2.0.0  
**Last Updated**: July 18, 2025  
**Maintained by**: Remesas365 Backend Team

For questions, improvements, or bug reports:
- Create an issue in the project repository
- Contact the backend team via Slack: `#r365-backend`
- Email: backend-team@remesas365.com

**Contributing**: Please read our [Contributing Guidelines](../CONTRIBUTING.md) before submitting changes.

---

*Built with ❤️ for the Remesas365 ecosystem*
