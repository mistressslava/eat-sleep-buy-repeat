# 🛒 eatSleepBuyRepeat

A full-stack grocery list app that helps you plan, organize, and track your shopping — build a product catalog once, then reuse those products to quickly put together new grocery lists.

## Features

- **Product catalog** — add, edit, and delete products you buy regularly, so you don't have to retype them every time
- **Grocery lists** — create multiple lists, add items from your product catalog, and mark items as done while you shop
- **List status tracking** — lists move between `OPEN` and `DONE` states
- **GitHub login** — authentication via GitHub OAuth2, no separate account/password needed
- **Responsive UI** built with React + TypeScript

## Tech Stack

**Backend**
- Java 21, Spring Boot 3.5
- Spring Web, Spring Security (OAuth2 client)
- MongoDB (via Spring Data MongoDB)
- Lombok
- JUnit + embedded MongoDB for testing
- JaCoCo for test coverage, SonarCloud for code quality

**Frontend**
- React 19 + TypeScript
- React Router
- Axios
- Vite

**DevOps**
- Docker
- GitHub Actions (CI/CD): build frontend → build backend → push image to Docker Hub → deploy to Render.com

## Project Structure

```
eatSleepBuyRepeatApp/
├── backend/                # Spring Boot REST API
│   └── src/main/java/org/example/backend/
│       ├── controller/      # REST endpoints (products, grocery lists, auth)
│       ├── model/           # Domain records (Product, GroceryList, Status, ...)
│       ├── repo/            # MongoDB repositories
│       ├── service/         # Business logic
│       ├── security/        # GitHub OAuth2 login & security config
│       └── exception/       # Global exception handling
├── frontend/                # React + TypeScript SPA
│   └── src/
│       ├── model/            # TypeScript types/interfaces
│       ├── NewGroceryList/   # Create-list flow
│       └── ProductCard/      # Product UI components
├── Dockerfile
└── .github/workflows/       # CI/CD pipelines
```

## API Overview

| Method | Endpoint                  | Description                  | Auth required |
|--------|----------------------------|-------------------------------|----------------|
| GET    | `/api/products`            | List all products             | ✅ |
| POST   | `/api/products`            | Add a new product              | ✅ |
| PUT    | `/api/products/{id}`       | Update a product               | ✅ |
| DELETE | `/api/products/{id}`       | Delete a product                | ✅ |
| GET    | `/api/grocery-list`        | List all grocery lists         | ✅ |
| GET    | `/api/grocery-list/{id}`   | Get a single grocery list       | ✅ |
| POST   | `/api/grocery-list`        | Create a new grocery list       | ✅ |
| DELETE | `/api/grocery-list/{id}`   | Delete a grocery list           | ✅ |
| GET    | `/api/auth/me`              | Get the currently logged-in user | ✅ |

## Getting Started

### Prerequisites
- Java 21
- Node.js 20+
- MongoDB instance (local or Atlas)
- A GitHub OAuth App (for login) — [create one here](https://github.com/settings/developers)

### Environment Variables

The backend expects these to be set (e.g. in a `.env` file or your run configuration):

```
MONGO_DB_URI=<your MongoDB connection string>
GITHUB_CLIENT_ID=<your GitHub OAuth client id>
GITHUB_CLIENT_SECRET=<your GitHub OAuth client secret>
APP_URL=<url the app runs at, e.g. http://localhost:5173>
```

### Run the backend

```bash
cd backend
./mvnw spring-boot:run
```

### Run the frontend

```bash
cd frontend
npm install
npm run dev
```

The frontend dev server proxies API calls to the backend; by default the app expects the backend on its usual Spring Boot port.

### Run tests

```bash
cd backend
./mvnw test
```

## Deployment

Pushing to `master` triggers a GitHub Actions pipeline that:
1. Builds the frontend and uploads the static build
2. Builds the backend, embedding the frontend build into the Spring Boot static resources
3. Packages everything into a Docker image and pushes it to Docker Hub
4. Triggers a deploy on Render.com

## Contributing

This project was built collaboratively as a learning project. Issues and pull requests are welcome.

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.
