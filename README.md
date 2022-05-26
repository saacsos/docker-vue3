# Vue 3 with Docker

## docker-compose.yml

```yml
services:
  my_node:
    container_name: my_node
    image: node:lts
    working_dir: /var/www/html/app/
    entrypoint: /bin/bash
    expose:
      - 3000
    ports:
      - 3000:3000
    volumes:
      - ./frontend/:/var/www/html/app
    tty: true
```

Create container

```bash
docker-compose up -d
```

Hooking into the container

```bash
docker exec -it my_node /bin/bash
```

## Setup Vue 3

When in the docker container, create a Vue project:

```bash
npm init vue@latest
```

And follow [Vuejs Quick Start](https://vuejs.org/guide/quick-start.html)

```bash
✔ Project name: … <your-project-name>
✔ Add TypeScript? … No
✔ Add JSX Support? … No
✔ Add Vue Router for Single Page Application development? … No
✔ Add Pinia for state management? … No
✔ Add Vitest for Unit testing? … No
✔ Add Cypress for both Unit and End-to-End testing? … No
✔ Add ESLint for code quality? … No
✔ Add Prettier for code formatting? … No

Scaffolding project in ./<your-project-name>...
Done.
```

```bash
cd <your-project-name>
npm install
npm run dev
```

## Setting up the Project for Docker

If you want to change default port (3000), the port must be set to match the one in the `docker-compose.yml` file.

Open the `vite.config.js` file and add the server object with the port field to the configuration:

```js
import { fileURLToPath, URL } from 'url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  server: {     // <-- add the server object
    port: 3000
  },
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```

And add parameter `--host` to the `vite` command in the `package.json`, so replace `dev` command in `scripts`:

```json
// package.json
...
"scripts": {
    "dev": "vite",
...
```

with:

```json
// package.json
...
"scripts": {
    "dev": "vite --host",
...
```