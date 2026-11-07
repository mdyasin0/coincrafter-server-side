# coincrafter– Server

A RESTful backend application for a Micro-Task and Earning Platform, built with Node.js, Express.js, and MongoDB. The server manages user accounts, role-based authorization, task operations, work submissions, coin transactions, payment processing, withdrawal requests, and administrative operations.

**Project Name:** Micro-Task and Earning Platform  
**Project Category:** Backend Development  
**Architecture:** MERN Stack – Server-side  
**Primary Technologies:** Node.js, Express.js, MongoDB

---

## 1. Project Overview

The Micro-Task and Earning Platform connects Buyers who create small tasks with Workers who complete those tasks in exchange for virtual coins.

The backend provides the core business logic and data management capabilities required to operate the platform. It handles user information, task creation, submission processing, coin balance management, payment records, withdrawal requests, and administrative operations.

The application supports three user roles:

- **Worker:** Completes available tasks, submits work, earns coins, and requests withdrawals.
- **Buyer:** Creates tasks, manages task budgets, reviews submissions, purchases coins, and manages payments.
- **Admin:** Manages users, updates account roles, oversees platform tasks, and processes withdrawal requests.

---

## 2. Backend Features and Responsibilities

### A. User and Account Management

- Manage registered user information and account records.
- Store user names, email addresses, profile information, roles, and coin balances.
- Support Worker and Buyer registration workflows.
- Assign initial coins based on the selected account role:
  - Worker: 10 coins.
  - Buyer: 50 coins.
- Maintain user role information for authorization.
- Support administrative user management operations.
- Implement user removal and role update functionality.


### C. Task Management

- Create and store task information in MongoDB.
- Manage task titles, descriptions, required worker counts, payable amounts, completion deadlines, submission instructions, and image URLs.
- Calculate the total task budget using the required worker count and per-worker reward.

**Task Budget Calculation:**

`Total Task Budget = Required Workers × Payable Amount`

- Validate the Buyer's available coin balance before task creation.
- Deduct the required coins when a task is successfully created.
- Retrieve available tasks based on remaining worker requirements.
- Retrieve tasks associated with individual Buyers.
- Support task updates and deletion.
- Restore the remaining task budget when an unfinished task is deleted.
- Support administrative task management operations.

### D. Worker Submission Management

- Store Worker task submissions in MongoDB.
- Associate submissions with the corresponding task, Worker, and Buyer.
- Maintain submission information, including task details, Worker information, Buyer information, submission date, and status.
- Initialize new submissions with a `pending` status.
- Retrieve submissions associated with individual Workers.
- Retrieve pending submissions for Buyer review.
- Support submission details retrieval and status management.

### E. Submission Review and Reward Processing

- Allow authorized Buyers to review submissions for their tasks.
- Support submission approval and rejection.
- Update submission status after review.

**When a submission is approved:**

- Change the submission status to `approved`.
- Credit the corresponding payable coins to the Worker.

**When a submission is rejected:**

- Change the submission status to `rejected`.
- Restore one required worker slot to the corresponding task.

### F. Coin Management and Business Logic

The application uses a virtual coin system to manage task budgets, Worker rewards, purchases, and withdrawals.

**Initial Coin Allocation**

| Account Role | Initial Coins |
|---|---:|
| Worker | 10 |
| Buyer | 50 |

**Buyer Coin Purchasing Rates**

| Coin Package | Price |
|---:|---:|
| 10 Coins | $1 |
| 150 Coins | $10 |
| 500 Coins | $20 |
| 1000 Coins | $35 |

**Worker Withdrawal Conversion**

| Coins | Withdrawal Amount |
|---:|---:|
| 20 Coins | $1 |
| 200 Coins | $10 |
| 300 Coins | $15 |

- Maintain user coin balances.
- Deduct Buyer coins when tasks are created.
- Credit Worker coins after successful submission approval.
- Credit Buyer coins after successful coin purchases.
- Restore the remaining task budget when applicable.
- Validate coin-related operations against account balances.

### G. Stripe Payment Processing

- Integrate Stripe for Buyer coin purchases.
- Support the server-side payment workflow.
- Handle payment-related requests.
- Record successful payment information.
- Update Buyer coin balances following successful payments.
- Maintain payment transaction records for payment history.

### H. Worker Withdrawal Management

- Manage Worker withdrawal requests.
- Apply the conversion rate of 20 coins per US dollar.
- Enforce the minimum withdrawal requirement of 200 coins ($10).
- Validate withdrawal requests against the Worker's available coin balance.
- Store withdrawal information, including:
  - Worker name and email.
  - Requested withdrawal coins.
  - Withdrawal amount.
  - Selected payment method.
  - Account number.
  - Withdrawal date.
  - Request status.

New withdrawal requests are stored with a `pending` status.

### I. Administrative Operations

- Retrieve platform statistics.
- Retrieve Worker and Buyer account information.
- Calculate total available user coins.
- Retrieve payment-related information.
- Manage registered users.
- Update user roles.
- Remove users from the database.
- Retrieve and manage platform tasks.
- Delete tasks through authorized administrative operations.
- Retrieve pending withdrawal requests.
- Process withdrawal requests and update their statuses.
- Deduct the corresponding coins from the Worker's account when a withdrawal is approved.

