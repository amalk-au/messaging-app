# Messaging App – Frontend

A web-based messaging application built with **React** and **Vite**. The frontend provides the user interface for account registration, authentication, contact management, private messaging, and profile management.

The application communicates with a REST API provided by the [backend](../backend/README.md).

## Table of Contents

- [Overview](#overview)

- [Features](#features)

- [Technology Stack](#technology-stack)

- [Project Structure](#project-structure)

- [Prerequisites](#prerequisites)

- [Getting Started](#getting-started)

- [Environment Variables](#environment-variables)

- [Available Scripts](#available-scripts)

- [Application Routes](#application-routes)

- [API Integration](#api-integration)

- [Authentication](#authentication)

- [Build](#build)

- [Deployment Configuration](#deployment-configuration)

- [Related Documentation](#related-documentation)

- [License](#license)

## Overview

The frontend is a single-page application (SPA) built using React 19 and Vite 6.

It uses React Router for client-side navigation and the browser Fetch API to communicate with the backend REST API.

Authentication is handled using JSON Web Tokens (JWT). The frontend stores the authentication token in browser `localStorage` and includes it in the `Authorization` header when making protected API requests.

## Features

### User Authentication

- User registration.

- User login.

- JWT-based authentication.

- Display of authentication status and user information.

- Logout functionality.

### Messaging

- Retrieve contacts.

- View messages associated with the logged-in user.

- Send messages to other users.

- View individual conversations.

### User Profiles

- Retrieve user profile information.

- Update user biography.

- Display user information decoded from the JWT payload.

### Application Navigation

- Client-side routing using React Router.

- Separate routes for authentication, contacts, conversations, and profile pages.

- Error page handling for application routes.

## Technology Stack

| Category             | Technology              |
| -------------------- | ----------------------- |
| UI Library           | React 19                |
| Build Tool           | Vite 6                  |
| Programming Language | JavaScript (ES Modules) |
| Routing              | React Router DOM 7      |
| HTTP Communication   | Browser Fetch API       |
| Authentication       | JSON Web Tokens (JWT)   |
| Styling              | CSS                     |
| Linting              | ESLint 9                |
| Deployment Utility   | gh-pages                |

## Project Structure

```
frontend/
├── public/                  # Static assets
├── src/
│   ├── components/
│   │   ├── Login.jsx        # User login form
│   │   ├── Register.jsx     # User registration form
│   │   ├── Contacts.jsx     # Contact management
│   │   ├── ContactList.jsx  # Contact list
│   │   ├── UserMessages.jsx # User messages
│   │   ├── ConversationView.jsx
│   │   ├── Profile.jsx     # User profile
│   │   ├── Layout.jsx      # Application layout
│   │   ├── JwtStatus.jsx   # JWT authentication status
│   │   └── ProjectInfo.jsx # Project information
│   │
│   ├── ApiQueries.js        # Backend API requests
│   ├── App.jsx              # Main application component
│   ├── routes.jsx           # Application route definitions
│   ├── ErrorPage.jsx        # Route error handling
│   ├── main.jsx             # Application entry point
│   ├── App.css              # Application styles
│   └── index.css            # Global styles
│
├── .gitignore
├── index.html
├── package.json
├── package-lock.json
├── vite.config.js
└── README.md
```

## Prerequisites

Make sure the following tools are installed:

- [Node.js](https://nodejs.org/) – JavaScript runtime.

- [pnpm](https://pnpm.io/) – Package manager.

- A running instance of the Messaging App backend.

The backend setup instructions are available in [`backend/README.md`](../backend/README.md).

## Getting Started

### 1\. Clone the repository

```
git clone https://github.com/amalk-au/messaging-app.git
```

### 2\. Navigate to the frontend directory

```
cd messaging-app/frontend
```

### 3\. Install dependencies

Using pnpm:

```
pnpm install
```

Alternatively, using npm:

```
npm ci
```

Use one package manager consistently for dependency installation and updates.

### 4\. Configure environment variables

Create a `.env` file in the `frontend` directory:

```
touch .env
```

Add the backend API URL:

```
VITE_API_URL=http://localhost:4000
```

The frontend uses this variable as the API origin. The API request functions append the `/messaging-api/v1` path.

For example:

```
http://localhost:4000/messaging-api/v1/contacts/login
```

Make sure the backend is running and accessible at the configured URL.

### 5\. Start the development server

```
pnpm dev
```

Vite will display the local development URL in the terminal.

With the current Vite base configuration, the application is served under:

```
http://localhost:5173/messaging-app/
```

Open the displayed URL in your browser.

## Environment Variables

The frontend uses Vite environment variables.

| Variable       | Description                        | Example                 |
| -------------- | ---------------------------------- | ----------------------- |
| `VITE_API_URL` | Base URL of the backend API server | `http://localhost:4000` |

**Important:**

- Vite exposes environment variables prefixed with `VITE_` to client-side code.

- Never store passwords, database credentials, JWT signing secrets, or other private credentials in frontend environment variables.

- Restart the Vite development server after changing environment variables.

- Do not commit `.env` files containing environment-specific configuration.

## Available Scripts

The following scripts are defined in `package.json`:

| Command           | Description                                         |
| ----------------- | --------------------------------------------------- |
| `pnpm dev`        | Starts the Vite development server.                 |
| `pnpm build`      | Builds the application for production.              |
| `pnpm preview`    | Locally previews the production build.              |
| `pnpm lint`       | Runs ESLint against the frontend source code.       |
| `pnpm run deploy` | Runs the configured GitHub Pages deployment script. |

Run the linter:

```
pnpm lint
```

Create a production build:

```
pnpm build
```

Preview the production build:

```
pnpm preview
```

## Application Routes

The application uses React Router DOM for client-side navigation.

| Route                        | Component          | Description              |
| ---------------------------- | ------------------ | ------------------------ |
| `/messaging-app`             | `App`              | Application landing page |
| `/messaging-app/projectinfo` | `ProjectInfo`      | Project information      |
| `/messaging-app/login`       | `Login`            | User login               |
| `/messaging-app/register`    | `Register`         | User registration        |
| `/messaging-app/profile`     | `Profile`          | User profile             |
| `/messaging-app/contacts`    | `Contacts`         | Contacts management      |
| `/messaging-app/messages`    | `UserMessages`     | User messages            |
| `/messaging-app/messageview` | `ConversationView` | Individual conversation  |
| `/messaging-app/contactlist` | `ContactList`      | Contact list             |

## API Integration

The frontend communicates with the backend using the Fetch API.

API request functions are centralised in:

```
src/ApiQueries.js
```

The backend API base path is:

```
/messaging-api/v1
```

The frontend integrates with the following endpoints:

| Method | Endpoint                                       | Description             |
| ------ | ---------------------------------------------- | ----------------------- |
| `POST` | `/contacts/register`                           | Register a new user     |
| `POST` | `/contacts/login`                              | Authenticate a user     |
| `GET`  | `/contacts`                                    | Retrieve contacts       |
| `GET`  | `/contacts/:loggedInUserID/messages`           | Retrieve messages       |
| `POST` | `/contacts/:loggedInUserID/message/:contactID` | Send a message          |
| `GET`  | `/contacts/:contactID/profile`                 | Retrieve a user profile |
| `PUT`  | `/contacts/:contactID/profile`                 | Update a user profile   |

Most endpoints require a valid JWT.

For protected requests, the frontend sends the token using the following HTTP header:

```
Authorization: Bearer <JWT_TOKEN>
```

For detailed request and response specifications, refer to the [backend API documentation](../backend/README.md).

## Authentication

The application uses JWT-based authentication.

### Login

When a user successfully logs in:

1.  The frontend sends the user's email and password to the backend.

2.  The backend validates the credentials.

3.  The backend returns a JWT.

4.  The frontend stores the JWT in browser `localStorage`.

The token is stored under:

```
localStorage.setItem("jwt", token);
```

### Authenticated Requests

The frontend retrieves the stored token and attaches it to protected API requests:

```
const token = localStorage.getItem("jwt");

const headers = {
  Authorization: `Bearer ${token}`,
};
```

### Logout

The frontend removes the stored JWT:

```
localStorage.removeItem("jwt");
```

**Security consideration:** Browser `localStorage` is accessible to JavaScript running on the page. For applications handling sensitive user data, consider evaluating an HttpOnly, Secure, SameSite cookie-based authentication approach and implementing appropriate protections against cross-site scripting (XSS).

## Build

Vite generates an optimised production build in the `dist/` directory.

Run:

```
pnpm build
```

The generated files can be found in:

```
frontend/dist/
```

To test the production build locally:

```
pnpm preview
```

### Vite Base Path

The current Vite configuration defines:

```
export default defineConfig({
  base: "/messaging-app",
  plugins: [react()],
});
```

This configures the application to be served from the `/messaging-app` path.

If deploying the frontend at a different path or domain root, update the `base` configuration accordingly.

## Deployment Configuration

The frontend includes `gh-pages` and a deployment script in `package.json`.

The current deployment configuration is intended for GitHub Pages and uses the `/messaging-app` base path.

The deployment script is:

```
{
  "predeploy": "npm run build",
  "deploy": "gh-pages -d dist"
}
```

The application is not currently published as a live demo. The deployment configuration is retained in the project for future use.

## Related Documentation

- [Main Repository](https://github.com/amalk-au/messaging-app)

- [Backend Documentation](../backend/README.md)

- [Backend API Reference](../backend/README.md)

## License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0)**.

See the repository's `LICENSE` file for the full license text.

## Contact

- **Author:** Amal K.

- **GitHub:** [@amalk-au](https://github.com/amalk-au)

- **Repository:** [messaging-app](https://github.com/amalk-au/messaging-app)

- **Portfolio:** [amalk.au](https://amalk.au)

- **Issues:** [GitHub Issues](https://github.com/amalk-au/messaging-app/issues)

---

Developed by [amalk-au](https://github.com/amalk-au)
