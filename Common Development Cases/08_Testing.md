# Testing

## Unit Testing Components
### Using Jest with Next.js

**Example:**

```tsx
// components/ExampleComponent.tsx
const ExampleComponent: React.FC<{ title: string }> = ({ title }) => (
  <h1>{title}</h1>
);

export default ExampleComponent;

// __tests__/ExampleComponent.test.tsx
import { render, screen } from '@testing-library/react';
import ExampleComponent from '../components/ExampleComponent';

test('renders the correct title', () => {
  render(<ExampleComponent title="Hello, World!" />);
  expect(screen.getByText('Hello, World!')).toBeInTheDocument();
});
```

**Comments:**
- **Component**: Simple component with a `title` prop.
- **Test**: Renders component and checks if the title is displayed.
- **Jest and Testing Library**: Used for unit testing and DOM assertions.



## Integration Testing
### Testing Pages and API Routes

**Example:**

```tsx
// pages/index.tsx
import { GetServerSideProps } from 'next';

const HomePage: React.FC<{ data: string }> = ({ data }) => (
  <div>{data}</div>
);

export const getServerSideProps: GetServerSideProps = async () => {
  // Simulate fetching data
  return { props: { data: 'Server Data' } };
};

export default HomePage;

// __tests__/index.test.tsx
import { render, screen } from '@testing-library/react';
import HomePage from '../pages/index';
import { server } from '../mocks/server'; // Mock server setup
import { rest } from 'msw';

test('renders server data on the homepage', async () => {
  server.use(
    rest.get('/api/data', (req, res, ctx) => {
      return res(ctx.json({ data: 'Server Data' }));
    })
  );
  
  render(<HomePage data="Server Data" />);
  expect(await screen.findByText('Server Data')).toBeInTheDocument();
});
```

**Comments:**
- **Page**: Renders data fetched via `getServerSideProps`.
- **Test**: Simulates server response and checks if data is rendered correctly.
- **MSW (Mock Service Worker)**: Used for mocking API responses in tests.



## End-to-End Testing
### Using Cypress or Playwright

**Example:**

```tsx
// cypress/integration/home.spec.ts
describe('Home Page', () => {
  it('should load and display server data', () => {
    // Visit the home page
    cy.visit('/');

    // Mock server response
    cy.intercept('GET', '/api/data', { body: { data: 'Server Data' } });

    // Assert that the data is displayed
    cy.contains('Server Data').should('be.visible');
  });
});
```

**Comments:**
- **Test File**: Cypress test for Next.js home page.
- **Visit**: Navigates to the home page.
- **Intercept**: Mocks API response.
- **Assert**: Checks if the data is visible on the page.



## Mocking API Requests
### Testing API Routes Independently

**Example:**

```tsx
// __tests__/api/hello.test.ts
import { NextApiRequest, NextApiResponse } from 'next';
import handler from '../../pages/api/hello'; // Adjust the import as necessary
import { createMocks } from 'node-mocks-http';

describe('API Route - /api/hello', () => {
  it('should return a greeting message', async () => {
    const { req, res }: { req: NextApiRequest; res: NextApiResponse } = createMocks({
      method: 'GET',
    });

    await handler(req, res);

    expect(res.statusCode).toBe(200);
    expect(res._getData()).toEqual({ message: 'Hello, World!' });
  });
});
```

**Comments:**
- **Test File**: Unit test for API route.
- **createMocks**: Creates mock `req` and `res` objects.
- **handler**: Calls API route handler.
- **Assertions**: Checks status code and response data.