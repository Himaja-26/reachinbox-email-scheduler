\# ReachInbox Email Scheduler - Development Plan



\## 1. Project Overview



Build a full-stack email scheduling application that allows users

to configure email senders, schedule emails for future delivery,

and manage scheduled and sent emails.



The system must support delayed job processing, persistent

scheduling, sender-specific rate limits, and reliable job execution.



\## 2. Technology Stack



\### Backend

\- Node.js

\- TypeScript

\- Express.js

\- PostgreSQL

\- Prisma ORM

\- Redis

\- BullMQ

\- Nodemailer

\- Zod

\- Vitest



\### Frontend

\- React

\- TypeScript

\- Vite

\- Tailwind CSS

\- Axios



\### Infrastructure

\- Docker

\- Docker Compose



\## 3. System Architecture



The application consists of the following components:



\### Frontend

Provides the user interface for:

\- Managing email senders

\- Composing emails

\- Scheduling emails

\- Viewing scheduled emails

\- Viewing sent emails



\### Backend API

Responsible for:

\- Request validation

\- Sender management

\- Email scheduling

\- Email cancellation

\- Email status management

\- Database operations



\### PostgreSQL

Acts as the persistent source of truth for:

\- Senders

\- Emails

\- Email scheduling metadata

\- Email delivery status



\### Redis

Used for:

\- BullMQ queue infrastructure

\- Delayed job management

\- Shared rate-limit state



\### Email Worker

A separate process responsible for:

\- Retrieving queued jobs

\- Claiming emails for processing

\- Applying rate limits

\- Sending emails through SMTP

\- Updating delivery status

\- Handling retries and failures



\### SMTP

Nodemailer will send emails using test SMTP

credentials provided by Ethereal.



\## 4. Project Development Phases



\### Phase 1 - Planning and Architecture

\- Document the project architecture

\- Define database entities

\- Define API endpoints

\- Define queue and rate-limit strategy

\- Document environment configuration



\### Phase 2 - Infrastructure and Backend Setup

\- Initialize backend TypeScript project

\- Configure Express

\- Configure Docker Compose

\- Start PostgreSQL and Redis

\- Implement health checks



\### Phase 3 - Database

\- Configure Prisma

\- Create database schema

\- Implement migrations

\- Create seed data



\### Phase 4 - API Foundation

\- Implement sender APIs

\- Implement email APIs

\- Add request validation

\- Implement error handling



\### Phase 5 - Queue Integration

\- Configure BullMQ

\- Implement delayed scheduling

\- Persist queue job references



\### Phase 6 - Worker Implementation

\- Create separate worker process

\- Implement job processing

\- Handle concurrency

\- Implement graceful shutdown



\### Phase 7 - Email Delivery

\- Integrate Nodemailer

\- Configure Ethereal SMTP

\- Implement delivery tracking



\### Phase 8 - Rate Limiting

\- Implement Redis-based atomic rate limiter

\- Enforce hourly sender limits

\- Enforce minimum delay between emails

\- Reschedule throttled jobs



\### Phase 9 - Frontend

\- Create dashboard

\- Implement sender management

\- Implement email composition

\- Implement scheduled emails

\- Implement sent emails



\### Phase 10 - Integration

\- Integrate frontend, backend, queue, worker and database

\- Test complete scheduling and delivery flow

\- Test restart and recovery scenarios



\### Phase 11 - Testing

\- Test API endpoints

\- Test queue behavior

\- Test worker behavior

\- Test rate limiting

\- Test failure handling

\- Test duplicate prevention



\### Phase 12 - Finalization

\- Complete documentation

\- Add setup instructions

\- Add demo data

\- Verify fresh setup

\- Prepare demonstration



\## 5. Reliability Principles



\- PostgreSQL will be the source of truth.

\- Redis will be used for queue infrastructure and rate limiting.

\- API and worker will run as separate processes.

\- Queue jobs will reference emails using email IDs.

\- Deterministic job IDs will help prevent duplicate queue jobs.

\- Email processing will use database-level safeguards.

\- Rate-limit decisions must be atomic.

\- Failed jobs will support retries where appropriate.

\- The system will handle restarts and recover pending work.

\- SMTP delivery will not be treated as exactly-once delivery.



\## 6. Development Workflow



Development will proceed phase by phase.



Each phase must be:

1\. Implemented

2\. Tested

3\. Verified

4\. Documented where necessary



The next phase will begin only after the current phase

has been reviewed and its checkpoint verified.

