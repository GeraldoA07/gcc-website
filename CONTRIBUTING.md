# Contributing to GCC Website

Thank you for your interest in contributing to the GCC Website project! This document provides guidelines and instructions for contributing.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Guidelines](#development-guidelines)
- [Pull Request Process](#pull-request-process)
- [Style Guidelines](#style-guidelines)
- [Testing Guidelines](#testing-guidelines)

## 🤝 Code of Conduct

By participating in this project, you agree to maintain a respectful and inclusive environment. We expect all contributors to:

- Be respectful and considerate
- Welcome newcomers and help them get started
- Focus on what is best for the community
- Show empathy towards other community members

## 🚀 Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/gcc-website.git
   cd gcc-website
   ```
3. **Add upstream remote**:
   ```bash
   git remote add upstream https://github.com/GeraldoA07/gcc-website.git
   ```
4. **Install dependencies**:
   ```bash
   npm install
   ```
5. **Set up environment variables** (see README.md)
6. **Create a branch** for your changes:
   ```bash
   git checkout -b feature/your-feature-name
   ```

## 💡 How to Contribute

### Reporting Bugs

If you find a bug, please create an issue with:
- Clear, descriptive title
- Detailed description of the issue
- Steps to reproduce the problem
- Expected vs actual behavior
- Screenshots (if applicable)
- Environment details (browser, OS, etc.)

### Suggesting Enhancements

For feature requests or enhancements:
- Check if the feature has already been requested
- Provide a clear description of the feature
- Explain why this feature would be useful
- Include mockups or examples if applicable

### Contributing Code

1. **Find an issue to work on** or create one
2. **Comment on the issue** to let others know you're working on it
3. **Follow the development guidelines** below
4. **Submit a pull request** when ready

## 🛠️ Development Guidelines

### Code Structure

- Keep components small and focused
- Use TypeScript for type safety
- Follow the existing project structure
- Create reusable components when appropriate

### Naming Conventions

- **Files**: Use kebab-case for file names (e.g., `user-profile.tsx`)
- **Components**: Use PascalCase (e.g., `UserProfile`)
- **Functions**: Use camelCase (e.g., `getUserData`)
- **Constants**: Use UPPER_SNAKE_CASE (e.g., `API_BASE_URL`)

### Component Guidelines

```typescript
// Example component structure
import React from 'react';

interface ComponentNameProps {
  // Define prop types
  title: string;
  onAction?: () => void;
}

export function ComponentName({ title, onAction }: ComponentNameProps) {
  // Component logic here
  
  return (
    <div className="container">
      <h1>{title}</h1>
      {/* Component JSX */}
    </div>
  );
}
```

### Styling with Tailwind CSS

- Use Tailwind utility classes
- Create custom classes in the config for repeated patterns
- Follow mobile-first responsive design
- Use semantic color names from the theme

```typescript
// Good
<div className="container mx-auto px-4 py-8 lg:px-8">

// Avoid inline styles when possible
<div style={{ padding: '20px' }}>
```

### Working with Supabase

- Use the Supabase client from `lib/supabase`
- Handle errors appropriately
- Use TypeScript types for database queries
- Implement proper loading and error states

```typescript
// Example Supabase query
const { data, error } = await supabase
  .from('events')
  .select('*')
  .order('date', { ascending: false });

if (error) {
  console.error('Error fetching events:', error);
  return;
}
```

## 🔄 Pull Request Process

1. **Update your branch** with the latest changes:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. **Ensure your code**:
   - Follows the style guidelines
   - Passes all tests
   - Includes appropriate documentation
   - Has no linting errors

3. **Run checks**:
   ```bash
   npm run lint
   npm run type-check
   npm run build
   ```

4. **Commit your changes**:
   ```bash
   git add .
   git commit -m "feat: add user profile feature"
   ```

5. **Push to your fork**:
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create a pull request**:
   - Provide a clear title and description
   - Reference any related issues
   - Add screenshots for UI changes
   - Request review from maintainers

### Commit Message Guidelines

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
type(scope): subject

body

footer
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples**:
```
feat(events): add event registration form
fix(gallery): resolve image loading issue
docs(readme): update installation instructions
```

## 🎨 Style Guidelines

### TypeScript

- Use TypeScript for all new code
- Define interfaces for props and data structures
- Avoid using `any` type
- Use strict type checking

### React/Next.js

- Use functional components with hooks
- Implement proper error boundaries
- Use Next.js Image component for images
- Implement proper loading states
- Use server components where appropriate (Next.js 13+)

### Accessibility

- Use semantic HTML elements
- Include proper ARIA labels
- Ensure keyboard navigation
- Maintain color contrast ratios
- Test with screen readers

### Performance

- Optimize images (use Next.js Image)
- Implement lazy loading where appropriate
- Minimize bundle size
- Use proper caching strategies

## 🧪 Testing Guidelines

### Writing Tests

- Write tests for new features
- Update tests when modifying existing features
- Aim for meaningful test coverage
- Test both success and error cases

### Running Tests

```bash
# Run all tests
npm run test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage
```

## 📝 Documentation

- Update README.md for major changes
- Add JSDoc comments for complex functions
- Update API documentation if applicable
- Include inline comments for complex logic

## 🔍 Code Review

All submissions require review. We use GitHub pull requests for this purpose. Reviewers will check:

- Code quality and style
- Test coverage
- Documentation
- Performance implications
- Security considerations

## 📞 Getting Help

- Check existing issues and discussions
- Ask questions in the issue comments
- Reach out to maintainers if needed

## 🙏 Thank You

Thank you for contributing to the GCC Website project! Your efforts help make this platform better for the entire community.
