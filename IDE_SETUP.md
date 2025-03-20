# IDE Setup for CourseMS Development

## VS Code Setup

### Recommended Extensions

- **PHP Intelephense** - PHP code intelligence
- **Laravel Artisan** - Laravel-specific commands
- **Tailwind CSS IntelliSense** - Tailwind CSS class name completion
- **TypeScript Vue Plugin (Volar)** - Vue/TS integration
- **ESLint** - JavaScript/TypeScript linting
- **Prettier - Code formatter** - Consistent code formatting
- **GitLens** - Git visualization and management
- **PHP Debug** - Xdebug integration

### Workspace Settings

Create a `.vscode/settings.json` file with the following settings:

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[php]": {
    "editor.defaultFormatter": "bmewburn.vscode-intelephense-client"
  },
  "intelephense.files.exclude": [
    "**/.git/**",
    "**/.svn/**",
    "**/.hg/**",
    "**/CVS/**",
    "**/.DS_Store/**",
    "**/node_modules/**",
    "**/bower_components/**",
    "**/vendor/**/{Tests,tests}/**",
    "**/*.{cache,log,tmp}/**"
  ],
  "emmet.includeLanguages": {
    "javascript": "javascriptreact",
    "typescript": "typescriptreact"
  },
  "tailwindCSS.includeLanguages": {
    "typescript": "javascript",
    "typescriptreact": "javascript"
  }
}
```

## PHPStorm Setup

### Recommended Plugins

- **Laravel** - Laravel-specific features
- **Tailwind CSS** - Tailwind CSS class name completion
- **React** - React support
- **Prettier** - Code formatting

### Key Settings

1. Enable Laravel plugin and configure project properly
2. Set up Composer in the IDE
3. Configure PHP CS Fixer or Prettier for code formatting
4. Enable ESLint for JavaScript/TypeScript linting

## Development Process Setup

### Launch Configuration

Create a `.vscode/launch.json` file for debugging:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for Xdebug",
      "type": "php",
      "request": "launch",
      "port": 9003,
      "pathMappings": {
        "/app": "${workspaceFolder}"
      }
    }
  ]
}
```

### Development Server Management

For VS Code, create npm scripts in package.json:

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "serve": "php artisan serve",
    "start": "concurrently \"npm run serve\" \"npm run dev\""
  }
}
```

Remember to follow the rule: only run one instance of `npm run dev` at a time to prevent port conflicts.

## Git Configuration

Set up Git hooks for pre-commit checks:

```bash
npx husky-init && npm install
npx husky add .husky/pre-commit "npm run lint && npm run format-check"
```

Add to package.json:

```json
{
  "scripts": {
    "lint": "eslint --ext .js,.ts,.tsx resources/js",
    "format": "prettier --write 'resources/js/**/*.{js,ts,tsx}'",
    "format-check": "prettier --check 'resources/js/**/*.{js,ts,tsx}'"
  }
}
```

## Docker Development Environment

If using Docker with Laravel Sail, configure your IDE to connect to the Docker containers for PHP and Node.js services.