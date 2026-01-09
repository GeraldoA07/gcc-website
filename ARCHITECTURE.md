# Architecture Documentation

This document describes the technical architecture of the GCC Website project.

## 📐 Architecture Overview

The GCC Website follows a modern JAMstack architecture with a clear separation between the frontend and backend services.

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                          │
│  ┌────────────────────────────────────────────────────┐    │
│  │         Next.js Frontend Application               │    │
│  │  - React Components                                │    │
│  │  - Server Components & Client Components           │    │
│  │  - API Routes                                      │    │
│  │  - Static & Dynamic Pages                          │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
                            │
                            │ HTTPS/API Calls
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    BACKEND SERVICES                          │
│  ┌────────────────────────────────────────────────────┐    │
│  │              Supabase Backend                      │    │
│  │  ┌──────────────┐  ┌──────────────┐  ┌─────────┐ │    │
│  │  │  PostgreSQL  │  │   Auth       │  │ Storage │ │    │
│  │  │   Database   │  │   Service    │  │ Service │ │    │
│  │  └──────────────┘  └──────────────┘  └─────────┘ │    │
│  │  ┌──────────────┐  ┌──────────────┐              │    │
│  │  │  Realtime    │  │     Edge     │              │    │
│  │  │  Subscriptions│  │  Functions  │              │    │
│  │  └──────────────┘  └──────────────┘              │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

## 🏗️ Frontend Architecture

### Next.js App Router Structure

The project uses Next.js 13+ with the App Router for optimal performance and developer experience.

```
app/
├── layout.tsx              # Root layout (shared across all pages)
├── page.tsx                # Home page
├── (public)/               # Public routes group
│   ├── about/
│   │   └── page.tsx        # About page
│   ├── events/
│   │   ├── page.tsx        # Events listing
│   │   └── [id]/
│   │       └── page.tsx    # Individual event page
│   ├── articles/
│   │   ├── page.tsx        # Articles listing
│   │   └── [slug]/
│   │       └── page.tsx    # Individual article page
│   ├── testimonies/
│   │   └── page.tsx        # Testimonies page
│   ├── gallery/
│   │   ├── page.tsx        # Gallery overview
│   │   └── [album]/
│   │       └── page.tsx    # Album view
│   ├── achievements/
│   │   └── page.tsx        # Achievements page
│   └── contact/
│       └── page.tsx        # Contact page
├── (admin)/                # Admin routes group
│   ├── layout.tsx          # Admin layout with auth
│   └── dashboard/
│       ├── page.tsx        # Dashboard home
│       ├── events/         # Event management
│       ├── articles/       # Article management
│       └── settings/       # Settings
└── api/                    # API routes
    ├── auth/               # Authentication endpoints
    ├── webhooks/           # Webhook handlers
    └── ...                 # Other API routes
```

### Component Architecture

```
components/
├── ui/                     # Base UI components
│   ├── Button.tsx
│   ├── Card.tsx
│   ├── Input.tsx
│   ├── Modal.tsx
│   └── ...
├── layout/                 # Layout components
│   ├── Header.tsx
│   ├── Footer.tsx
│   ├── Navigation.tsx
│   └── Sidebar.tsx
├── features/               # Feature-specific components
│   ├── events/
│   │   ├── EventCard.tsx
│   │   ├── EventCalendar.tsx
│   │   └── EventRegistrationForm.tsx
│   ├── articles/
│   │   ├── ArticleCard.tsx
│   │   ├── ArticleList.tsx
│   │   └── ArticleContent.tsx
│   ├── testimonies/
│   │   ├── TestimonyCard.tsx
│   │   └── TestimonyForm.tsx
│   └── gallery/
│       ├── GalleryGrid.tsx
│       ├── ImageViewer.tsx
│       └── AlbumCard.tsx
└── shared/                 # Shared/common components
    ├── LoadingSpinner.tsx
    ├── ErrorBoundary.tsx
    └── SEOHead.tsx
```

## 🗄️ Database Architecture

### PostgreSQL Schema (Supabase)

#### Core Tables

**users**
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email TEXT UNIQUE NOT NULL,
  full_name TEXT,
  role TEXT DEFAULT 'member',
  avatar_url TEXT,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

