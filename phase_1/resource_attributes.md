# Core Resource Models

## User Model
### Required Information
- `id` (UUID, made by system): A unique number for each user
- `email` (Text): User's email address
- `password` (Text, hidden): User's secret password
- `firstName` (Text): User's first name
- `lastName` (Text): User's last name
- `phoneNumber` (Text): User's phone number
- `userType` (Choice): What type of user they are (WORKER, CLIENT, ADMIN)
- `createdAt` (Date/Time, made by system): When the account was made
- `updatedAt` (Date/Time, made by system): When the account was last changed

### Optional Information
- `profilePicture` (Text): Link to their profile photo
- `dateOfBirth` (Date): When they were born
- `address` (Group): Where they live
  - `street` (Text): Street name
  - `city` (Text): City name
  - `state` (Text): State name
  - `country` (Text): Country name
  - `postalCode` (Text): Postal code
- `verificationStatus` (Choice): If their account is checked (UNVERIFIED, PENDING, VERIFIED)
- `lastLoginAt` (Date/Time): When they last logged in

### Rules
- Email must be unique and look like a real email
- Password must be at least 8 letters long with 1 big letter, 1 small letter, and 1 number
- Phone number must be in the right format
- User must be at least 18 years old

## Job Posting Model
### Required Information
- `id` (UUID, made by system): A unique number for each job
- `title` (Text): Job title
- `description` (Text): What the job is about
- `clientId` (UUID): Who posted the job
- `budget` (Group): How much the job pays
  - `amount` (Number): How much money
  - `currency` (Text): What type of money (like KES, NGN, ZAR)
- `status` (Choice): Job status (OPEN, IN_PROGRESS, COMPLETED, CANCELLED)
- `createdAt` (Date/Time, made by system): When the job was posted
- `updatedAt` (Date/Time, made by system): When the job was last changed

### Optional Information
- `category` (Text): What type of job it is
- `skills` (List): What skills are needed
- `location` (Group): Where the job is
  - `type` (Choice): Where you work (REMOTE, ONSITE, HYBRID)
  - `address` (Text): Where the job is located
- `deadline` (Date/Time): When to apply by
- `estimatedDuration` (Text): How long the job will take

### Rules
- Title must be between 5 and 100 letters
- Description must be between 50 and 5000 letters
- Budget must be more than 0
- Currency must be a real money code

## Worker Profile Model
### Required Information
- `id` (UUID, made by system): A unique number
- `userId` (UUID): Which user this profile belongs to
- `title` (Text): Their job title
- `bio` (Text): About them
- `hourlyRate` (Group): How much they charge per hour
  - `amount` (Number): How much money
  - `currency` (Text): What type of money
- `availability` (Choice): If they can work (AVAILABLE, BUSY, UNAVAILABLE)
- `createdAt` (Date/Time, made by system): When the profile was made
- `updatedAt` (Date/Time, made by system): When the profile was last changed

### Optional Information
- `skills` (List): What they can do
- `experience` (List): Where they worked before
  - `title` (Text): Job title
  - `company` (Text): Company name
  - `startDate` (Date): When they started
  - `endDate` (Date): When they finished
  - `description` (Text): What they did
- `education` (List): Where they went to school
  - `institution` (Text): School name
  - `degree` (Text): What they studied
  - `field` (Text): What subject
  - `graduationYear` (Number): When they finished
- `certifications` (List): Their certificates
- `portfolio` (List): Their work examples
- `languages` (List): What languages they speak

### Rules
- Title must be between 3 and 50 letters
- Bio must be between 100 and 2000 letters
- Hourly rate must be more than 0
- Experience dates must make sense

## Application Model
### Required Information
- `id` (UUID, made by system): A unique number
- `jobId` (UUID): Which job they're applying for
- `workerId` (UUID): Who is applying
- `proposal` (Text): Why they want the job
- `status` (Choice): Application status (PENDING, ACCEPTED, REJECTED)
- `createdAt` (Date/Time, made by system): When they applied
- `updatedAt` (Date/Time, made by system): When the application was last changed

### Optional Information
- `bidAmount` (Group): How much they want to be paid
  - `amount` (Number): How much money
  - `currency` (Text): What type of money
- `estimatedDuration` (Text): How long they think it will take
- `attachments` (List): Files they attached
- `message` (Text): Extra message to the client

### Rules
- Proposal must be between 100 and 2000 letters
- Bid amount must be more than 0
- A worker can only apply once to each job

## Review Model
### Required Information
- `id` (UUID, made by system): A unique number
- `jobId` (UUID): Which job this is for
- `reviewerId` (UUID): Who wrote the review
- `revieweeId` (UUID): Who is being reviewed
- `rating` (Number): Score from 1 to 5
- `comment` (Text): What they think
- `createdAt` (Date/Time, made by system): When the review was written
- `updatedAt` (Date/Time, made by system): When the review was last changed

### Optional Information
- `response` (Text): Reply to the review
- `tags` (List): Categories for the review

### Rules
- Rating must be between 1 and 5
- Comment must be between 10 and 1000 letters
- A user can only review another user once per job

## Transaction Model
### Required Information
- `id` (UUID, made by system): A unique number
- `jobId` (UUID): Which job this is for
- `amount` (Group): How much money
  - `value` (Number): The amount
  - `currency` (Text): What type of money
- `paymentMethod` (Choice): How they paid (MOBILE_MONEY, BANK_TRANSFER, CASH)
- `status` (Choice): Payment status (PENDING, COMPLETED, FAILED, REFUNDED)
- `createdAt` (Date/Time, made by system): When the payment was made
- `updatedAt` (Date/Time, made by system): When the payment was last changed

### Optional Information
- `mobileMoneyDetails` (Group): Mobile money info
  - `provider` (Text): Which mobile money service (M-PESA, MTN, Airtel)
  - `phoneNumber` (Text): Phone number for payment
  - `transactionId` (Text): Payment reference number
- `bankDetails` (Group): Bank info
  - `accountNumber` (Text): Bank account number
  - `bankName` (Text): Name of bank
  - `accountName` (Text): Name on account
- `receipt` (Text): Link to payment receipt
- `notes` (Text): Extra notes about payment

### Rules
- Amount must be more than 0
- Currency must be a real money code
- Mobile money phone number must be in the right format
- One job can have many payments
- Payment status changes must make sense

### Offline Support
- Can save payments when offline
- Fixes duplicate payments
- Makes receipts when online
- Updates status when back online 