# Messaging App — Backend

The backend service for the Messaging App. It provides a REST API for user registration, authentication, contact retrieval, messaging, and profile management.

The API is built with **Node.js**, **Express 5**, **PostgreSQL**, and **Prisma ORM**. Authentication is handled using JSON Web Tokens (JWT), and user passwords are hashed using bcrypt.

## Tech Stack

| Technology     | Purpose                               |
| -------------- | ------------------------------------- |
| Node.js        | JavaScript runtime                    |
| Express 5      | REST API framework                    |
| PostgreSQL     | Relational database                   |
| Prisma ORM 6   | Database access and schema management |
| JSON Web Token | API authentication                    |
| bcryptjs       | Password hashing                      |
| dotenv         | Environment variable management       |
| CORS           | Cross-origin resource sharing         |

## Project Structure

```
backend/
├── src/
│   ├── controllers/
│   │   └── mainController.js
│   ├── middleware/
│   │   └── Authenticator.js
│   ├── prisma/
│   │   ├── generated/
│   │   │   └── prisma/
│   │   ├── schema.prisma
│   │   ├── PrismaQueries.js
│   │   └── seedSampleData.js
│   ├── routes/
│   │   └── mainRoutes.js
│   └── index.js
├── .env
├── package.json
└── README.md
```

### Directory Overview

- `controllers/` — Handles incoming API requests and sends responses.

- `middleware/` — Contains JWT authentication middleware.

- `prisma/` — Contains the Prisma schema, database queries, generated Prisma Client, and sample data seed script.

- `routes/` — Defines the API endpoints and associates them with controllers.

- `index.js` — Configures and starts the Express server.

## Getting Started

### Prerequisites

Before running the backend, ensure you have installed:

