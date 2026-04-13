Run the full verification suite for this project. Execute the steps in order and report results.

1. **Type check**: `{{TYPE_CHECK_COMMAND}}`
2. **Build**: `{{BUILD_COMMAND}}`
3. **Unit tests**: `{{TEST_COMMAND}}`

<!-- Example for Astro projects:
1. Type check: cd $CLAUDE_PROJECT_DIR && npx astro check
2. Build: cd $CLAUDE_PROJECT_DIR && npx astro build
3. Unit tests: cd $CLAUDE_PROJECT_DIR && npm test

Example for pnpm monorepos:
1-3 combined: cd $CLAUDE_PROJECT_DIR && pnpm verify
-->

Report the result of each step. If any step fails, stop and report the failure with details. Do not skip failing steps.
