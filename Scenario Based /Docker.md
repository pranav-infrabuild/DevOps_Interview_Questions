# Docker Interview Question:

### 1) Identify the errors in the Dockerfile. Explain why each of the identified errors is a problem. Rewrite the Dockerfile with the correct syntax.

#### Dockerfile Submitted by the Candidate:
```dockerfile
FROM node:18

WORDIR app/

COPY package.json

COPY..

RUN npm index.js

EXPOSE 8000

CMD['node' 'index.js']
```

<details>
  
#### Errors Identified:
1. **Typo in `WORKDIR`**: The keyword `WORDIR` is incorrect. It should be `WORKDIR`.
2. **Incorrect Path in `COPY` Instruction**: The `COPY package.json` is missing the destination path. It should specify where to copy the file inside the container.
3. **Missing Space in `COPY..`**: The instruction `COPY..` lacks the necessary space between `COPY` and `..`. This will result in a syntax error.
4. **Incorrect `RUN` Command**: The command `RUN npm index.js` is incorrect. `npm` is used for managing Node.js packages, not for running a script. The correct command should be `RUN node index.js` or a different command that installs dependencies, like `RUN npm install`.
5. **Incorrect Syntax for `CMD`**: The `CMD` instruction syntax is wrong. It should either be a JSON array (`CMD ["node", "index.js"]`) or a string with a shell form (`CMD node index.js`).

#### Corrected Dockerfile:
```dockerfile
FROM node:18

WORKDIR /app

COPY package.json /app

COPY . /app

RUN npm install

EXPOSE 8000

CMD ["node", "index.js"]
```
</details>
