# Version 2 changes

- CLI Output - own error codes that can be searched within our documentation
- Workspace-aware CLI - helps with packages already used in other workspaces
- Zero-Installs - reads the vendor files directly from the cache, if the cache becomes part of your repository then you never need to run yarn install again
- New Command: yarn dlx - replacement for `npx`
- New Command: yarn workspaces foreach - plugin extends yarn to help run commands over multiple workspaces
- New Protocols - [protocols page](https://next.yarnpkg.com/features/protocols)
  - patch - Creates a patched copy of the original package
  - portal - Creates a link to the given folder (follows dependencies)
- Workspace Releases - experimental way to ship new versions in monorepositories to only changed packages
- Workspace Constraints - put constraints on the packages and their dependancies
- Build Dependency Tracking - rebuild dependency tree when needed
- Normalized Shell - universal npm scripts that work on windows/osx/linux
- New Lockfile Format - now pure yaml
- TypeScript Codebase - rewritten in Typescript
- Modular Architecture - plugins loaded at runtime
- Normalized Configuration - .yarnrc.yml contains all configurations of yarn
- Strict Package Boundaries - Packages aren't allowed to require other packages unless they actually list them in their dependencies
- Read-Only Packages - Packages are now kept within their cache archives. For safety and to  prevent cache corruptions, those archives are mounted as read-only  drives and cannot be modified under normal circumstances

# Resources

-  [Introducing Yarn 2 - dev.to.md](../sources/Introducing Yarn 2 - dev.to/Introducing Yarn 2 - dev.to.md) 