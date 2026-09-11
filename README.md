## 1. What was the most challenging part of designing this API?
The most challenging part was ensuring consistency across all resources while maintaining clear relationships between members, books, loans, and categories. Designing endpoints that correctly handle nested relationships (like `/api/members/:id/loans`) required careful thought to avoid redundancy and confusion.

## 2. Which endpoint was the hardest to design and why?
The hardest endpoint to design was `POST /api/loans`. It needed to validate multiple conditions — checking if the member and book exist, verifying book availability, and updating the number of available copies. Balancing these checks while keeping the response structure clean and meaningful was tricky.

## 3. How did you decide which operations should be nested resources vs. top-level resources?
Nested resources were used when an operation clearly depended on another entity — for example, loans belonging to a specific member or books belonging to a category. Top-level resources were reserved for independent entities that could exist on their own, like `/api/books` or `/api/members`.

## 4. If you had to add a "Reviews" feature (users can review books), how would you design those endpoints?
I would create a new resource called `reviews` linked to both `members` and `books`.  
Endpoints might look like:
- `GET /api/books/:id/reviews` → Get all reviews for a book  
- `POST /api/books/:id/reviews` → Add a new review for a book  
- `GET /api/reviews/:id` → View a specific review  
- `PATCH /api/reviews/:id` → Update a review  
- `DELETE /api/reviews/:id` → Delete a review  

Each review would include fields like `id`, `book_id`, `member_id`, `rating`, `comment`, and timestamps.

## 5. What is one thing you would do differently if you started over?
If I started over, I would integrate authentication earlier in the design process. Planning JWT-based access control from the start would make it easier to secure endpoints and define roles (e.g., admin vs. member) without refactoring later.
