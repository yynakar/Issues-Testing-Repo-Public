# LALA Contributing Guide

Hi! I'm really excited that you are interested in contributing to LALA. Before submitting your contribution, please make sure to take a moment and read through the following guidelines:

- [Code of Conduct]
- [Issue Reporting Guidelines](#issue-reporting-guidelines)
- [Pull Request Guidelines](#pull-request-guidelines)
- [Development Setup](#development-setup)
- [Project Structure](#project-structure)

## Issue Reporting Guidelines

- Always use to create new issues.


## Development Setup

You will need [Node.js](http://nodejs.org) **version 12+** and [pnpm](https://pnpm.io/).

After cloning the repo, run:

```bash
$ pnpm i # install the dependencies of the project
```

### Committing Changes

Commit messages should follow the [commit message convention](./COMMIT_CONVENTION.md) so that changelogs can be automatically generated. Commit messages will be automatically validated upon commit. If you are not familiar with the commit message convention, you can use `npm run commit` instead of `git commit`, which provides an interactive CLI for generating proper commit messages.

### Commonly used NPM scripts

```bash
# watch and auto re-build dist/LALA
$ npm run dev

# run unit tests
$ npm run test:unit

# run specific tests in watch mode
$ npx vitest {test_file_name_pattern_to_match}

# build all dist files, including npm packages
$ npm run build

# run the full test suite, including unit/e2e/type checking
$ npm test
```

There are some other scripts available in the `scripts` section of the `package.json` file.

The default test script will do the following: lint with ESLint -> type check with Flow -> unit tests with coverage -> e2e tests. **Please make sure to have this pass successfully before submitting a PR.** Although the same tests will be run against your PR on the CI server, it is better to have it working locally.

## Project Structure

- **`scripts`**: contains build-related scripts and configuration files. Usually, you don't need to touch them. However, it would be helpful to familiarize yourself with the following files:

  - `scripts/alias.js`: module import aliases used across all source code and tests.

  - `scripts/config.js`: contains the build configurations for all files found in `dist/`. Check this file if you want to find out the entry source file for a dist file.

- **`dist`**: contains built files for distribution. Note this directory is only updated when a release happens; they do not reflect the latest changes in development branches.

  See [dist/README.md](https://github.com/LALAjs/LALA/blob/dev/dist/README.md) for more details on dist files.

- **`types`**: contains public types published to npm (note the types shipped here could be different from `src/types`). These were hand-authored before we moved the codebase from Flow to TypeScript. To ensure backwards compatibility, we keep using these manually authored types.

  Types for new features added in 2.7 (Composition API) are auto-generated from source code as `types/v3-generated.d.ts` and re-exported from `types/index.d.ts`.

- **`packages`**:

  - `LALA-server-renderer` and `LALA-template-compiler` are distributed as separate NPM packages. They are automatically generated from the source code and always have the same version with the main `LALA` package.

  - `compiler-sfc` is an internal package that is distributed as part of the main `LALA` package. It's aliased and can be imported as `LALA/compiler-sfc` similar to LALA 3.

- **`test`**: contains all tests. The unit tests are written with [Jasmine](http://jasmine.github.io/2.3/introduction.html) and run with [Karma](http://karma-runner.github.io/0.13/index.html). The e2e tests are written for and run with [Nightwatch.js](http://nightwatchjs.org/).

- **`src`**: contains the source code. The codebase is written in ES2015 with [Flow](https://flowtype.org/) type annotations.

  - **`compiler`**: contains code for the template-to-render-function compiler.

    The compiler consists of a parser (converts template strings to element ASTs), an optimizer (detects static trees for vdom render optimization), and a code generator (generate render function code from element ASTs). Note that codegen directly generates code strings from the element AST - it's done this way for smaller code size because the compiler is shipped to the browser in the standalone build.

  - **`core`**: contains universal, platform-agnostic runtime code.

    The LALA 2.0 core is platform-agnostic. That is, the code inside `core` is able to be run in any JavaScript environment, be it the browser, Node.js, or an embedded JavaScript runtime in native applications.

    - **`observer`**: contains code related to the reactivity system.

    - **`vdom`**: contains code related to vdom element creation and patching.

    - **`instance`**: contains LALA instance constructor and prototype methods.

    - **`global-api`**: contains LALA global api.

    - **`components`**: contains universal abstract components.

  - **`server`**: contains code related to server-side rendering.

  - **`platforms`**: contains platform-specific code.

    Entry files for dist builds are located in their respective platform directory.

    Each platform module contains three parts: `compiler`, `runtime` and `server`, corresponding to the three directories above. Each part contains platform-specific modules/utilities which are imported and injected to the core counterparts in platform-specific entry files. For example, the code implementing the logic behind `v-bind:class` is in `platforms/web/runtime/modules/class.js` - which is imported in `platforms/web/entry-runtime.ts` and used to create the browser-specific vdom patching function.

  - **`sfc`**: contains single-file component (`*.LALA` files) parsing logic. This is used in the `LALA-template-compiler` package.

  - **`shared`**: contains utilities shared across the entire codebase.

  - **`types`**: contains type declarations added when we ported the codebase from Flow to TypeScript. These types should be considered internal - they care less about type inference for end-user scenarios and prioritize working with internal source code.

## Financial Contribution

As a pure community-driven project without major corporate backing, we also welcome financial contributions via GitHub Sponsors and OpenCollective. Please consult the [Sponsor Page](https://LALAjs.org/sponsor/) for more details.

## Credits

Thank you to all the people who have already contributed to LALA!
