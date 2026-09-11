1. Overview
The Library Management System API is a RESTful API designed
to manage library members, books, loans, and categories.
It allows the library to manage its book catalog, register
members, process book borrowing and returns, and organize
books by category.

2. Base URL
Production: https://api.library.com/v1
Development: http://localhost:3000/api

3. Resources
1. Members
2. Books
3. Loans
4. Categories

Members
- id
- first_name
- last_name
- email
- phone
- address
- membership_date
- status
- created_at
- updated_at

Books
- id
- title
- author
- isbn
- published_year
- category_id
- total_copies
- available_copies
- created_at
- updated_at

Loans
- id
- member_id
- book_id
- borrowed_at
- due_date
- returned_at
- status
- created_at
- updated_at

Categories
- id
- name
- description
- created_at
- updated_at

4. Endpoints
GET    /api/members
GET    /api/members/:id
POST   /api/members
PUT    /api/members/:id
PATCH  /api/members/:id
DELETE /api/members/:id

GET    /api/books
GET    /api/books/:id
POST   /api/books
PUT    /api/books/:id
PATCH  /api/books/:id
DELETE /api/books/:id

GET    /api/loans
GET    /api/loans/:id
POST   /api/loans
PUT    /api/loans/:id
PATCH  /api/loans/:id
DELETE /api/loans/:id

GET    /api/categories
GET    /api/categories/:id
POST   /api/categories
PUT    /api/categories/:id
PATCH  /api/categories/:id
DELETE /api/categories/:id
Nested Endpoints
GET  /api/members/:id/loans
POST /api/members/:id/loans

GET  /api/categories/:id/books
POST /api/categories/:id/books

GET  /api/books/:id/loans
POST /api/books/:id/loans



PART 4: Define Request & Response Formats

*Instructor Cue: "Now we specify what data goes IN (request) and what data comes OUT (response)."*

Step 5: Request Body

Endpoint: POST /api/members
Request Body:

{
    "first_name": "Juan",
    "last_name": "Dela Cruz",
    "email": "juan.delacruz@gmail.com",
    "phone": "09171234567",
    "address": "Cebu City"
}


Endpoint: POST /api/loans
Request Body:
{
    "member_id": 12,
    "book_id": 5,
    "due_date": "2026-09-18"
}



Endpoint: PATCH /api/books/:id
Request Body:
{
    "available_copies": 4
}


Step 6: Response Body Examples

For each endpoint, define what the response should look like (both success and error cases).

Endpoint: GET /api/members/:id
Success Response (200 OK):
{
    "id": 12,
    "first_name": "Juan",
    "last_name": "Dela Cruz",
    "email": "juan.delacruz@gmail.com",
    "phone": "09171234567",
    "address": "Cebu City",
    "membership_date": "2026-09-04",
    "status": "active",
    "created_at": "2026-09-04T08:30:00Z",
    "updated_at": "2026-09-04T08:30:00Z"
}

Error Response (404 Not Found)
{
    "error": {
        "code": "MEMBER_NOT_FOUND",
        "message": "Member with ID 12 does not exist"
    }
}

Endpoint: /api/loans                              Success Response (201 Created)
{
    "id": 101,
    "member_id": 12,
    "book_id": 5,
    "borrowed_at": "2026-09-04T09:00:00Z",
    "due_date": "2026-09-18",
    "returned_at": null,
    "status": "active",
    "created_at": "2026-09-04T09:00:00Z",
    "updated_at": "2026-09-04T09:00:00Z"
}
Error Response (409 Conflict)
{
    "error": {
        "code": "BOOK_NOT_AVAILABLE",
        "message": "This book has no available copies for borrowing"
    }
}


Endpoint: /api/books/:id                           Success Response (204 No Content)
No response body.
The book was successfully deleted.
Error Response (404 Not Found)
{
    "error": {
        "code": "BOOK_NOT_FOUND",
        "message": "Book with ID 5 does not exist"
    }
}
PART 5: Choose Status Codes

*Instructor Cue: "This is where most beginners fail. The status code is the FIRST line of error handling."*

Step 7: Status Code Mapping
For each endpoint, specify the exact HTTP status codes for different scenarios.
Endpoint: GET /api/books/:id                                                                                           - Success: 200 (Book retrieved successfully)                                                                  - Created: N/A                                                                                                                  - Bad Request: 400 (Invalid book ID)                                                                                - Not Found: 404 (Book does not exist)                                                                              - Conflict: N/A                                                                                                                   - Server Error: 500 (Internal server error) 

Endpoint: POST /api/members                                                                             - Success: N/A (use 201 Created instead)                                                               - Created: 201 (Member created successfully)                                                                 - Bad Request: 400 (Required member information is missing or invalid)                     - Not Found: N/A                                                                                                                  - Conflict: 409 (Email address is already registered)                                                          - Server Error: 500 (Database or server error) 
Endpoint: POST /api/loans                                                                                           - Success: N/A (use 201 Created instead)                                                                          - Created: 201 (Loan created successfully)                                                                           - Bad Request: 400 (Required loan information is missing or invalid)                                             - Not Found: 404 (Member or book does not exist)                                                           - Conflict: 409 (Book has no available copies)                                                                     - Server Error: 500 (Database or server error)

