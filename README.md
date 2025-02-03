# @lukaskj/bun-base-template

Template for Bun projects.

Commands:
- `bun run dev`: Run project with `src/index.ts` entrypoint;
- `bun run lint`: Lint source files with `tsc` and `biome`;
- `bun run format`: Format code using `biome` and the configuration file at `biome.json`;
- `bun run test`: Run test files from `test` folder.
- `bun run test:cov`: Run tests with coverage.

Dependencies:
- [husky](https://typicode.github.io/husky/)
- [@biomejs/biome](https://github.com/biomejs/biome)
- [@faker-js/faker](https://fakerjs.dev/)

