# Messaging App

A full-stack web messaging application that allows registered users to communicate, manage contacts, send messages and maintain their profiles through a React frontend and Node.js REST API.

## Table of Contents

- [Overview](#overview)

- [Features](#features)

- [Screenshots](#screenshots)

- [Live Demo](#live-demo)

- [Tech Stack](#tech-stack)

- [Architecture](#architecture)

- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)

  - [Installation](#installation)

  - [Environment Variables](#environment-variables)

- [Project Structure](#project-structure)

- [Available Scripts](#available-scripts)

- [Development](#development)

- [Testing](#testing)

- [API Reference](#api-reference)

- [Deployment](#deployment)

- [Troubleshooting](#troubleshooting)

- [Contributing](#contributing)

- [License](#license)

- [Acknowledgements](#acknowledgements)

- [Contact](#contact)

---

## Overview

Messaging App is a full-stack web application with authentication, authorisation, REST API and relational database management.

The application provides a web interface where registered users can discover contacts, send and receive messages, and manage their profiles.

The project uses a React frontend communicating with a Node.js and Express backend. The backend exposes a REST API, handles authentication using JSON Web Tokens (JWT), and manages persistent application data through PostgreSQL and Prisma ORM.

The frontend and backend are maintained in separate directories within the same repository.

## Features

- **User Registration:** Register an account using a name, email address, password and profile bio.

- **User Authentication:** Authenticate registered users using JWT-based authentication.

- **Protected API Routes:** Restrict access to protected endpoints using JWT authentication middleware.

- **Contact Management:** Retrieve contacts and access messaging-related information.

- **Messaging:** Send messages to other registered contacts.

- **Message Management:** Retrieve messages associated with the authenticated user's account.

- **User Profiles:** View and update profile information, including the user's bio.

- **Persistent Storage:** Store user accounts and messages in a PostgreSQL database.

- **Responsive Web Interface:** Access the application through a browser-based React interface.

## Screenshots

<p align="center">
  <img src="./docs/screenshots/main-view.png" alt="Main View" width="800" />
  <img src="./docs/screenshots/login.png" alt="Login" width="400" />
  <img src="./docs/screenshots/register.png" alt="Register" width="400" />
  <img src="./docs/screenshots/profile.png" alt="Profile" width="800" />
  <img src="./docs/screenshots/jwt-status.png" alt="JWT Status" width="800" />
  <img src="./docs/screenshots/all-messages-view.png" alt="All Messages" width="800" />
  <img src="./docs/screenshots/all-contacts.png" alt="All Contacts" width="800" />
  <img src="./docs/screenshots/new-message.png" alt="New Message" width="400" />
    <img src="./docs/screenshots/docs.png" alt="New Message" width="800" />
</p>

> **Deployment note:** The frontend uses React Router with browser-based routing. Directly accessing nested routes, such as `/send`, may result in a 404 response depending on the hosting configuration. Use the application's internal navigation when accessing the live demo.
>
> The backend and database use separately hosted services.

## Tech Stack

| Category                  | Technology                      |
| ------------------------- | ------------------------------- |
| Frontend                  | React 19                        |
| Frontend Tooling          | Vite 6                          |
| Routing                   | React Router 7                  |
| Backend Runtime           | Node.js                         |
| Backend Framework         | Express 5                       |
| Language                  | JavaScript (ES Modules)         |
| Markup                    | HTML5                           |
| Styling                   | CSS3                            |
| Database                  | PostgreSQL                      |
| ORM                       | Prisma 6                        |
| Authentication            | JSON Web Tokens (JWT)           |
| Password Hashing          | bcryptjs                        |
| API Communication         | REST API                        |
| Middleware                | CORS, Express                   |
| Environment Configuration | dotenv                          |
| Package Manager           | pnpm (preferred)                |
| API Development           | Postman                         |
| Frontend Deployment       | GitHub Pages / portfolio domain |
| Backend Hosting           | Render                          |
| Database Hosting          | Neon                            |

## Architecture

The application follows a separated frontend/backend architecture.

### Frontend

The frontend is a single-page application (SPA) built with React and Vite.

React Router manages client-side navigation, while the frontend communicates with the backend through HTTP requests.

The application provides the interfaces for:

- User registration and login.

- Contact and message management.

- Sending and retrieving messages.

- Viewing and updating user profiles.

- Displaying authentication status.

### Backend

The backend is a REST API built with Node.js and Express.

Its responsibilities include:

- Handling authentication and registration requests.

- Generating and validating JWT access tokens.

- Protecting authenticated API endpoints.

- Processing contact and messaging operations.

- Retrieving and updating user profile information.

- Interacting with PostgreSQL through Prisma ORM.

### Database

PostgreSQL provides persistent storage.

Prisma ORM manages database access and defines the application's data models.

The current database schema includes:

| Model     | Description                                                               |
| --------- | ------------------------------------------------------------------------- |
| `Contact` | Stores user account information, including name, email, password and bio. |
| `Message` | Stores message content, sender, recipient and timestamp.                  |

Messages are associated with their sender and recipient through relational fields.

### Authentication Flow

1.  A user submits their login credentials through the frontend.

2.  The backend validates the credentials.

3.  The backend generates a JWT containing authenticated user information.

4.  The frontend receives the JWT.

5.  Subsequent protected API requests include the token in the `Authorization` header using the `Bearer` scheme.

6.  Backend authentication middleware validates the token before allowing access to protected routes.

JWT tokens are configured with a 30-minute expiration period.

## Getting Started

Follow these instructions to set up the frontend, backend and database locally.

### Prerequisites

Ensure the following tools are installed:

| Requirement | Description                                                  |
| ----------- | ------------------------------------------------------------ |
| Node.js     | JavaScript runtime compatible with the project dependencies. |
| pnpm        | Preferred package manager.                                   |
| PostgreSQL  | Database server.                                             |
| Git         | Version control and repository cloning.                      |

You can verify your installations using:

```
node --version
pnpm --version
git --version
```

### Installation

#### 1\. Clone the repository

```
git clone https://github.com/amalk-au/messaging-app.git

cd messaging-app
```

#### 2\. Install frontend dependencies

```
cd frontend

pnpm install
```

#### 3\. Install backend dependencies

```
cd ../backend

pnpm install
```

#### 4\. Configure environment variables

Create a `.env` file inside the `backend/` directory.

```
cd ../backend

touch .env
```

Configure the required environment variables as described in the [Environment Variables](#environment-variables) section.

#### 5\. Generate the Prisma Client

From the repository root:

```
pnpm --dir backend exec prisma generate --schema src/prisma/schema.prisma
```

#### 6\. Configure the database

Ensure your PostgreSQL database is running and the `DATABASE_URL` points to the correct database.

Synchronise the database schema:

```
pnpm --dir backend exec prisma db push --schema src/prisma/schema.prisma
```

#### 7\. Seed sample data (Optional)

The backend provides a sample data seeding script.

```
pnpm --dir backend run seed
```

#### 8\. Start the application

Start the backend in one terminal:

```
pnpm --dir backend run dev
```

Start the frontend in another terminal:

```
pnpm --dir frontend run dev
```

Vite will display the local development URL in the terminal.

### Environment Variables

The backend uses environment variables for database connectivity, authentication and cross-origin requests.

Create `backend/.env` with the following configuration:

| Variable         | Description                                                | Required |
| ---------------- | ---------------------------------------------------------- | -------- |
| `DATABASE_URL`   | PostgreSQL database connection string used by Prisma.      | Yes      |
| `JWT_SECRET_KEY` | Secret used to sign and verify JWT authentication tokens.  | Yes      |
| `CORS_URL`       | Allowed frontend origin for cross-origin API requests.     | Yes      |
| `PORT`           | Backend server port. Defaults to `4000` when not provided. | No       |

Example configuration:

```
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE?schema=public"

JWT_SECRET_KEY="your-secure-secret"

CORS_URL="http://localhost:5173"

PORT=4000
```

Replace the example database credentials and JWT secret with your actual configuration.

The frontend also requires its API base URL to point to the running backend. Verify the existing frontend API configuration before changing it; its exact environment-variable configuration should be confirmed from the frontend source.

> Never commit `.env` files, database credentials, JWT secrets or other sensitive configuration to version control.

## Project Structure

The repository is organised into separate frontend and backend applications.

```
messaging-app/
│
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   ├── routes.jsx
│   │   ├── App.css
│   │   └── index.css
│   ├── index.html
│   ├── package.json
│   └── vite.config.js
│
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   │   └── mainController.js
│   │   ├── middleware/
│   │   │   └── Authenticator.js
│   │   ├── prisma/
│   │   │   ├── schema.prisma
│   │   │   ├── PrismaQueries.js
│   │   │   └── seedSampleData.js
│   │   ├── routes/
│   │   │   └── mainRoutes.js
│   │   └── index.js
│   └── package.json
│
├── docs/
│   └── screenshots/
│
├── .gitignore
├── LICENSE
└── README.md
```

The structure above highlights the main application files and directories. Additional files may exist within the source directories.

## Available Scripts

The frontend and backend maintain independent `package.json` files. Run commands from the repository root using `pnpm --dir`.

### Frontend

| Command                           | Description                                              |
| --------------------------------- | -------------------------------------------------------- |
| `pnpm --dir frontend run dev`     | Start the Vite development server.                       |
| `pnpm --dir frontend run build`   | Build the frontend for production.                       |
| `pnpm --dir frontend run lint`    | Run ESLint.                                              |
| `pnpm --dir frontend run preview` | Preview the production build locally.                    |
| `pnpm --dir frontend run deploy`  | Build and deploy the `dist/` directory using `gh-pages`. |

### Backend

| Command                         | Description                                 |
| ------------------------------- | ------------------------------------------- |
| `pnpm --dir backend run start`  | Start the production backend server.        |
| `pnpm --dir backend run dev`    | Start the backend using Node.js watch mode. |
| `pnpm --dir backend run seed`   | Execute the sample data seeding script.     |
| `pnpm --dir backend run studio` | Open Prisma Studio.                         |

### Prisma Commands

| Command                                                                     | Description                                      |
| --------------------------------------------------------------------------- | ------------------------------------------------ |
| `pnpm --dir backend exec prisma generate --schema src/prisma/schema.prisma` | Generate the Prisma Client.                      |
| `pnpm --dir backend exec prisma db push --schema src/prisma/schema.prisma`  | Synchronise the database schema with PostgreSQL. |

> The repository does not currently define a root-level package manager workspace. Frontend and backend commands should therefore be executed independently.

## Development

Follow these guidelines when contributing to the project.

1.  Create a feature branch before implementing changes.

2.  Keep frontend and backend responsibilities separated.

3.  Follow existing JavaScript and ES Module conventions.

4.  Use Prisma ORM for database interactions.

5.  Protect authenticated API endpoints using the existing JWT middleware.

6.  Avoid exposing sensitive configuration or authentication credentials.

7.  Keep API responses consistent with the existing implementation.

8.  Run the frontend linter and production build before submitting changes.

9.  Test backend functionality manually or with API development tools such as Postman.

Keep changes focused, maintainable and well-documented.

## Testing

The frontend currently provides ESLint as its configured code-quality check.

Run:

```
pnpm --dir frontend run lint
```

Build the frontend to identify production compilation issues:

```
pnpm --dir frontend run build
```

The backend package currently contains a placeholder `test` script rather than an implemented automated test suite.

Backend functionality can be tested manually using Postman or another HTTP client.

Recommended areas for testing include:

- User registration.

- User login and JWT generation.

- Authentication of protected endpoints.

- Contact retrieval.

- Message retrieval and sending.

- Profile retrieval and updates.

- Database connectivity and persistence.

Automated frontend and backend test suites have not been documented because corresponding test implementations are not currently configured.

## API Reference

The backend exposes a REST API under the following path:

```
/messaging-api/v1
```

The default local backend URL is:

```
http://localhost:4000
```

### Endpoints

| Method | Endpoint                                       | Authentication | Description                               |
| ------ | ---------------------------------------------- | -------------- | ----------------------------------------- |
| GET    | `/status`                                      | No             | Check backend availability.               |
| POST   | `/contacts/login`                              | No             | Authenticate a user and generate a JWT.   |
| POST   | `/contacts/register`                           | No             | Register a new user.                      |
| GET    | `/contacts`                                    | Yes            | Retrieve contacts.                        |
| GET    | `/contacts/:loggedInUserID/messages`           | Yes            | Retrieve messages associated with a user. |
| POST   | `/contacts/:loggedInUserID/message/:contactID` | Yes            | Send a message to a contact.              |
| GET    | `/contacts/:contactID/profile`                 | Yes            | Retrieve a user profile.                  |
| PUT    | `/contacts/:contactID/profile`                 | Yes            | Update a user profile.                    |

All endpoints are prefixed with `/messaging-api/v1`.

### Authentication

Protected endpoints require a JWT in the request header:

```
Authorization: Bearer <JWT>
```

### Request Examples

**Register a user**

```
POST /messaging-api/v1/contacts/register
Content-Type: application/json
```

```
{
  "name": "John",
  "email": "john@example.com",
  "password": "password",
  "bio": "Hello, I'm John."
}
```

**Login**

```
POST /messaging-api/v1/contacts/login
Content-Type: application/json
```

```
{
  "email": "john@example.com",
  "password": "password"
}
```

**Send a message**

```
POST /messaging-api/v1/contacts/1/message/2
Authorization: Bearer <JWT>
Content-Type: application/json
```

```
{
  "message": "Hello!"
}
```

**Update a profile**

```
PUT /messaging-api/v1/contacts/1/profile
Authorization: Bearer <JWT>
Content-Type: application/json
```

```
{
  "contactID": "1",
  "updatedBio": "Updated profile information."
}
```

> Endpoint availability and request formats are based on the current backend route and controller implementations.

## Deployment

The application uses separate deployment targets for its frontend, backend and database.

| Component | Hosting                         | Configuration          |
| --------- | ------------------------------- | ---------------------- |
| Frontend  | GitHub Pages / Portfolio domain | Vite production build. |
| Backend   | Render                          | Node.js application.   |
| Database  | Neon                            | PostgreSQL database.   |

### Frontend Deployment

The frontend is configured for deployment under the `/messaging-app` subdirectory.

This is configured in `frontend/vite.config.js`:

```
export default defineConfig({
  base: '/messaging-app',
  plugins: [react()],
})
```

Build the frontend:

```
pnpm --dir frontend run build
```

The production files are generated in:

```
frontend/dist/
```

The frontend package also provides a deployment script:

```
pnpm --dir frontend run deploy
```

This script uses `gh-pages` to publish the generated `dist/` directory.

The deployed application should be accessible at:

[https://www.amalk.au/messaging-app](https://www.amalk.au/messaging-app)

Ensure the hosting configuration supports the portfolio's custom domain and the `/messaging-app` base path.

### Backend Deployment

The backend is configured to start using:

```
pnpm --dir backend run start
```

The server listens on the port specified by `PORT`, defaulting to `4000`.

Configure the production environment variables on the hosting provider:

- `DATABASE_URL`

- `JWT_SECRET_KEY`

- `CORS_URL`

- `PORT` (if required by the hosting platform)

Ensure the production database is accessible and the Prisma Client has been generated.

### Database Deployment

The project uses PostgreSQL, with Neon identified as the database hosting provider.

Configure the production `DATABASE_URL` and synchronise the Prisma schema with the production database as part of the deployment process.

Avoid running development database commands against production without reviewing the potential impact.

## Troubleshooting

| Issue                                       | Possible Solution                                                                                                                                   |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Frontend dependencies fail to install       | Verify Node.js and pnpm versions, then reinstall dependencies in the frontend directory.                                                            |
| Backend dependencies fail to install        | Verify Node.js and pnpm versions, then reinstall dependencies in the backend directory.                                                             |
| Database connection fails                   | Check PostgreSQL availability and verify `DATABASE_URL`.                                                                                            |
| Prisma Client errors                        | Run Prisma Client generation using the project's schema path.                                                                                       |
| JWT authentication fails                    | Verify `JWT_SECRET_KEY` is configured consistently across backend environments.                                                                     |
| CORS errors                                 | Ensure `CORS_URL` matches the frontend origin.                                                                                                      |
| Backend fails to start                      | Verify backend dependencies, environment variables and database configuration.                                                                      |
| Frontend cannot communicate with backend    | Check the frontend API base URL and backend availability.                                                                                           |
| Nested routes return 404                    | Navigate through the application instead of directly accessing nested routes. Check hosting rewrite configuration if direct navigation is required. |
| First request to backend is slow            | Free hosting tiers may introduce delays when services are inactive. Allow time for the backend to start.                                            |
| Frontend deployment displays missing assets | Verify the Vite `base` configuration matches the deployed subdirectory.                                                                             |

## Contributing

Contributions, improvements and suggestions are welcome.

To contribute:

1.  Fork the repository.

2.  Create a feature branch.

3.  Implement your changes following the existing project conventions.

4.  Test the affected functionality.

5.  Commit your changes with a descriptive commit message.

6.  Open a pull request describing your changes.

For significant changes, consider opening an issue first to discuss the proposed implementation.

## License

This project is licensed under the GNU General Public License v3.0 (GPL-3.0).

You are free to use, modify and distribute this software under the terms of the licence.

See [LICENSE](LICENSE) for the full licence text.

## Acknowledgements

- [React](https://react.dev/) — Frontend application framework.

- [Vite](https://vite.dev/) — Frontend development and build tooling.

- [Express](https://expressjs.com/) — Backend web framework.

- [Prisma](https://www.prisma.io/) — Database ORM.

- [PostgreSQL](https://www.postgresql.org/) — Relational database.

- [Neon](https://neon.tech/) — PostgreSQL hosting.

- [Render](https://render.com/) — Backend hosting.

- [GitHub Pages](https://pages.github.com/) — Frontend deployment.

## Contact

- **Author:** Amal K.

- **GitHub:** [@amalk-au](https://github.com/amalk-au)

- **Repository:** [messaging-app](https://github.com/amalk-au/messaging-app)

- **Portfolio:** [amalk.au](https://amalk.au)

- **Issues:** [GitHub Issues](https://github.com/amalk-au/messaging-app/issues)

---

Developed by [amalk-au](https://github.com/amalk-au)
