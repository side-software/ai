Create a new page. The page route/name is: $ARGUMENTS

Follow these steps:

1. Create the page file at the appropriate path under the project's pages directory. Follow the existing pattern for page files in this project.

2. Create any associated component files the page needs, following the project's page component pattern.

3. Run the project's type check command to verify 0 errors.

4. If the dev server is running, verify the route loads: `curl -s -o /dev/null -w "%{http_code}" http://localhost:{{PORT}}/{{route}}`

<!-- Customize for your project:
- Specify the pages directory (src/pages/, app/, pages/, etc.)
- Specify the page pattern (Astro pages with React islands, Next.js pages, plain React routes, etc.)
- Specify required wrappers (layout components, providers, auth guards)
- Specify the dev server port
-->
