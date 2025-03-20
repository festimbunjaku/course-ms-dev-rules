# CourseMS: Development Standards & Best Practices
> Laravel 12 + Inertia.js 2.0 + PHP 8.4+ + TypeScript + React 19

## 1. Core Principles & Architecture

### Development Philosophy
- Build maintainable, testable, and scalable code
- Follow SOLID principles and clean architecture patterns
- Implement API-driven development with Laravel backend and Inertia.js frontend
- Maintain strict typing throughout the codebase
- Use dependency injection to improve testability and reduce coupling

### Code Organization
- Follow PSR-12 coding standards for PHP
- Use namespacing effectively to organize code
- Keep controllers thin by delegating business logic to services/actions
- Group related functionality into dedicated modules/domains
- Separate concerns between frontend and backend responsibilities

## 2. Laravel 12 Best Practices

### General Standards
- Use PHP 8.4+ features extensively:
  - Read-only properties
  - Union types
  - Named arguments
  - Match expressions
  - Enums for type-safe alternatives to constants
- Leverage native Laravel helpers (Str::, Arr::, now(), etc.)
- Follow Laravel conventions for file and directory structure

### Laravel-Specific Standards
- Use `php artisan make:{option}` to create models, migrations, controllers, etc.
- Note that `app\Console\Kernel.php` does not exist in Laravel 11+. If the file is not present, use the `bootstrap/app.php` file for related configurations.
- In Laravel 11+ commands created in `app\Console\Commands\` are automatically registered and available to use.
- Add environment variables to config files and avoid using env variables directly in the code. For example `config('app.name')` instead of `env('APP_NAME')`.
- Avoid N+1 queries by using eager loading or batch loading:
  ```php
  $users = User::with('posts')->get();
  $posts = Post::whereIn('user_id', $users->pluck('id'))->get();
  ```
- Prefer soft deletes for models using the `SoftDeletes` trait:
  ```php
  class User extends Model
  {
      use SoftDeletes;
  }
  ```

### API Development
- Use Inertia.js for data communication between frontend and backend
- Avoid building separate API endpoints when using Inertia.js
- Leverage Inertia's shared data and props for passing data to components
- For Ajax-like interactions, use Inertia's visit() method with the data option
- When API endpoints are necessary for external services, use Laravel API Resources
- Structure controllers to return Inertia responses when serving pages:
  ```php
  return Inertia::render('Dashboard', [
      'courses' => $courses,
      'stats' => $stats
  ]);
  ```
- Keep sensitive logic server-side and only pass necessary data to the frontend

### Eloquent & Database Practices
- Always define relationships in models
- Protect models from mass assignment vulnerabilities with $fillable or $guarded
- Optimize database queries:
  - Use eager loading (with()) to avoid N+1 problems
  - Create indices for frequently queried columns
  - Use query scopes for common filtering operations
- Implement database transactions for multi-step operations
- Use migrations for all database changes with appropriate up() and down() methods

### Security & Validation
- Implement Form Requests for validation logic
- Use Laravel Sanctum for API authentication
- Protect against CSRF, XSS, SQL Injection using Laravel's built-in protections
- Implement proper authorization checks using Gates and Policies
- Validate all user input, both on client and server sides

### Performance Optimization
- Implement caching strategies using Redis or Memcached
- Use queues for processing time-consuming tasks
- Implement database query optimization and indexing
- Use Laravel's built-in performance tools (route caching, config caching, etc.)
- Configure proper server settings for production environments

### Error Handling & Logging
- Create custom exceptions for domain-specific errors
- Implement centralized exception handling
- Use structured logging with appropriate log levels
- Monitor application performance and errors
- Implement proper error reporting in production

## 3. Inertia.js 2.0 & Frontend Best Practices

### Component Architecture
- Organize frontend files logically:
  - `/components` - Reusable UI components
  - `/pages` - Page-level components mapped to routes
  - `/layouts` - Layout components
  - `/types` - TypeScript interfaces and types
  - `/hooks` - Custom React hooks
  - `/utils` - Utility functions
- Create small, single-responsibility components
- Implement consistent component naming conventions

### State Management
- Use React Context API for global state when appropriate
- Implement proper state management patterns
- Minimize prop drilling with composition or context
- Use Inertia's shared data feature for app-wide state
- Leverage TypeScript for type-safe state management

### UI & Styling
- Use ShadCN UI and Tailwind CSS for consistent, responsive design
- Implement design system patterns for UI consistency
- Follow accessibility best practices (WCAG 2.1 AA)
- Create mobile-first responsive layouts
- Use CSS variables for theme customization
- Use the latest and paid version of v0.dev for UI generation:
  - Generate sleek, responsive, and visually stunning UI components
  - Follow cutting-edge UI/UX principles in all generated components
  - Ensure designs are modern and aligned with latest design trends
  - Create components that are polished and intuitive to use
- Structure all output from v0.dev as optimized React (TSX) components:
  - Ensure full compatibility with Inertia.js 2.0
  - Optimize for the Laravel 12 React Starter Kit architecture
  - Implement proper TypeScript typing for all components
  - Use Tailwind CSS for styling following v4 standards
- Balance dynamic and static UI approaches:
  - Choose the best approach for each component based on its function
  - Implement dynamic UI for interactive elements
  - Use static UI for content-focused components
  - Ensure consistency between dynamic and static elements
- Integrate v0.dev output systematically:
  - Organize generated components in the appropriate project directories
  - Maintain consistent naming conventions
  - Ensure all imports are properly configured
  - Document component usage and properties

### React Best Practices
- Use functional components with hooks instead of class components
- Create custom hooks for reusable logic
- Use the Context API for state management when needed
- Implement proper prop validation with PropTypes and TypeScript
- Use React.memo for performance optimization when necessary
- Use fragments to avoid unnecessary DOM elements
- Ensure proper list rendering with unique keys
- Prefer composition over inheritance for component design

### Tailwind CSS 4 Practices
- Use CSS-first configuration with `@theme` directive:
  ```css
  @import "tailwindcss";
  
  @theme {
    --font-display: "Satoshi", "sans-serif";
    --breakpoint-3xl: 1920px;
    --color-avocado-500: oklch(0.84 0.18 117.33);
  }
  ```
- Use `@import "tailwindcss"` instead of separate `@tailwind` directives
- Use updated package names: `@tailwindcss/postcss`, `@tailwindcss/cli`, `@tailwindcss/vite`
- Use CSS variables for all design tokens
- Implement container queries with `@container` for responsive components
- Utilize 3D transforms and enhanced gradients for modern UI elements
- Apply composable variants by chaining them for complex interactions
- Create custom utilities with `@utility` directive when needed

### Performance Optimization
- Implement code splitting and lazy loading
- Optimize bundle size with tree shaking
- Use React's memo, useMemo, and useCallback for performance
- Implement proper loading states and skeletons
- Configure Vite for optimal frontend builds

### Inertia.js-Specific Practices
- Use Inertia links and forms for seamless navigation
- Leverage shared data to minimize redundant API calls
- Implement proper page transitions
- Use Inertia's error handling capabilities
- Utilize Inertia's progress indicators for better UX

### Development Server Management
- Ensure only one instance of npm run dev is running at any time
- Check for active instances before starting a new development server
- If development server is closed, restart it safely without causing conflicts
- Prevent multiple instances to avoid port binding errors and resource consumption
- Monitor development server health during development sessions

## 4. TypeScript Standards

### General Standards
- Enable strict mode in TypeScript configuration
- Create meaningful interfaces and type definitions
- Avoid using `any` type; use proper type definitions
- Use TypeScript's utility types when appropriate
- Create reusable type definitions for common structures

### Frontend TypeScript
- Define prop types for all React components
- Use TypeScript with React's hooks effectively
- Create typed API request/response interfaces
- Implement proper typing for forms and validation
- Use generics for reusable components

### Type Safety
- Ensure full type coverage across the application
- Run TypeScript in strict mode
- Use type guards for runtime type checking
- Define domain types that represent business entities
- Create discriminated unions for complex state management

## 5. Testing Standards

### General Testing Principles
- Aim for high test coverage (minimum 80%)
- Write tests before implementing features (TDD)
- Create meaningful test descriptions
- Separate unit, integration, and e2e tests
- Use testing utilities to simplify test writing

### Backend Testing
- Use Pest for PHP testing
- Implement feature tests for API endpoints
- Create unit tests for services and models
- Use database factories for test data
- Mock external services in tests

### Frontend Testing
- Use Jest and React Testing Library
- Test component rendering and user interactions
- Implement snapshot testing for UI components
- Create integration tests for complex workflows
- Test form validations and error states

## 6. Workflow & Collaboration Standards

### Version Control
- Use Git with a clear branching strategy (e.g., GitFlow)
- Write meaningful commit messages following conventional commits
- Create pull/merge requests for code reviews
- Implement CI/CD pipelines for automated testing
- Protect main branches with required reviews

### Documentation
- Write clear README files for all major components
- Add docblocks to classes and methods
- Document complex algorithms and business rules
- Keep documentation up-to-date with code changes

### Code Quality Tools
- Use static analysis tools (PHPStan, ESLint)
- Implement automatic code formatting (Prettier)
- Set up pre-commit hooks for quality checks
- Use SonarQube or similar for code quality metrics
- Perform regular dependency updates

### Development Process
- Implement agile development practices
- Break down work into manageable tasks
- Conduct regular code reviews
- Maintain a knowledge base for the project
- Follow continuous integration practices

## 7. Environment & Operations

### Local Development
- Use consistent development environments (Docker)
- Set up proper environment variables
- Configure debugging tools
- Use hot reloading for faster development
- Implement development-only features

### Deployment
- Implement zero-downtime deployments
- Use environment-specific configurations
- Set up proper monitoring and alerting
- Implement backup and disaster recovery
- Use infrastructure as code for server provisioning

### Security Operations
- Conduct regular security audits
- Implement proper access controls
- Keep dependencies updated
- Monitor for security vulnerabilities
- Implement proper authentication and authorization

## 8. Performance & Optimization

### Frontend Performance
- Optimize asset loading and bundling
- Implement proper caching strategies
- Use code splitting and lazy loading
- Optimize images and media
- Implement performance monitoring

### Backend Performance
- Optimize database queries
- Implement caching for frequently accessed data
- Use queues for background processing
- Configure proper server settings
- Monitor application performance

## 9. Accessibility & Internationalization

### Accessibility
- Follow WCAG 2.1 AA standards
- Use semantic HTML elements
- Implement keyboard navigation
- Provide proper ARIA attributes
- Test with screen readers

### Internationalization
- Use Laravel's localization features
- Implement proper text direction support
- Use translation keys instead of hardcoded strings
- Support multiple locales
- Consider cultural differences in UI design

## 10. Database Standards

### MySQL Best Practices
- Use appropriate data types to optimize storage and performance (e.g., INT for IDs, VARCHAR with appropriate length)
- Create indexes for columns used in WHERE, JOIN, and ORDER BY clauses
- Implement foreign keys to maintain referential integrity
- Use EXPLAIN to analyze and optimize queries
- Avoid using SELECT * and only retrieve needed columns
- Use prepared statements to prevent SQL injection
- Use appropriate character set and collation (e.g., utf8mb4_unicode_ci)
- Use transactions for operations that must be atomic
- Implement database normalization to appropriate levels (typically 3NF)
- Create meaningful and consistent naming conventions for tables and columns