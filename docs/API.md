\# API Contract



\## Base URL



Development:



http://localhost:3000/api



\## 1. Health



\### GET /health



Purpose:

Check API availability and infrastructure connectivity.



Expected response:



{

&#x20; "success": true,

&#x20; "data": {

&#x20;   "api": "ok",

&#x20;   "database": "ok",

&#x20;   "redis": "ok"

&#x20; }

}



\## 2. Sender APIs



\### POST /api/senders



Purpose:

Create a sender account.



Request:



{

&#x20; "name": "Demo Sender",

&#x20; "email": "sender@example.com",

&#x20; "smtpHost": "smtp.example.com",

&#x20; "smtpPort": 587,

&#x20; "smtpUsername": "sender@example.com",

&#x20; "smtpPassword": "password",

&#x20; "hourlyLimit": 100,

&#x20; "minDelaySeconds": 2

}



The SMTP password must be encrypted before storage.



\### GET /api/senders



Purpose:

List configured senders.



\### GET /api/senders/:id



Purpose:

Retrieve a sender's configuration.



Sensitive credentials must not be exposed.



\## 3. Email APIs



\### POST /api/emails



Purpose:

Schedule an email.



Request:



{

&#x20; "senderId": "sender-uuid",

&#x20; "recipient": "recipient@example.com",

&#x20; "subject": "Hello",

&#x20; "body": "Test email body",

&#x20; "scheduledAt": "2026-12-01T10:00:00.000Z"

}



Expected behavior:



\- Validate the request.

\- Verify the sender exists.

\- Verify the scheduled time.

\- Create the email record.

\- Enqueue a delayed job.

\- Return the created email.



\### GET /api/emails



Purpose:

List emails.



Optional query parameters:



\- status

\- senderId

\- page

\- limit



\### GET /api/emails/scheduled



Purpose:

List scheduled emails.



\### GET /api/emails/sent



Purpose:

List successfully sent emails.



\### GET /api/emails/:id



Purpose:

Retrieve an email's details.



\### DELETE /api/emails/:id



Purpose:

Cancel a scheduled email.



Cancellation is allowed only when the email

has not already been sent or is actively processing.



\## 4. Response Format



Successful response:



{

&#x20; "success": true,

&#x20; "data": {},

&#x20; "message": "Request successful"

}



Error response:



{

&#x20; "success": false,

&#x20; "message": "An error occurred",

&#x20; "errors": \[]

}



\## 5. Validation



\- Request payloads will be validated using Zod.

\- Invalid input will return HTTP 400.

\- Missing resources will return HTTP 404.

\- Unexpected server errors will return HTTP 500.



\## 6. Pagination



List APIs will support pagination.



Parameters:



\- page: Page number, starting from 1.

\- limit: Number of records per page.



Pagination metadata will be included in responses.



\## 7. Security



\- SMTP passwords must never be returned through API responses.

\- SMTP passwords must be encrypted before database storage.

\- Inputs must be validated.

\- Internal errors must not expose secrets.



