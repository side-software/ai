Write tests for: $ARGUMENTS

Follow these steps:

1. Read the source file to understand what needs testing.

2. Determine the test type:
   - **Unit test** (pure logic, no DOM): Place alongside or in a `__tests__/` directory
   - **Component test** (React rendering): Needs a testing library (testing-library, enzyme, etc.)
   - **E2E test** (browser workflow): Uses Playwright, Cypress, etc.

3. Follow existing test patterns in the project:
   - Match the test runner (Vitest, Jest, Playwright, etc.)
   - Match the assertion style
   - Match the mocking patterns
   - Match the file naming convention (`.test.ts`, `.spec.ts`, etc.)

4. Run the tests to verify they pass.

5. Run the project's type check command to verify no type errors in test files.

<!-- Customize for your project:
- Specify the test runner and assertion library
- Specify the test file location convention
- Specify mocking patterns (vi.mock, jest.mock, MSW, etc.)
- Specify any test utilities or fixtures the project provides
-->
