# Quamodoro (quasar-project)

A productivity app
## Install the dependencies
```bash
yarn
# or
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```

### Lint the files
```bash
yarn lint
# or
npm run lint
```

### Format the files
```bash
yarn format
# or
npm run format
```

### Build the app for production
```bash
quasar build
```

## Deploy to GitHub Pages

This project is ready for GitHub Pages using:

- `vueRouterMode: 'hash'`
- `publicPath: './'`
- GitHub Actions workflow at `.github/workflows/deploy.yml`

### Steps
1. Push the project to your GitHub repository.
2. In the repository, go to **Settings > Pages**.
3. Set **Source** to **GitHub Actions**.
4. Push to the `main` branch or run the workflow manually.

### Result
The app will be published from the `dist/spa` folder automatically.

### Customize the configuration
See [Configuring quasar.config.js](https://v2.quasar.dev/quasar-cli-vite/quasar-config-js).
