# GCC Website

A modern, comprehensive community church website built for Grace Community Church (GCC).

## 📖 Project Overview

The GCC Website serves as the digital home for Grace Community Church, providing a welcoming and informative platform for both church members and the general public. This project aims to create a vibrant online presence that reflects the church's mission, values, and community spirit.

### Purpose

The website serves multiple purposes:
- **Information Hub**: Central location for church information, service times, and contact details
- **Community Platform**: Showcase testimonies, achievements, and member contributions
- **Event Management**: Share upcoming events, gatherings, and church activities
- **Content Delivery**: Publish articles, sermons, and educational content
- **Media Gallery**: Display photos and videos from church events and activities
- **Outreach Tool**: Provide clear pathways for visitors to join the community
- **Member Engagement**: Enable members to share their talents, works, and testimonies

### Target Audience

- **Church Members**: Access to church information, events, and community resources
- **Visitors & Seekers**: Introduction to the church, its beliefs, and how to get involved
- **General Public**: Community outreach and public events information

## 🎯 Goals & Features

### Core Features

1. **Home Page**
   - Welcome message and church vision
   - Featured content and upcoming events
   - Quick access to important information

2. **About Section**
   - Church history and mission
   - Leadership team and staff
   - Statement of faith and beliefs

3. **Events**
   - Upcoming events calendar
   - Event details and registration
   - Past events archive

4. **Articles & Blog**
   - Sermons and teachings
   - Blog posts and devotionals
   - Community announcements

5. **Testimonies**
   - Member testimonies and stories
   - Faith journeys and experiences
   - Community impact stories

6. **Gallery**
   - Photo galleries from church events
   - Video content and recordings
   - Community highlights

7. **Achievements**
   - Community accomplishments
   - Member milestones
   - Ministry impact stories

8. **Contact & Join**
   - Contact information and forms
   - Location and service times
   - Membership pathways
   - Volunteer opportunities

### Design Principles

- **Mobile-First**: Optimized for mobile devices with responsive design
- **SEO-Friendly**: Built with search engine optimization best practices
- **Accessible**: WCAG compliant for inclusive user experience
- **Fast & Performant**: Optimized loading times and performance
- **User-Friendly**: Intuitive navigation and clear information architecture
- **Content-Driven**: Easy content management and updates

## 🛠️ Technology Stack

