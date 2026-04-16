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
- Dynamic `publicPath` based on repository name
- GitHub Actions workflow at `.github/workflows/deploy.yml`

### Steps

1. **Configure the repository name** in `quasar.config.js`:
   - Replace `quasar-simulador-productividad` with your actual GitHub repository name
   - Look for this line: `publicPath: process.env.GITHUB_PAGES === 'true' ? '/quasar-simulador-productividad/' : './',`
   - Example: if your repo is `https://github.com/username/my-app`, change it to `'/my-app/'`

2. **Push to GitHub**:
   ```bash
   git add .
   git commit -m "Update for GitHub Pages deployment"
   git push origin main
   ```

3. **Enable GitHub Pages** in your repository settings:
   - Go to Settings > Pages
   - Set **Source** to **GitHub Actions**
   - The workflow will run automatically on push

### Result

The app will be published from the `dist/spa` folder automatically.
The URL will be: `https://username.github.io/repository-name/`

### Customize the configuration
See [Configuring quasar.config.js](https://v2.quasar.dev/quasar-cli-vite/quasar-config-js).
