# NodeJS PostgreSQL Chat Application

A real-time chat application built with Node.js, Express, Socket.IO, and PostgreSQL.

## Architecture

![Architecture Diagram](https://github.com/ajeetraina/nodejs-postgres-chat-app/blob/main/docs/architecture.png)

## Features

- Real-time messaging using Socket.IO
- User authentication and authorization
- Persistent message storage in PostgreSQL
- Chat room functionality
- Message history
- Online/offline user status
- Typing indicators
- Read receipts

## Technology Stack

- **Frontend**: HTML, CSS, JavaScript, Bootstrap 5
- **Backend**: Node.js, Express.js
- **Real-time Communication**: Socket.IO
- **Database**: PostgreSQL
- **Authentication**: JWT (JSON Web Tokens)
- **Deployment**: Docker containerization support

## Prerequisites

- Node.js (v16 or higher)
- PostgreSQL (v14 or higher)
- npm or yarn

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/ajeetraina/nodejs-postgres-chat-app.git
cd nodejs-postgres-chat-app
```

### 2. Install dependencies

```bash
npm install
```

### 3. Set up environment variables

Create a `.env` file in the root directory:

```
# Server Configuration
PORT=3000
NODE_ENV=development

# Database Configuration
DB_HOST=localhost
DB_PORT=5432
DB_NAME=chat_app
DB_USER=postgres
DB_PASSWORD=your_password

# JWT Secret
JWT_SECRET=your_jwt_secret
JWT_EXPIRATION=1d
```

### 4. Set up the database

```bash
# Create database
createdb chat_app

# Run migrations
npm run migrate
```

### 5. Start the development server

```bash
npm run dev
```

Visit `http://localhost:3000` in your browser.

## Database Schema

The application uses the following database schema:

### Users Table
- `id`: UUID (Primary Key)
- `username`: VARCHAR (unique)
- `email`: VARCHAR (unique)
- `password`: VARCHAR (hashed)
- `avatar_url`: VARCHAR (nullable)
- `created_at`: TIMESTAMP
- `updated_at`: TIMESTAMP

### Rooms Table
- `id`: UUID (Primary Key)
- `name`: VARCHAR
- `description`: TEXT (nullable)
- `is_private`: BOOLEAN
- `created_by`: UUID (Foreign Key to Users)
- `created_at`: TIMESTAMP
- `updated_at`: TIMESTAMP

### Messages Table
- `id`: UUID (Primary Key)
- `room_id`: UUID (Foreign Key to Rooms)
- `user_id`: UUID (Foreign Key to Users)
- `content`: TEXT
- `created_at`: TIMESTAMP
- `updated_at`: TIMESTAMP

### Room_Members Table
- `room_id`: UUID (Foreign Key to Rooms)
- `user_id`: UUID (Foreign Key to Users)
- `joined_at`: TIMESTAMP
- PRIMARY KEY (`room_id`, `user_id`)

## API Endpoints

### Authentication
- `POST /api/auth/register`: Register a new user
- `POST /api/auth/login`: Login a user
- `POST /api/auth/logout`: Logout a user
- `GET /api/auth/me`: Get current user info

### Users
- `GET /api/users`: Get all users
- `GET /api/users/:id`: Get a specific user
- `PUT /api/users/:id`: Update user information
- `DELETE /api/users/:id`: Delete a user

### Rooms
- `GET /api/rooms`: Get all rooms
- `POST /api/rooms`: Create a new room
- `GET /api/rooms/:id`: Get a specific room
- `PUT /api/rooms/:id`: Update room information
- `DELETE /api/rooms/:id`: Delete a room
- `GET /api/rooms/:id/members`: Get all members of a room
- `POST /api/rooms/:id/members`: Add a member to a room
- `DELETE /api/rooms/:id/members/:userId`: Remove a member from a room

### Messages
- `GET /api/rooms/:roomId/messages`: Get all messages in a room
- `POST /api/rooms/:roomId/messages`: Create a new message
- `DELETE /api/messages/:id`: Delete a message

## Socket.IO Events

### Client → Server
- `join_room`: Join a chat room
- `leave_room`: Leave a chat room
- `send_message`: Send a message to a room
- `typing_start`: User started typing
- `typing_end`: User stopped typing
- `read_messages`: Mark messages as read

### Server → Client
- `user_joined`: A user joined the room
- `user_left`: A user left the room
- `receive_message`: Receive a message
- `user_typing`: A user is typing
- `user_status_change`: A user's online status changed

## Docker Support

### Build the Docker image

```bash
docker build -t nodejs-postgres-chat-app .
```

### Run with Docker Compose

```bash
docker-compose up -d
```

## Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add some amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
