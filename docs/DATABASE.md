\# Database Design



\## 1. Overview



The application will use PostgreSQL as its persistent database

and Prisma ORM to interact with the database.



The database will store sender configurations, scheduled emails,

email delivery status, and processing metadata.



\## 2. Entities



\### Sender



Represents an email account configured by the user for sending emails.



Fields:



\- id: UUID, primary key

\- name: Sender display name

\- email: Sender email address

\- smtpHost: SMTP server hostname

\- smtpPort: SMTP port

\- smtpUsername: SMTP username

\- smtpPasswordEncrypted: Encrypted SMTP password

\- hourlyLimit: Maximum emails allowed per hour

\- minDelaySeconds: Minimum delay between consecutive emails

\- createdAt: Timestamp

\- updatedAt: Timestamp



\### Email



Represents an email scheduled or sent through the system.



Fields:



\- id: UUID, primary key

\- senderId: UUID, foreign key referencing Sender

\- recipient: Recipient email address

\- subject: Email subject

\- body: Email content

\- scheduledAt: Scheduled delivery time

\- status: Email status

\- queueJobId: BullMQ job identifier

\- attempts: Number of processing attempts

\- sentAt: Actual delivery timestamp

\- errorMessage: Last recorded error

\- createdAt: Timestamp

\- updatedAt: Timestamp



\## 3. Relationships



\- One Sender can have many Emails.

\- Each Email belongs to exactly one Sender.

\- Deleting a Sender must not silently delete its email history.



\## 4. Email Statuses



The application will support these statuses:



\- SCHEDULED

\- PROCESSING

\- SENT

\- FAILED

\- CANCELLED



\## 5. Constraints and Considerations



\- IDs will use UUIDs.

\- Sender email addresses should be unique.

\- Emails must reference an existing sender.

\- Emails should not be scheduled in the past.

\- SMTP passwords must be encrypted before being stored.

\- Email status transitions must be controlled by the backend.

\- Email history should remain available for tracking.

\- Database timestamps should be stored consistently in UTC.

\- The database will be the source of truth for email records.



\## 6. Reliability



\- Database transactions will be used where necessary.

\- Processing claims must prevent multiple workers from

&#x20; simultaneously processing the same email.

\- Database state must be reconciled with queue state when needed.

\- SMTP delivery cannot be guaranteed to occur exactly once.

