# Backend development

- It is recommended to open the `dodeka/backend` folder in VS Code, not `dodeka`.
- If you have not set it up yet, start with [Backend setup](../setup/backend.md).

### Continuous Integration (CI)

Tests (including some additional tests that run against a live database) and all the above tools are all run in GitHub actions. If you open a Pull Request, these checks are run for every commit you push. If any fail, the "check" will fail, indicating that we should not merge. 

### VS Code settings

VS Code doesn't come included with all necessary/useful tools for developing a Python application. Therefore, a few extensions have been added in `dodeka/backend/.vscode/extensions.json`. VS Code should automatically recommend them for you to install.