### J. Database Operations

MongoDB is used to store and manage the platform's application data.

The backend handles the following data groups:

- User account information.
- Task information.
- Worker submission records.
- Payment records.
- Withdrawal requests.

The database operations support the platform's task management, account management, coin transactions, and administrative workflows.

### K. API Development

- Develop RESTful API endpoints using Express.js.
- Handle HTTP requests and responses.
- Implement server-side business logic.
- Support database operations through API requests.
- Support pagination for applicable data retrieval operations.
- Use environment variables for application configuration and sensitive credentials.

---

## 3. Business Logic

The platform follows a coin-based business model.

### Task Creation

When a Buyer creates a task, the backend calculates the total required budget.

`Total Budget = Required Workers × Payable Amount`

The task is created only when the Buyer's available coin balance is sufficient. The required coins are deducted from the Buyer's account.

### Submission Approval

When a Buyer approves a Worker submission, the corresponding submission status is updated and the Worker receives the specified reward coins.

### Submission Rejection

When a Buyer rejects a submission, its status changes to `rejected`, and the task's required worker count increases by one.

### Task Deletion and Refund

When a Buyer deletes an unfinished task, the remaining task budget is calculated and returned to the Buyer's account.

`Refund Amount = Remaining Workers × Payable Amount`

### Withdrawal

Workers can request withdrawals when they have at least 200 available coins.

`Withdrawal Amount = Withdrawal Coins ÷ 20`

The withdrawal request is initially stored as `pending`. After administrative approval, the corresponding coins are deducted from the Worker's balance.

---

## 4. Technology Stack

### Core Backend Technologies

- [Node.js](https://nodejs.org/) – JavaScript runtime environment.
- [Express.js](https://expressjs.com/) – Backend framework for developing RESTful APIs.
- [MongoDB](https://www.mongodb.com/) – NoSQL database for application data management.

### Payment Integration

- [Stripe](https://stripe.com/) – Payment processing integration.

### Supporting Technologies and Packages

- [CORS](https://www.npmjs.com/package/cors) – Cross-Origin Resource Sharing configuration.
- [dotenv](https://www.npmjs.com/package/dotenv) – Environment-based configuration management.

**Architecture:** MongoDB, Express.js, React.js, and Node.js (MERN Stack).

---

## 5. Environment Configuration and Security

The application uses environment variables to manage sensitive configuration values.

Before running the server, create a `.env` file in the server project root and provide the configuration values required by the application.

# mongodb

DB_USER=
DB_PASS=
# stripe

STRIPE_SECRET_KEY=

**Security Guidelines:**

- Never commit `.env` files to a public GitHub repository.
- Add `.env` to `.gitignore`.
- Never expose Stripe secret keys or MongoDB credentials.
- Keep sensitive configuration values on the server.



---

## 6. Getting Started

Follow these instructions to set up the backend project locally.

### Prerequisites

- Node.js installed.
- npm installed.
- MongoDB database access.
- Required Stripe configuration for payment-related functionality.

### Step 1: Clone the Repository

```bash
git clone https://github.com/mdyasin0/coincrafter-server-side.git
```

### Step 2: Navigate to the Project Directory

```bash
cd YOUR_SERVER_PROJECT_DIRECTORY
```

### Step 3: Install Dependencies

```bash
npm install
```

### Step 4: Configure Environment Variables

Create a `.env` file in the server project root.

Add the required configuration values using the exact variable names expected by the server application.

### Step 5: Check Available Scripts

```bash
npm run
```

Review the scripts defined in `package.json` and use the appropriate development or production command.

### Step 6: Start the Server

Run command "nodemon index.js"

---

## 7. API Documentation

The backend exposes RESTful API resources for the platform's core operations.

The API functionality is organized around these resource areas:

| Resource | Responsibilities |
|---|---|
| Users | Account information, user management, and role updates. |
| Tasks | Task creation, retrieval, updating, and deletion. |
| Submissions | Submission creation, retrieval, approval, and rejection. |
| Payments | Coin purchases and payment record management. |
| Withdrawals | Withdrawal requests and administrative processing. |
| Admin | Platform statistics and administrative operations. |

Specific API routes, HTTP methods, request parameters, and response structures should be documented according to the implemented server routes.

---

## 8. Project Highlights

- Backend development using Node.js and Express.js.
- RESTful API architecture.
- MongoDB-based application data management.
- Task creation and management workflows.
- Submission review and reward allocation.
- Coin-based business logic.
- Stripe payment processing.
- Worker withdrawal management.
- Administrative operations.
- Environment-based configuration.


---

## 9. Author

**MD Yasin**

- **Email:** mdyasin48902@gmail.com
- **GitHub:** [github.com/mdyasin0](https://github.com/mdyasin0)

---

*Backend component of the Micro-Task and Earning Platform – Junior MERN Stack Developer assessment project.*
