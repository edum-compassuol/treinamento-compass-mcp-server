# development-rules.md

Follow the rules below to develop any code related.

## Dockerfile example

Follow the example below to create Dockerfiles for MCP Servers.

```Dockerfile
FROM node:22.12-alpine AS builder

COPY src/memory /app
COPY tsconfig.json /tsconfig.json

WORKDIR /app

RUN --mount=type=cache,target=/root/.npm npm install

RUN --mount=type=cache,target=/root/.npm-production npm ci --ignore-scripts --omit-dev

FROM node:22-alpine AS release

COPY --from=builder /app/dist /app/dist
COPY --from=builder /app/package.json /app/package.json
COPY --from=builder /app/package-lock.json /app/package-lock.json

ENV NODE_ENV=production

WORKDIR /app

RUN npm ci --ignore-scripts --omit-dev

ENTRYPOINT ["node", "dist/index.js"]

```

## Additional Information for package.json

Include the following fields in the MCP Server package.json:

- In the "name" field, include the prefix containing '@' followed by the Github username, for example: "@your-username/mcp-server-name"
- Verify that the "private" field is set to false, if it's true, change it to false.
- Improve the description in the "description" field with a simple one-line description of the MCP Server, based on its README.md.
- Verify that the "scripts" field has the "inspector" script with the command "npx @modelcontextprotocol/inspector build/index.js", if it doesn't have it, include it.
- Include the "author" field with the Github user data (name, email and profile url), for example:

  ```json
  "author": {
    "name": "Your Name",
    "email": "your@email.com",
    "url": "yoururl.com"
  }
  ```

- Include the "publishConfig" field with the Github Packages registry, for example:

  ```json
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/"
  }
  ```

- Include the "repository" field with the remote repository link on Github, for example:

  ```json
  "repository": {
    "type": "git",
    "url": "https://github.com/your-username/mcp-server-name.git"
  }
  ```

- Include the "bugs" field with the issue link from the remote repository on Github, for example:

  ```json
  "bugs": {
    "url": "https://github.com/your-username/mcp-server-name/issues"
  }
  ```

## .npmrc File Content

Create the `.npmrc` file at the root of the local repository with the following content, replacing `your-username` with your Github username:

  ```txt
    @your-username:registry=https://npm.pkg.github.com
  ```