**events**
```sql
CREATE TABLE events (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  title TEXT NOT NULL,
  description TEXT,
  event_date TIMESTAMP NOT NULL,
  location TEXT,
  image_url TEXT,
  registration_required BOOLEAN DEFAULT false,
  max_attendees INTEGER,
  status TEXT DEFAULT 'upcoming',
  created_by UUID REFERENCES users(id),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

**articles**
```sql
CREATE TABLE articles (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  title TEXT NOT NULL,
  slug TEXT UNIQUE NOT NULL,
  content TEXT NOT NULL,
  excerpt TEXT,
  featured_image TEXT,
  author_id UUID REFERENCES users(id),
  published BOOLEAN DEFAULT false,
  published_at TIMESTAMP,
  category TEXT,
  tags TEXT[],
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

**testimonies**
```sql
CREATE TABLE testimonies (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  title TEXT NOT NULL,
  content TEXT NOT NULL,
  author_name TEXT NOT NULL,
  author_image TEXT,
  featured BOOLEAN DEFAULT false,
  approved BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

**gallery_albums**
```sql
CREATE TABLE gallery_albums (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name TEXT NOT NULL,
  description TEXT,
  cover_image TEXT,
  event_date DATE,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

**gallery_images**
```sql
CREATE TABLE gallery_images (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  album_id UUID REFERENCES gallery_albums(id) ON DELETE CASCADE,
  image_url TEXT NOT NULL,
  caption TEXT,
  order_index INTEGER,
  created_at TIMESTAMP DEFAULT NOW()
);
```

**achievements**
```sql
CREATE TABLE achievements (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  title TEXT NOT NULL,
  description TEXT,
  category TEXT,
  achievement_date DATE,
  image_url TEXT,
  featured BOOLEAN DEFAULT false,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);
```

### Row Level Security (RLS)

Supabase RLS policies ensure data security:

```sql
-- Example: Public read access for published articles
CREATE POLICY "Public articles are viewable by everyone"
  ON articles FOR SELECT
  USING (published = true);

-- Example: Only authenticated users can create testimonies
CREATE POLICY "Authenticated users can create testimonies"
  ON testimonies FOR INSERT
  WITH CHECK (auth.role() = 'authenticated');

-- Example: Only admins can manage events
CREATE POLICY "Admins can manage events"
  ON events FOR ALL
  USING (
    auth.uid() IN (
      SELECT id FROM users WHERE role = 'admin'
    )
  );
```

## 🔐 Authentication & Authorization

### Authentication Flow

1. User signs up/logs in via Supabase Auth
2. JWT token generated and stored in httpOnly cookie
3. Client-side authentication state managed via Supabase client
4. Protected routes check authentication status
5. Server components verify token on server-side

### Role-Based Access Control

**Roles**:
- `visitor`: Unauthenticated users
- `member`: Registered church members
- `editor`: Content creators
- `admin`: Full access

**Permissions Matrix**:
```
Action              | Visitor | Member | Editor | Admin
--------------------|---------|--------|--------|-------
View public pages   |    ✓    |   ✓    |   ✓    |   ✓
View member content |    ✗    |   ✓    |   ✓    |   ✓
Submit testimony    |    ✗    |   ✓    |   ✓    |   ✓
Create articles     |    ✗    |   ✗    |   ✓    |   ✓
Manage events       |    ✗    |   ✗    |   ✓    |   ✓
Manage users        |    ✗    |   ✗    |   ✗    |   ✓
System settings     |    ✗    |   ✗    |   ✗    |   ✓
```

## 📦 Storage Architecture

### Supabase Storage Buckets

```
storage/
├── avatars/               # User profile images
│   └── [user-id]/
├── events/                # Event images
│   └── [event-id]/
├── articles/              # Article images
│   └── [article-id]/
├── gallery/               # Gallery images
│   └── [album-id]/
└── achievements/          # Achievement images
    └── [achievement-id]/
```

### Storage Policies

- Public buckets for public-facing images
- Private buckets for user-uploaded content
- Size limits enforced per file type
- Image optimization on upload

## 🔄 State Management

### Client-Side State

- **React Context**: Global app state (auth, theme)
- **Local State**: Component-specific state with `useState`
- **Server State**: Managed via React Server Components
- **URL State**: Search params and routing state

### Data Fetching Strategy

```typescript
// Server Components (default in Next.js 13+)
async function EventsPage() {
  const events = await fetchEvents(); // Direct DB query
  return <EventsList events={events} />;
}

// Client Components (when interactivity needed)
'use client';
function EventRegistration() {
  const [loading, setLoading] = useState(false);
  
  const handleRegister = async () => {
    setLoading(true);
    await registerForEvent();
    setLoading(false);
  };
  
  return <Button onClick={handleRegister}>Register</Button>;
}
```

## 🚀 Performance Optimization

### Rendering Strategies

1. **Static Generation (SSG)**: For mostly static content
   - Home page
   - About pages
   - Static articles

2. **Server-Side Rendering (SSR)**: For dynamic content
   - Events listing
   - Recent articles
   - User dashboards

3. **Incremental Static Regeneration (ISR)**: For semi-dynamic content
   - Article pages (revalidate every hour)
   - Event pages (revalidate every 15 minutes)

4. **Client-Side Rendering (CSR)**: For highly interactive features
   - Admin dashboards
   - Real-time comments
   - Interactive forms

### Caching Strategy

```typescript
// Example: ISR with revalidation
export const revalidate = 3600; // 1 hour

async function ArticlePage({ params }: { params: { slug: string } }) {
  const article = await fetchArticle(params.slug);
  return <Article data={article} />;
}
```

### Image Optimization

- Next.js Image component for automatic optimization
- WebP format with fallbacks
- Responsive images for different screen sizes
- Lazy loading for off-screen images

## 🔍 SEO Architecture

### Meta Tags & Schema

```typescript
// Example metadata
export const metadata: Metadata = {
  title: 'GCC - Grace Community Church',
  description: 'Welcome to Grace Community Church...',
  openGraph: {
    title: 'GCC - Grace Community Church',
    description: '...',
    images: ['/og-image.jpg'],
  },
  twitter: {
    card: 'summary_large_image',
  },
};
```

### Sitemap Generation

```typescript
// app/sitemap.ts
export default async function sitemap() {
  const articles = await fetchArticles();
  const events = await fetchEvents();
  
  return [
    { url: 'https://gcc.org', changefreq: 'daily' },
    ...articles.map(a => ({
      url: `https://gcc.org/articles/${a.slug}`,
      lastmod: a.updated_at,
    })),
    ...events.map(e => ({
      url: `https://gcc.org/events/${e.id}`,
      lastmod: e.updated_at,
    })),
  ];
}
```

## 🛡️ Security Considerations

### Best Practices

1. **Environment Variables**: Sensitive keys in `.env.local`
2. **API Security**: Rate limiting and input validation
3. **XSS Prevention**: Sanitize user input
4. **CSRF Protection**: Built into Next.js forms
5. **SQL Injection**: Prevented by Supabase client
6. **File Upload**: Size and type restrictions
7. **Authentication**: Secure JWT tokens with httpOnly cookies

## 📱 Mobile-First Approach

### Responsive Breakpoints

```typescript
// tailwind.config.js
module.exports = {
  theme: {
    screens: {
      'sm': '640px',   // Mobile landscape
      'md': '768px',   // Tablet
      'lg': '1024px',  // Desktop
      'xl': '1280px',  // Large desktop
      '2xl': '1536px', // Extra large desktop
    },
  },
};
```

### Progressive Web App (PWA) Ready

- Service worker for offline functionality
- Web app manifest for installability
- Push notifications capability
- Offline-first data caching

## 🔄 Deployment Architecture

### CI/CD Pipeline

```
GitHub Push
    │
    ▼
GitHub Actions
    │
    ├─→ Lint & Type Check
    ├─→ Run Tests
    ├─→ Build Application
    │
    ▼
Vercel Deployment
    │
    ├─→ Preview Deployment (PRs)
    └─→ Production Deployment (main branch)
```

### Environment Setup

- **Development**: Local development with hot reload
- **Staging**: Preview deployments for testing
- **Production**: Production environment with CDN

## 📊 Monitoring & Analytics

### Observability

- **Error Tracking**: Integration-ready for Sentry
- **Analytics**: Google Analytics or Plausible
- **Performance Monitoring**: Vercel Analytics
- **Database Monitoring**: Supabase dashboard

## 🔮 Future Architecture Considerations

- **Microservices**: Consider splitting complex features
- **GraphQL**: Potential migration from REST
- **Edge Functions**: For complex serverless logic
- **CDN**: CloudFlare or similar for global distribution
- **Elasticsearch**: For advanced search capabilities
- **Redis**: For caching and session management

---

This architecture is designed to be scalable, maintainable, and aligned with modern web development best practices while serving the needs of the GCC community.
