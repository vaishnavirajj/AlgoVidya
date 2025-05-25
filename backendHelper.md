# AlgoVidya Backend Developer Notes

## 1. Overview
AlgoVidya is a platform for learning Data Structures and Algorithms (DSA). The backend needs to support features like user progress tracking, question management, and personalized recommendations.

## 2. Technology Stack
- **Node.js** with **Express.js** for the server
- **MongoDB** for the database (NoSQL allows for flexible schema as the project evolves)
- **JWT** for authentication
- **Redis** for caching (optional, for performance optimization)

## 3. Data Models
### a. **User**
- `_id`: ObjectId
- `username`: String
- `email`: String
- `passwordHash`: String
- `progress`: Array of **QuestionProgress**

### b. **Question**
- `_id`: ObjectId
- `title`: String
- `description`: String
- `difficulty`: Enum ['Easy', 'Medium', 'Hard']
- `category`: String
- `companies`: Array of Strings
- `solution`: Object
  - `text`: String
  - `code`: Object
    - `cpp`: String
    - `java`: String
    - `javascript`: String
- `videos`: Array of Objects
  - `title`: String
  - `language`: String
  - `url`: String

### c. **QuestionProgress**
- `questionId`: ObjectId (ref: **Question**)
- `status`: Enum ['Solved', 'Attempted', 'Not Attempted']
- `lastAttemptDate`: Date

### d. **CustomList**
- `_id`: ObjectId
- `userId`: ObjectId (ref: **User**)
- `name`: String
- `questions`: Array of ObjectId (ref: **Question**)

## 4. API Endpoints
### a. **Authentication**
- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/logout`

### b. **User Progress**
- `GET /api/progress`
- `POST /api/progress/:questionId`
- `PUT /api/progress/:questionId`

### c. **Questions**
- `GET /api/questions`
- `GET /api/questions/:id`
- `POST /api/questions` (admin only)
- `PUT /api/questions/:id` (admin only)
- `DELETE /api/questions/:id` (admin only)

### d. **Custom Lists**
- `GET /api/lists`
- `POST /api/lists`
- `PUT /api/lists/:id`
- `DELETE /api/lists/:id`
- `POST /api/lists/:id/questions`
- `DELETE /api/lists/:id/questions/:questionId`

### e. **Recommendations**
- `GET /api/recommendations`

## 5. Authentication Flow
1. User registers or logs in
2. Server validates credentials and issues a JWT
3. Client stores the JWT (e.g., in localStorage)
4. Client includes the JWT in the **Authorization** header for subsequent requests
5. Server validates the JWT for protected routes

## 6. Data Flow
1. Frontend requests data (e.g., questions, user progress)
2. Backend receives request, authenticates if necessary
3. Backend queries the database for requested data
4. Backend processes data if needed (e.g., filtering, sorting)
5. Backend sends response to frontend
6. Frontend updates its state and re-renders components

## 7. Saving User Progress
1. User completes a question or updates their progress
2. Frontend sends a POST or PUT request to `/api/progress/:questionId`
3. Backend updates the **User** document in the database
4. Backend sends a success response
5. Frontend updates its local state

## 8. Recommendation System
1. Backend analyzes user's solved questions and progress
2. Considers factors like question difficulty, category, and user's recent activity
3. Queries the database for suitable questions
4. Applies ranking algorithm to select the best recommendation
5. Returns recommended question(s) to the frontend

## 9. Performance Considerations
- Implement pagination for question lists
- Use indexing in MongoDB for frequently queried fields
- Consider caching frequently accessed data with Redis
- Implement rate limiting to prevent abuse

## 10. Security Measures
- Use **HTTPS** for all communications
- Implement proper input validation and sanitization
- Use **bcrypt** for password hashing
- Implement **CORS** policies
- Use environment variables for sensitive information (e.g., database credentials, JWT secret)

## 11. Scalability
- Consider using a **load balancer** for distributing traffic
- Implement **database sharding** if the dataset grows significantly
- Use a **CDN** for serving static assets (e.g., question images, video thumbnails)

## 12. Monitoring and Logging
- Implement error logging (e.g., using **Winston**)
- Set up performance monitoring (e.g., with **New Relic** or **Prometheus**)
- Log all API requests for auditing and debugging purposes

## 13. Testing
- Write unit tests for individual functions and API endpoints
- Implement integration tests for testing the entire flow
- Use tools like **Jest** for testing Node.js applications

## 14. Deployment
- Use **Docker** for containerization
- Consider using **Kubernetes** for orchestration if scaling to multiple services
- Implement **CI/CD** pipelines (e.g., using **GitHub Actions** or **GitLab CI**)

## 15. Future Considerations
- Implement **real-time features** (e.g., live coding sessions) using **WebSockets**
- Add support for **OAuth 2.0** for social login options
- Implement an **admin panel** for content management
- Consider adding a **discussion forum** or **Q&A section**

 