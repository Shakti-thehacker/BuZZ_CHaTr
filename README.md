# BuZZ_CHaT - Real-time Group Chat Application

## Overview

BuZZ_CHaT is a modern, cyberpunk-themed real-time group chat application built with a full-stack TypeScript architecture. The application requires groups to be created before anyone can join them, featuring password-protected groups and real-time messaging with a distinctive neon aesthetic and Instagram integration.

## User Preferences

Preferred communication style: Simple, everyday language.
Responsive design: Fully optimized for smartphones, tablets, and desktop devices.
Privacy-focused: Automatically delete all chat data 5 minutes after everyone leaves a group.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter for lightweight client-side routing
- **UI Components**: Radix UI primitives with shadcn/ui components
- **Styling**: Tailwind CSS with custom cyberpunk theme and neon color palette
- **State Management**: TanStack Query (React Query) for server state management
- **Form Handling**: React Hook Form with Zod validation

### Backend Architecture
- **Runtime**: Node.js with Express.js
- **Language**: TypeScript with ES modules
- **Development**: tsx for development server with hot reload
- **Build**: esbuild for production bundling
- **API**: RESTful endpoints with JSON communication

### Database Layer
- **ORM**: Drizzle ORM with PostgreSQL dialect
- **Database**: PostgreSQL (configured for Neon Database)
- **Migrations**: Drizzle Kit for schema management
- **Connection**: @neondatabase/serverless for serverless PostgreSQL connections

## Key Components

### Database Schema
Located in `shared/schema.ts`, defines three main entities:
- **Groups**: Store group information (id, name, password, created_at)
- **Group Members**: Track user membership in groups (id, group_id, username, joined_at)
- **Messages**: Store chat messages (id, group_id, username, content, type, created_at)

### Storage Layer
Implements an abstraction layer (`IStorage`) with both in-memory (`MemStorage`) and future database implementations:
- Group operations (create, retrieve, auto-cleanup)
- Member management (add, remove, check membership)
- Message handling (create, retrieve, pagination)
- Privacy protection: Automatic deletion of all group data 5 minutes after last member leaves
- Cleanup cancellation: If someone rejoins within 5 minutes, cleanup is cancelled

### API Endpoints
- `POST /api/groups/create`: Create new group (mandatory before joining)
- `POST /api/groups/join`: Join existing group (group must exist first)
- `POST /api/groups/:id/leave`: Leave a group
- `POST /api/groups/:id/messages`: Send messages
- `GET /api/groups/:id/messages`: Retrieve group messages
- `GET /api/groups/:id/members`: Get group member list

### Frontend Components
- **JoinGroupModal**: Group authentication and creation interface with separate Create/Join buttons and Instagram link at top
- **ChatInterface**: Main chat experience with adaptive layout for desktop/mobile and Instagram footer
- **MessageList**: Real-time message display with responsive message bubbles and touch-optimized scrolling
- **UserList**: Online member sidebar with full-screen mobile overlay and desktop sidebar
- **MessageInput**: Message composition with mobile-optimized touch targets and form validation
- **ScannerEffect**: Cyberpunk scanning animation optimized for different screen sizes

## Data Flow

### Authentication Flow
1. User provides group name, username, and password
2. **Create Group**: Creates new group if it doesn't exist, adds creator as first member
3. **Join Group**: Validates group exists and password matches, then adds user to existing group
4. Groups must be created before others can join them
5. Adds appropriate join/creation message to chat

### Messaging Flow
1. User submits message through MessageInput component
2. API validates and stores message in database
3. TanStack Query automatically refetches messages (2-second polling)
4. MessageList updates with new messages and auto-scrolls
5. Real-time updates visible to all group members

### State Management
- Server state managed by TanStack Query with automatic refetching
- Local component state for UI interactions
- Form state handled by React Hook Form
- No global client state management needed

## External Dependencies

### UI and Styling
- **Radix UI**: Accessible component primitives
- **Tailwind CSS**: Utility-first styling framework
- **Lucide React**: Icon library
- **class-variance-authority**: Component variant management

### Development Tools
- **Vite**: Frontend build tool and development server
- **esbuild**: Fast TypeScript/JavaScript bundler
- **tsx**: TypeScript execution engine for development

### Database and Validation
- **Drizzle ORM**: Type-safe database toolkit
- **Zod**: Runtime type validation
- **date-fns**: Date manipulation utilities

## Deployment Strategy

### Development
- Frontend: Vite development server with HMR
- Backend: tsx with auto-restart on file changes
- Database: Local PostgreSQL or cloud-hosted Neon Database

### Production Build
1. Frontend builds to `dist/public` directory
2. Backend bundles to `dist/index.js` with esbuild
3. Static files served by Express in production
4. Environment variables for database connection

### Environment Configuration
- `NODE_ENV`: Controls development vs production behavior
- `DATABASE_URL`: PostgreSQL connection string
- Vite-specific environment variables for frontend configuration

### Replit Integration
- Configured with Replit-specific plugins for development
- Runtime error overlay for enhanced debugging
- Cartographer plugin for code mapping in development

## Recent Changes (July 2025)

### Group Creation System
- **Mandatory Group Creation**: Users must create groups before others can join them
- **Separate API Endpoints**: `/api/groups/create` for new groups, `/api/groups/join` for existing ones
- **Enhanced UI**: Join modal now has separate "Create Group" and "Join Group" buttons
- **Better Error Handling**: Clear messages when attempting to join non-existent groups

### Instagram Integration
- **Clickable Instagram Link**: @buzz__chat__sgi now redirects to Instagram profile
- **Strategic Placement**: Instagram link moved to top of join modal and bottom footer of chat interface
- **Improved UX**: Removed Instagram ID from chat header to reduce clutter

### Visual Updates
- **Purple Color Refresh**: Updated purple accent from violet (280°) to bright magenta (300°) for better contrast
- **Enhanced Neon Effects**: Improved visibility and cyberpunk aesthetic
- **Responsive Design**: Instagram links optimized for mobile and desktop

The application uses a monorepo structure with shared TypeScript definitions, enabling type safety across the full stack while maintaining clear separation between client and server code.