- [Node.js](https://nodejs.org/)

- [pnpm](https://pnpm.io/)

- [PostgreSQL](https://www.postgresql.org/)

You will also need access to a PostgreSQL database.

### 1\. Install Dependencies

From the project root, navigate to the backend directory:

```
cd backend
```

Install the dependencies:

```
pnpm install
```

### 2\. Configure Environment Variables

Create a `.env` file in the `backend/` directory:

```
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE?schema=public"
JWT_SECRET_KEY="your-secure-secret-key"
CORS_URL="http://localhost:5173"
PORT=4000
```

| Variable         | Description                                          |
| ---------------- | ---------------------------------------------------- |
| `DATABASE_URL`   | PostgreSQL connection string used by Prisma.         |
| `JWT_SECRET_KEY` | Secret key used to sign and verify JWTs.             |
| `CORS_URL`       | Frontend origin permitted to access the API.         |
| `PORT`           | Port used by the Express server. Defaults to `4000`. |

Replace the database connection details with your own PostgreSQL credentials.

Generate a strong, private value for `JWT_SECRET_KEY`. Do not commit your `.env` file or expose this secret.

If your frontend runs on a different port or origin, update `CORS_URL` accordingly.

### 3\. Generate Prisma Client

Generate the Prisma Client using the project's schema:

```
pnpm exec prisma generate --schema src/prisma/schema.prisma
```

### 4\. Set Up the Database

Apply the Prisma schema to your development database:

```
pnpm exec prisma db push --schema src/prisma/schema.prisma
```

This synchronises the database structure with the Prisma schema.

Alternatively, if you are using Prisma migrations, create and apply a migration:

```
pnpm exec prisma migrate dev --schema src/prisma/schema.prisma --name init
```

Use one approach consistently for your database setup.

### 5\. Start the Backend

Start the development server:

```
pnpm dev
```

Or start the server normally:

```
pnpm start
```

The backend runs at:

```
http://localhost:4000
```

Check that the API is running:

```
curl http://localhost:4000/messaging-api/v1/status
```

A successful response includes the backend status and a list of available API endpoints.

## Available Scripts

The following scripts are defined in `package.json`:

| Command       | Description                                 |
| ------------- | ------------------------------------------- |
| `pnpm start`  | Starts the backend server.                  |
| `pnpm dev`    | Starts the backend with Node.js watch mode. |
| `pnpm studio` | Opens Prisma Studio.                        |
| `pnpm seed`   | Runs the sample data seed script.           |

To open Prisma Studio:

```
pnpm studio
```

Prisma Studio provides a browser-based interface for viewing and managing database records.

To run the sample data seed script:

```
pnpm seed
```

## API Documentation

### Base URL

For local development:

```
http://localhost:4000
```

All API endpoints use the following prefix:

```
/messaging-api/v1
```

### Authentication

The API uses JSON Web Tokens (JWT) to protect authenticated endpoints.

To authenticate:

1.  Register a user or log in using an existing account.

2.  Obtain the JWT returned by the login endpoint.

3.  Include the token in the `Authorization` header when accessing protected endpoints.

Header format:

```
Authorization: Bearer YOUR_JWT_TOKEN
```

Tokens expire after **30 minutes**.

### Endpoint Overview

| Method | Endpoint                                                        | Authentication | Description                                        |
| ------ | --------------------------------------------------------------- | -------------- | -------------------------------------------------- |
| GET    | `/messaging-api/v1/status`                                      | Public         | Check backend status and list available endpoints. |
| POST   | `/messaging-api/v1/contacts/register`                           | Public         | Register a new user.                               |
| POST   | `/messaging-api/v1/contacts/login`                              | Public         | Authenticate a user and obtain a JWT.              |
| GET    | `/messaging-api/v1/contacts`                                    | Required       | Retrieve contacts.                                 |
| GET    | `/messaging-api/v1/contacts/:loggedInUserID/messages`           | Required       | Retrieve messages associated with a user.          |
| POST   | `/messaging-api/v1/contacts/:loggedInUserID/message/:contactID` | Required       | Send a message to another contact.                 |
| GET    | `/messaging-api/v1/contacts/:contactID/profile`                 | Required       | Retrieve a user's profile.                         |
| PUT    | `/messaging-api/v1/contacts/:contactID/profile`                 | Required       | Update a user's profile bio.                       |

---

### 1\. Check API Status

**GET**`/messaging-api/v1/status`

Public endpoint used to check whether the backend is running.

Example:

```
curl http://localhost:4000/messaging-api/v1/status
```

A successful response contains:

```
{
  "status": "Backend Systems running ok",
  "response": [
    "/messaging-api/v1/status : GET End Point to show the running status of backend systems",
    "/messaging-api/v1/contacts/login : POST End Point to login to Messaging App",
    "/messaging-api/v1/contacts/register : POST End Point to Register new user"
  ]
}
```

The actual response also lists the authenticated contact, messaging, and profile endpoints.

### 2\. Register a User

**POST**`/messaging-api/v1/contacts/register`

Public endpoint used to create a new user account.

Request body:

```
{
  "name": "John Smith",
  "email": "john@example.com",
  "password": "password123",
  "bio": "Hello, I'm John."
}
```

Successful response:

```
{
  "response": "User Registered successfully !"
}
```

Passwords are hashed using bcrypt before being stored in the database.

### 3\. Login

**POST**`/messaging-api/v1/contacts/login`

Public endpoint used to authenticate an existing user.

Request body:

```
{
  "email": "john@example.com",
  "password": "password123"
}
```

Successful response:

```
{
  "jwt": "YOUR_JWT_TOKEN"
}
```

The returned JWT contains user-related claims and expires after 30 minutes.

Use this token in the `Authorization` header when accessing protected endpoints.

### 4\. Get Contacts

**GET**`/messaging-api/v1/contacts`

Authenticated endpoint used to retrieve contacts.

Request headers:

```
Authorization: Bearer YOUR_JWT_TOKEN
```

The controller returns a response containing a status and the contacts retrieved from the database.

### 5\. Get User Messages

**GET**`/messaging-api/v1/contacts/:loggedInUserID/messages`

Authenticated endpoint used to retrieve messages sent and received by a user.

Example:

```
GET /messaging-api/v1/contacts/1/messages
```

Request headers:

```
Authorization: Bearer YOUR_JWT_TOKEN
```

Example response structure:

```
{
  "status": "Get logged in users messages",
  "response": {
    "status": "Messages fetched successfully",
    "response": [
      {
        "id": 1,
        "message": "Hello!",
        "contactIdSender": 1,
        "contactIdReceiver": 2,
        "time": "2026-01-01T10:00:00.000Z",
        "senderName": "John Smith",
        "receiverName": "Jane Smith",
        "senderEmail": "john@example.com",
        "receiverEmail": "jane@example.com"
      }
    ]
  }
}
```

Messages are returned in chronological order.

### 6\. Send a Message

**POST**`/messaging-api/v1/contacts/:loggedInUserID/message/:contactID`

Authenticated endpoint used to send a message to another contact.

Example:

```
POST /messaging-api/v1/contacts/1/message/2
```

Request headers:

```
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json
```

Request body:

```
{
  "message": "Hello Jane!"
}
```

Successful response:

```
{
  "response": "Message Sent"
}
```

The sender and receiver IDs are supplied through the URL parameters.

### 7\. Get User Profile

**GET**`/messaging-api/v1/contacts/:contactID/profile`

Authenticated endpoint used to retrieve a user's profile information.

Example:

```
GET /messaging-api/v1/contacts/1/profile
```

Request headers:

```
Authorization: Bearer YOUR_JWT_TOKEN
```

Example response:

```
{
  "status": "Load logged in users profile",
  "response": {
    "email": "john@example.com",
    "bio": "Hello, I'm John."
  }
}
```

### 8\. Update User Profile

**PUT**`/messaging-api/v1/contacts/:contactID/profile`

Authenticated endpoint used to update a user's profile bio.

Example:

```
PUT /messaging-api/v1/contacts/1/profile
```

Request headers:

```
Authorization: Bearer YOUR_JWT_TOKEN
Content-Type: application/json
```

Request body:

```
{
  "contactID": 1,
  "updatedBio": "Updated profile bio."
}
```

Successful response:

```
{
  "response": "User Profile Updated!"
}
```

---

## Database Schema

The application uses PostgreSQL with Prisma ORM.

The Prisma schema is located at:

```
src/prisma/schema.prisma
```

It currently defines two models.

### Contact

Stores registered user information.

| Field              | Type        | Description                    |
| ------------------ | ----------- | ------------------------------ |
| `id`               | Int         | Unique user identifier.        |
| `name`             | String      | User's name.                   |
| `email`            | String      | Unique email address.          |
| `password`         | String      | Hashed password.               |
| `bio`              | String      | User profile biography.        |
| `messagesSent`     | Message\[\] | Messages sent by the user.     |
| `messagesReceived` | Message\[\] | Messages received by the user. |

### Message

Stores messages exchanged between contacts.

| Field               | Type     | Description                 |
| ------------------- | -------- | --------------------------- |
| `id`                | Int      | Unique message identifier.  |
| `message`           | String   | Message content.            |
| `contactIdSender`   | Int      | ID of the sender.           |
| `contactIdReceiver` | Int      | ID of the receiver.         |
| `time`              | DateTime | Message creation timestamp. |

The `Contact` and `Message` models have separate relations for sent and received messages.

## Error Responses

The authentication middleware returns the following responses.

### Missing JWT

HTTP status: `401 Unauthorized`

```
{
  "message": "Access denied. No token provided."
}
```

### Invalid or Expired JWT

HTTP status: `403 Forbidden`

```
{
  "message": "Forbidden: Invalid or expired token"
}
```

Other controller errors are currently handled individually and may return different response structures.

## Testing the API

You can test the API using:

- [Postman](https://www.postman.com/) — GUI-based API testing.

- [Postman CLI](https://learning.postman.com/docs/postman-cli/postman-cli-overview/) — Run Postman collections from the command line.

- `curl` — Send HTTP requests directly from the terminal.

Example status check:

```
curl -i http://localhost:4000/messaging-api/v1/status
```

For authenticated endpoints, include the JWT:

```
curl -i \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  http://localhost:4000/messaging-api/v1/contacts
```

## Deployment

The backend can be deployed independently of the frontend.

When deploying:

1.  Configure a PostgreSQL database.

2.  Set the required environment variables in the hosting provider.

3.  Install the production dependencies.

4.  Generate the Prisma Client.

5.  Apply the database schema or migrations.

6.  Start the server using:

```
pnpm start
```

Ensure the frontend's origin is configured in `CORS_URL` and that the production database connection and JWT secret are configured securely.

## Known Limitations

The current implementation has some areas that could be improved:

- Request validation and error handling are not yet standardised across all endpoints.

- Authenticated routes verify the JWT, but the controllers should also verify that requested user IDs belong to the authenticated user.

- The contact retrieval query should explicitly select permitted fields to avoid exposing password hashes.

- The login flow should handle non-existent users and invalid credentials consistently.

- Automated backend tests have not yet been configured.

These are potential improvements for future development.

## License

This project is licensed under the **GNU General Public License v3.0 (GPL-3.0-only)**.

## Contact

- **Author:** Amal K.

- **GitHub:** [@amalk-au](https://github.com/amalk-au)

- **Repository:** [messaging-app](https://github.com/amalk-au/messaging-app)

- **Portfolio:** [amalk.au](https://amalk.au)

- **Issues:** [GitHub Issues](https://github.com/amalk-au/messaging-app/issues)

---

Developed by [amalk-au](https://github.com/amalk-au)
