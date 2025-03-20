# Development Environment Setup for CourseMS

## Primary Development Environment: Herd

CourseMS primarily uses [Herd](https://herd.laravel.com/) as the recommended local development environment for Laravel.

### Using Herd for Laravel Development

1. **Installation**
   - Download and install Herd from the [official website](https://herd.laravel.com/)
   - Herd is available for macOS and Windows

2. **Configuration**
   - Herd automatically configures PHP, MySQL, and Nginx
   - Provides isolated PHP environments per project
   - Includes SSL support out of the box

3. **Project Setup**
   - Add your project to Herd through the GUI
   - Herd automatically configures hostnames (e.g., `coursems.test`)
   - No need for manual virtual host configuration

4. **Database Access**
   - Connect to MySQL databases through the provided MySQL socket
   - Use TablePlus or another compatible database management tool

5. **Performance Benefits**
   - Optimized PHP-FPM setup for Laravel
   - Fast site loads and hot reloading
   - Lower resource usage compared to Docker-based solutions

### Recommended Herd Configuration

In your `.env` file, use the following connection settings when working with Herd:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=coursems
DB_USERNAME=root
DB_PASSWORD=
```

## Alternative: Docker with Laravel Sail

For developers who prefer Docker, Laravel Sail is also supported for development.

1. **Installation**
   - Ensure Docker Desktop is installed
   - Configure Laravel Sail using `php artisan sail:install`

2. **Running with Sail**
   - Start containers: `./vendor/bin/sail up -d`
   - Run commands: `./vendor/bin/sail php artisan migrate`

3. **Configuration**
   - Edit `docker-compose.yml` to customize services
   - Sail provides MySQL, Redis, Mailhog, and other services

## Local Development Workflow

Regardless of whether you use Herd or Sail, follow these steps to get started:

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-org/coursems.git
   cd coursems
   ```

2. **Install dependencies**
   ```bash
   composer install
   npm install
   ```

3. **Environment setup**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

4. **Run migrations**
   ```bash
   php artisan migrate --seed
   ```

5. **Start development server**
   ```bash
   # If using Herd - Herd handles the PHP server automatically
   npm run dev
   
   # If using Sail
   ./vendor/bin/sail up -d
   ./vendor/bin/sail npm run dev
   ```

## Production Environment Recommendations

- Use a managed Laravel hosting solution like [Laravel Forge](https://forge.laravel.com/) or [Laravel Vapor](https://vapor.laravel.com/)
- Configure CI/CD pipelines for automated testing and deployment
- Use environment-specific configuration for production settings