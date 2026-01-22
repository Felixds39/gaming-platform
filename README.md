# Gaming Platform

A full-stack gaming platform with authentication, admin dashboard, and embeddable games.

## Features

- 🎮 **Game Library** - Browse and play published games
- 🔐 **User Authentication** - Register and login
- 👨‍💼 **Admin Dashboard** - Create, edit, and publish games
- 🎯 **Embeddable Games** - Support for HTML5 games via iframe
- 📱 **Responsive Design** - Works on desktop and mobile

## Tech Stack

- **Frontend**: React + Vite + React Router
- **Backend**: Node.js + Express
- **Database**: SQLite
- **Authentication**: JWT (jsonwebtoken)

## Getting Started

### Prerequisites
- Node.js (v16+)
- npm or yarn

### Installation

```bash
# Install server dependencies
cd server
npm install

# Install client dependencies
cd ../client
npm install
```

### Configuration

Create `server/.env`:
```
PORT=4000
JWT_SECRET=your-long-secret-key
ADMIN_BOOTSTRAP_CODE=CREATE_FIRST_ADMIN_123
```

Create `client/.env`:
```
VITE_API_BASE=http://localhost:4000
```

### Running Locally

**Terminal 1 - Backend:**
```bash
cd server
npm run dev
```

**Terminal 2 - Frontend:**
```bash
cd client
npm run dev
```

Open http://localhost:5173

## Workflow

1. **Register** a user account
2. **Login** with credentials
3. Go to "Admin Setup" → promote yourself with bootstrap code
4. Access **Admin Dashboard** to create/publish games
5. Games appear on Home page for all users
6. Click games to play with embedded iframe

## Games

Included: **THE IRON TRIBE • Confident in Christ** - Bible journey game (6 steps, ages 12-25)

Located in `client/public/games/iron-tribe.html`

## API Endpoints

### Auth
- `POST /api/auth/register` - Create account
- `POST /api/auth/login` - Login
- `POST /api/auth/bootstrap-admin` - Promote user to admin

### Games (Public)
- `GET /api/games` - List published games
- `GET /api/games/:id` - Get game details

### Admin
- `GET /api/admin/games` - List all games
- `POST /api/admin/games` - Create game
- `PUT /api/admin/games/:id` - Update game
- `DELETE /api/admin/games/:id` - Delete game
- `POST /api/admin/games/:id/publish` - Publish/unpublish game

## License

MIT