Endpoint: DELETE /api/books/:id                                                                                    - Success: 204 (Book deleted successfully)                                                                     - Created: N/A                                                                                                                    - Bad Request: 400 (Invalid book ID)                                                                               - Not Found: 404 (Book does not exist)                                                                             - Conflict: 409 (Book has active loans)                                                                                 - Server Error: 500 (Database or server error)


Step 8: Special Scenarios
Think about edge cases and define how your API handles them.

Scenarios to Consider:
1. Borrowing an unavailable book: What status code? What error message?
2. Returning an already-returned loan: What status code?
3. Deleting a book that has active loans: What status code?
4. Creating a member with a duplicate email: What status code?

Scenario: Borrowing a book with 0 available copies                                      Status Code: 409                                                                                                    Error Code: "BOOK_NOT_AVAILABLE"                                                                   Error Message: "This book has no available copies for borrowing."

Scenario: Returning a loan that has already been returned                                   Status Code: 409                                                                                                         Error Code: "LOAN_ALREADY_RETURNED"                                                           Error Message: "This loan has already been returned."

Scenario: Deleting a book that has active loans                                                     Status Code: 409                                                                                                         Error Code: "BOOK_HAS_ACTIVE_LOANS"                                                          Error Message: "This book cannot be deleted because it has active loans."
Scenario: Creating a member with an email that already exists                              Status Code: 409                                                                                                        Error Code: "EMAIL_ALREADY_EXISTS"                                                             Error Message: "A member with this email address already exists."


PART 6: Advanced Features (Optional Challenge)

*Instructor Cue: "If you finish early, let's add some professional features."*


Challenge 1: Query Parameters for Filtering & Sorting

Design query parameters for searching and filtering.


Endpoint: GET /api/books
Query Parameters:

- ?search=programming       → Search books by title or author

- ?category_id=3                  → Filter books by category

- ?sort=published_year        → Sort books by publication year

- ?order=desc                      → Sort from newest to oldest

- ?page=1&limit=10             → Display page 1 with 10 books

Full URL: GET /api/books?search=programming&category_id=3&sort=published_year&order=desc&page=1&limit=10

Endpoint: GET /api/loans                                                                                      Query Parameters:
- ?status=active  → Show active loans                                                                          - ?status=returned  → Show returned loans                                                                     - ?status=overdue→ Show overdue loans                                                                           - ?member_id=12  → Show loans for member 12                                                    - ?page=1&limit=10  → Display page 1 with 10 loans

Full URL: 
GET /api/loans?status=active&member_id=12&page=1&limit=10

PART 5: Choose Status Codes
Step 7: Status Code Mapping
1. GET /api/books/:id
Endpoint: GET /api/books/:id

- Success: 200 (Book retrieved successfully)
- Created: N/A
- Bad Request: 400 (Invalid book ID)
- Not Found: 404 (Book does not exist)
- Conflict: N/A
- Server Error: 500 (Internal server error)

2. POST /api/members
Endpoint: POST /api/members

- Success: N/A (use 201 Created instead)
- Created: 201 (Member created successfully)
- Bad Request: 400 (Required member information is missing or invalid)
- Not Found: N/A
- Conflict: 409 (Email address is already registered)
- Server Error: 500 (Database or server error)

3. POST /api/loans
Endpoint: POST /api/loans

- Success: N/A (use 201 Created instead)
- Created: 201 (Loan created successfully)
- Bad Request: 400 (Required loan information is missing or invalid)
- Not Found: 404 (Member or book does not exist)
- Conflict: 409 (Book has no available copies)
- Server Error: 500 (Database or server error)

4. DELETE /api/books/:id
Endpoint: DELETE /api/books/:id

- Success: 204 (Book deleted successfully)
- Created: N/A
- Bad Request: 400 (Invalid book ID)
- Not Found: 404 (Book does not exist)
- Conflict: 409 (Book has active loans)
- Server Error: 500 (Database or server error)

Step 8: Special Scenarios
Scenario 1: Borrowing an unavailable book
Scenario: Borrowing a book with 0 available copies
Status Code: 409
Error Code: "BOOK_NOT_AVAILABLE"
Error Message: "This book has no available copies for borrowing."
Scenario 2: Returning an already-returned loan
Scenario: Returning a loan that has already been returned
Status Code: 409
Error Code: "LOAN_ALREADY_RETURNED"
Error Message: "This loan has already been returned."
Scenario 3: Deleting a book that has active loans
Scenario: Deleting a book that has active loans
Status Code: 409
Error Code: "BOOK_HAS_ACTIVE_LOANS"
Error Message: "This book cannot be deleted because it has active loans."
Scenario 4: Creating a member with a duplicate email
Scenario: Creating a member with an email that already exists
Status Code: 409
Error Code: "EMAIL_ALREADY_EXISTS"
Error Message: "A member with this email address already exists."
Scenario 5: Borrowing a non-existent book
Scenario: Borrowing a book that does not exist
Status Code: 404
Error Code: "BOOK_NOT_FOUND"
Error Message: "The specified book does not exist."
Scenario 6: Creating a loan for a non-existent member
Scenario: Creating a loan for a member that does not exist
Status Code: 404
Error Code: "MEMBER_NOT_FOUND"
Error Message: "The specified member does not exist."
PART 6: Advanced Features
Challenge 1: Query Parameters
1. GET /api/books
Endpoint: GET /api/books

