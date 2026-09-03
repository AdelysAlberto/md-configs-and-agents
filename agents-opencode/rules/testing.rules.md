
# Testing Standards

## Unit Test Guidelines

All unit tests must follow these conventions:

- **Test Runner**: Jest or Vitest (configured per project).
- **Test Location**: `__tests__` folders or `*..test.ts` / `*.spec.ts` files adjacent to source.
- **Assertion Library**: Expect (`jest`/`vitest`).
- **Result Pattern Testing**: Test services that return `{ ok, data, error }` by asserting both success and failure paths.

### Unit Test Template

```typescript
import { getUser } from 'src/services/users';
import { ApiResult } from 'src/types';

describe('getUser', () => {
  it('should return ok with data on success', async () => {
    const result = await getUser('123');
    expect(result.ok).toBe(true);
    expect(result.data.id).toBe('123');
  });

  it('should return error on failure', async () => {
    const result = await getUser('invalid');
    expect(result.ok).toBe(false);
    expect(typeof result.error).toBe('string');
  });
});
```

### Mocking Strategy

- Use **MSW** (Mock Service Worker) for integration test mocks.
- Mock API responses at the network intercept level, not manual object mocking.
- Never mock internal implementation details; mock only public APIs.

## Integration Test Guidelines

- **Critical Flows Only**: Auth, checkout, onboarding, and any user journey that fails silently must have integration tests.
- **MSW + React Testing Library**: Use MSW handlers to mock API responses within RTL tests.
- **Accessibility Tests**: Include `a11y` tests for critical components using `@testing-library/react`.

### Integration Test Template

```typescript
import { render } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Provider } from 'msw-react';
import { setupServer } from 'msw';
import { rest } from 'msw';

const handlers = [
  rest.get('/api/users/:id', (req, res, ctx) => {
    return res(ctx.json({ id: req.params.id, name: 'Test User' }));
  }),
];

const server = setupServer(...handlers);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

describe('User Profile Page', () => {
  it('should display user data successfully', async () => {
    const user = await render(<UserProfile userId="123" />);
    const getByText = user.getByText;
    await userEvent.type(getByText('Loading...'), '');
    expect(getByText('Test User')).toBeInTheDocument();
  });
});
```