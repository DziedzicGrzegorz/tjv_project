# Secure File Sharing System

A modern, secure file sharing platform with user and group-based access control, built with Spring Boot backend and Next.js frontend.

## Project Overview

This application provides a comprehensive solution for secure file storage and sharing with granular permission controls. Users can upload files, organize them, share with other users or groups, and maintain version control.

## Key Features

- **User Authentication & Authorization**: Secure login and registration
- **File Management**: Upload, download, and manage files
- **Version Control**: Track changes with file versioning
- **Sharing Capabilities**: Share files with individual users or groups
- **Permission Control**: Granular access permissions for shared content
- **Group Management**: Create and manage user groups
- **Responsive UI**: Modern interface that works on all devices
- **Large File Support**: Handle files up to 50MB

## Tech Stack

### Backend (Spring Boot)

- **Framework**: Spring Boot
- **Database**: PostgreSQL with JPA/Hibernate
- **Security**: JWT Authentication
- **Storage**: Azure Blob Storage for file data
- **Documentation**: OpenAPI/Swagger
- **Build Tool**: Maven/Gradle

### Frontend (Next.js)

- **Framework**: Next.js 15 with React 19
- **Styling**: TailwindCSS
- **State Management**: React Hooks
- **Form Handling**: React Hook Form with Zod validation
- **UI Components**: shadcn/ui and Aceternity UI
- **Authentication**: JWT with HTTP-only cookies

## Architecture

The application follows a microservices-inspired architecture:

1. **Frontend Service**: Next.js application that provides the user interface
2. **Backend API Service**: Spring Boot REST API that handles business logic
3. **Database**: PostgreSQL for structured data storage
4. **Blob Storage**: Azure Storage for file content

## Getting Started

### Prerequisites

- Node.js 18+ (Frontend)
- Java 17+ (Backend)
- PostgreSQL database
- Azure Storage account
- Docker (optional for containerized deployment)

### Installation

#### Backend Setup

1. Clone the backend repository:
```
git clone https://gitlab.fit.cvut.cz/dziedgrz/tjv_backend.git
cd tjv_backend
```

2. Configure the application by filling in the necessary values in `application.properties`:

```properties
# Database Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/your_database
spring.datasource.username=your_username
spring.datasource.password=your_password

# Azure Storage Configuration
azure.storage.connection.string=your_azure_connection_string
azure.storage.container.name=your_container_name
azure.storage.queue.name=your_queue_name

# JWT Security Configuration
application.security.jwt.secret-key=your_jwt_secret_key
application.security.jwt.access_expiration=3600000
application.security.jwt.refresh_expiration=86400000
```

3. Build and run the backend:
```
./mvnw spring-boot:run
```

The backend API will be available at: `http://localhost:8080/api/v1`

#### Frontend Setup

1. Clone the frontend repository:
```
git clone https://gitlab.fit.cvut.cz/your-username/tjv_frontend.git
cd tjv_frontend
```

2. Install dependencies:
```
npm install
```

3. Create a `.env.local` file with required configuration:
```
NEXT_PUBLIC_API_URL=http://localhost:8080/api/v1
```

4. Run the development server:
```
npm run dev
```

The frontend will be available at: `http://localhost:3000`

## API Documentation

API documentation is available via Swagger UI:
- `http://localhost:8080/api/v1/swagger-ui.html`

## Deployment

### Docker Deployment

Both the frontend and backend can be containerized and deployed together:

```
docker-compose up -d
```

### Cloud Deployment

The application is designed to be deployed to cloud platforms like Azure:

- Frontend: Azure Static Web Apps
- Backend: Azure App Service
- Database: Azure Database for PostgreSQL
- Storage: Azure Blob Storage

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a merge request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Faculty of Information Technology, Czech Technical University in Prague