Query Parameters:

- ?search=programming  → Search books by title or author

- ?category_id=3             → Filter books by category

- ?sort=published_year   → Sort books by publication year

- ?order=desc                 → Sort from newest to oldest

- ?page=1&limit=10        → Display page 1 with 10 books
Full URL Example
GET /api/books?search=programming&category_id=3&sort=published_year&order=desc&page=1&limit=10

2. GET /api/loans
Endpoint: GET /api/loans

Query Parameters:

- ?status=active       → Show active loans

- ?status=returned   → Show returned loans

- ?status=overdue   → Show overdue loans

- ?member_id=12   → Show loans for member 12

- ?page=1&limit=10  → Display page 1 with 10 loans

Full URL Example
GET /api/loans?status=active&member_id=12&page=1&limit=10

Challenge 2: API Versioning
Current Version: v1
New Version: v2

Breaking Change:
The "available_copies" field will be replaced with
"total_copies" and "borrowed_copies".

Migration Strategy:
Support both /api/v1 and /api/v2 for six months.
Developers can gradually update their applications
from v1 to v2.

Deprecation Timeline:
Announce the deprecation of v1 when v2 is released.
Keep v1 available for six months before removing it.
Example:
/api/v1/books
/api/v2/books



PART 7: Final Documentation
1. Overview
The Library Management System API is a RESTful API designed
to manage library members, books, loans, and categories.
It allows the library to manage its book catalog, register
members, process book borrowing and returns, and organize
books by category.
2. Base URL
Production: https://api.library.com/v1
Development: http://localhost:3000/api
3. Resources
1. Members
2. Books
3. Loans
4. Categories
Members
- id
- first_name
- last_name
- email
- phone
- address
- membership_date
- status
- created_at
- updated_at
Books
- id
- title
- author
- isbn
- published_year
- category_id
- total_copies
- available_copies
- created_at
- updated_at
Loans
- id
- member_id
- book_id
- borrowed_at
- due_date
- returned_at
- status
- created_at
- updated_at
Categories
- id
- name
- description
- created_at
- updated_at
4. Endpoints
GET    /api/members
GET    /api/members/:id
POST   /api/members
PUT    /api/members/:id
PATCH  /api/members/:id
DELETE /api/members/:id

GET    /api/books
GET    /api/books/:id
POST   /api/books
PUT    /api/books/:id
PATCH  /api/books/:id
DELETE /api/books/:id

GET    /api/loans
GET    /api/loans/:id
POST   /api/loans
PUT    /api/loans/:id
PATCH  /api/loans/:id
DELETE /api/loans/:id

GET    /api/categories
GET    /api/categories/:id
POST   /api/categories
PUT    /api/categories/:id
PATCH  /api/categories/:id
DELETE /api/categories/:id
Nested Endpoints
GET  /api/members/:id/loans
POST /api/members/:id/loans

GET  /api/categories/:id/books
POST /api/categories/:id/books

GET  /api/books/:id/loans
POST /api/books/:id/loans
5. Authentication
The API will use JWT (JSON Web Token) authentication.
Users will log in to receive a JWT token. The token will
be included in the Authorization header when accessing
protected endpoints.

Example:

Authorization: Bearer <JWT_TOKEN>

Authentication will be implemented in the final period.
6. Error Handling
{
    "error": {
        "code": "BOOK_NOT_FOUND",
        "message": "Book with ID 5 does not exist"
    }
}
Common status codes:
200 OK                  → Request successful
201 Created             → Resource created successfully
204 No Content          → Resource deleted successfully
400 Bad Request         → Invalid request
401 Unauthorized        → Authentication required
403 Forbidden           → Access denied
404 Not Found           → Resource does not exist
409 Conflict            → Request conflicts with existing data
500 Internal Server Error → Server error
7. Special Scenarios
- Book has no available copies → 409 Conflict
- Loan has already been returned → 409 Conflict
- Book has active loans → 409 Conflict
- Member email already exists → 409 Conflict
- Member does not exist → 404 Not Found
- Book does not exist → 404 Not Found
8. Future Enhancements
- User authentication and authorization
- Book reservation system
- Automatic overdue notifications
- Fine and penalty management
- Book reviews and ratings
- Admin dashboard
- Advanced search
- Reports and analytics
- Book cover image uploads

// note my ER Diagram and API Flow borrowing book is on my document pdf file.


