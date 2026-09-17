---
name: testing-strategy
description: Testing standards, unit testing for services and hooks, integration testing with MSW and React Testing Library, edge case diagnosis, and test-driven design. Use when writing tests or planning QA coverage.
license: MIT
compatibility: opencode
metadata:
  domain: testing
  frameworks: vitest,msw,rtl
---

# Testing Strategy & QA Diagnostic Standards

Comprehensive testing guide for reliable unit and integration testing using **Vitest**, **React Testing Library (RTL)**, and **Mock Service Worker (MSW)**.

## 1. Core Testing Invariants

1. **Test Boundaries, Not Implementation Details**: Assert on user-visible outputs, API contract results, and DOM accessibility roles. Never assert on internal private state or component internals.
2. **Result Pattern Testing**: Every service returning `{ success: true, data } | { success: false, error }` must have test coverage for BOTH the happy path and all classified error branches.
3. **Network Interception with MSW**: For integration tests, mock network responses at the HTTP transport layer using MSW. Never mock `fetch` or `axios` with ad-hoc `vi.fn()` hacks.
4. **Fast and Deterministic**: Unit tests must execute in memory without filesystem or actual network dependency. Integration tests must be isolated and reset state between runs.

---

## 2. Unit Testing Services (Result Pattern)

```typescript
import { describe, it, expect, vi, beforeEach } from 'vitest';
import { fetchUserOrders } from './orders.service';

describe('fetchUserOrders', () => {
  it('should return Ok with orders on successful API response', async () => {
    const result = await fetchUserOrders('user-123');

    expect(result.success).toBe(true);
    if (result.success) {
      expect(result.data).toHaveLength(2);
      expect(result.data[0].id).toBe('ord-1');
    }
  });

  it('should return Err with NOT_FOUND code when user has no orders', async () => {
    const result = await fetchUserOrders('invalid-user');

    expect(result.success).toBe(false);
    if (!result.success) {
      expect(result.error.code).toBe('NOT_FOUND');
    }
  });
});
```

---

## 3. Integration Testing with MSW and React Testing Library

```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { http, HttpResponse } from 'msw';
import { setupServer } from 'msw/node';
import { describe, it, expect, beforeAll, afterEach, afterAll } from 'vitest';
import { OrderHistory } from './OrderHistory';

const handlers = [
  http.get('/api/orders', () => {
    return HttpResponse.json([
      { id: '1', item: 'Mechanical Keyboard', status: 'delivered' },
    ]);
  }),
];

const server = setupServer(...handlers);

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

describe('<OrderHistory />', () => {
  it('should display the loading state and then render the orders list', async () => {
    render(<OrderHistory />);

    expect(screen.getByRole('status')).toHaveTextContent(/loading/i);

    const orderItem = await screen.findByText('Mechanical Keyboard');
    expect(orderItem).toBeInTheDocument();
  });

  it('should display an error alert when the network request fails', async () => {
    server.use(
      http.get('/api/orders', () => {
        return new HttpResponse(null, { status: 500 });
      })
    );

    render(<OrderHistory />);

    const alert = await screen.findByRole('alert');
    expect(alert).toHaveTextContent(/failed to load orders/i);
  });
});
```

---

## 4. Hook Testing Guidelines

When testing custom hooks:
```typescript
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

it('should increment count on action', () => {
  const { result } = renderHook(() => useCounter(0));

  act(() => {
    result.current.increment();
  });

  expect(result.current.count).toBe(1);
});
```

---

## 5. Diagnostic Checklist for Critical Flows

Ensure the following critical journeys have 100% test coverage:
- **Authentication**: Login, session expiration, token refresh failure.
- **Data Mutations**: Form validation, submit error handling, optimistic updates rollback.
- **Empty & Edge States**: Empty list placeholders, network timeout fallbacks, missing properties.