### Frontend
- **Framework**: [Next.js](https://nextjs.org/) - React framework with server-side rendering, static site generation, and optimal performance
- **Styling**: [Tailwind CSS](https://tailwindcss.com/) - Utility-first CSS framework for rapid UI development
- **Language**: TypeScript (recommended) for type safety
- **UI Components**: Custom components built with React and Tailwind CSS

### Backend & Services
- **Backend-as-a-Service**: [Supabase](https://supabase.com/)
  - **Database**: PostgreSQL for robust data storage
  - **Authentication**: Supabase Auth for user management and security
  - **Storage**: Supabase Storage for images, videos, and media files
  - **Real-time**: Real-time subscriptions for dynamic content

### Additional Tools
- **Version Control**: Git & GitHub
- **Deployment**: Vercel (recommended for Next.js) or similar platforms
- **Analytics**: Integration-ready for Google Analytics or similar tools
- **Forms**: Contact forms and event registration

## 📁 Project Structure

```
gcc-website/
├── app/                    # Next.js app directory
│   ├── (routes)/          # Route groups
│   │   ├── about/         # About pages
│   │   ├── events/        # Events pages
│   │   ├── articles/      # Articles/blog pages
│   │   ├── testimonies/   # Testimonies pages
│   │   ├── gallery/       # Gallery pages
│   │   ├── achievements/  # Achievements pages
│   │   └── contact/       # Contact pages
│   ├── api/               # API routes
│   ├── layout.tsx         # Root layout
│   └── page.tsx           # Home page
├── components/            # Reusable React components
│   ├── ui/               # UI components
│   ├── layout/           # Layout components
│   └── shared/           # Shared components
├── lib/                  # Utility functions
│   ├── supabase/         # Supabase client and helpers
│   └── utils/            # Helper functions
├── public/               # Static assets
│   ├── images/           # Images
│   └── icons/            # Icons
├── styles/               # Global styles
├── types/                # TypeScript type definitions
├── .env.local            # Environment variables (not committed)
├── .env.example          # Environment variables template
├── next.config.js        # Next.js configuration
├── tailwind.config.js    # Tailwind CSS configuration
├── tsconfig.json         # TypeScript configuration
└── package.json          # Project dependencies
```

## 🚀 Getting Started

### Prerequisites

- Node.js 18.x or higher
- npm, yarn, or pnpm package manager
- Supabase account (for backend services)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/GeraldoA07/gcc-website.git
   cd gcc-website
   ```

2. **Install dependencies**
   ```bash
   npm install
   # or
   yarn install
   # or
   pnpm install
   ```

3. **Set up environment variables**
   
   Create a `.env.local` file in the root directory:
   ```env
   # Supabase Configuration
   NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
   NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
   
   # Optional: Analytics, etc.
   NEXT_PUBLIC_GA_MEASUREMENT_ID=your_ga_id
   ```

4. **Set up Supabase**
   - Create a new project at [supabase.com](https://supabase.com)
   - Run database migrations (if available)
   - Configure storage buckets for images and media
   - Set up authentication providers

5. **Run the development server**
   ```bash
   npm run dev
   # or
   yarn dev
   # or
   pnpm dev
   ```

6. **Open your browser**
   Navigate to [http://localhost:3000](http://localhost:3000)

### Building for Production

```bash
npm run build
npm run start
```

## 📊 Database Schema

The Supabase PostgreSQL database includes tables for:
- **Users**: User profiles and authentication
- **Events**: Church events and activities
- **Articles**: Blog posts and articles
- **Testimonies**: Member testimonies
- **Gallery**: Media items and albums
- **Achievements**: Community achievements
- **Pages**: CMS-style page content
- **Settings**: Site configuration

## 🔐 Authentication & Authorization

- Public content accessible to all visitors
- Member-only areas protected with Supabase Auth
- Admin panel for content management
- Role-based access control (Admin, Editor, Member, Visitor)

## 🎨 Design System

- Consistent color palette reflecting church branding
- Typography system with readable fonts
- Reusable component library
- Responsive breakpoints for all device sizes
- Accessibility-first approach

## 🌟 Future Features

The project is designed to accommodate future enhancements:
- Member portal with personalized content
- Online giving and donations
- Prayer request system
- Small group finder
- Podcast integration
- Live streaming integration
- Newsletter subscription
- Multi-language support
- Advanced search functionality
- Social media integration
- Mobile app (React Native)

## 🤝 Contributing

We welcome contributions from the community! Whether you're fixing bugs, improving documentation, or proposing new features, your help is appreciated.

### Development Workflow

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Commit your changes (`git commit -m 'Add amazing feature'`)
5. Push to the branch (`git push origin feature/amazing-feature`)
6. Open a Pull Request

### Code Standards

- Follow the existing code style
- Write meaningful commit messages
- Add comments for complex logic
- Ensure responsive design
- Test on multiple devices and browsers
- Update documentation as needed

## 📝 Content Management

The website is designed for easy content management:
- Admin dashboard for content updates
- Markdown support for articles
- Media upload and management
- Event scheduling and management
- User-friendly interfaces for non-technical staff

## 🧪 Testing

```bash
# Run tests (when implemented)
npm run test

# Run linting
npm run lint

# Type checking
npm run type-check
```

## 📦 Deployment

### Recommended Deployment (Vercel)

1. Push code to GitHub
2. Connect repository to Vercel
3. Configure environment variables
4. Deploy automatically on push

### Alternative Deployments
- Netlify
- AWS Amplify
- Self-hosted with Docker

## 📞 Support & Contact

For questions, issues, or contributions:
- **Repository**: [github.com/GeraldoA07/gcc-website](https://github.com/GeraldoA07/gcc-website)
- **Issues**: [GitHub Issues](https://github.com/GeraldoA07/gcc-website/issues)
- **Discussions**: [GitHub Discussions](https://github.com/GeraldoA07/gcc-website/discussions)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built with ❤️ for the Grace Community Church community
- Thanks to all contributors and supporters
- Powered by open-source technologies

---

**Note**: This is a living document that will be updated as the project evolves. Suggestions for improvements are always welcome!
