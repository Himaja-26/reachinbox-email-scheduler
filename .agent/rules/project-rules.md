\# ReachInbox Email Scheduler - Project Rules



\## 1. Project Overview



Build a production-oriented email scheduling service and dashboard

for the ReachInbox Software Development Intern assignment.



The application must allow users to schedule emails, persist

scheduled jobs, send emails using Ethereal SMTP, and view

scheduled and sent emails through a dashboard.



The original assignment requirements are stored in:



docs/ASSIGNMENT.md



Treat the original assignment as the source of truth.



\---



\## 2. Mandatory Technology Stack



\### Backend

\- TypeScript

\- Node.js

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



Do not introduce additional frameworks, databases, or infrastructure

without explaining why and obtaining approval.



\---



\## 3. Architecture Rules



\- Keep the API server and worker as separate processes.

\- Both processes must use the same backend codebase.

\- API entrypoint: backend/src/server.ts

\- Worker entrypoint: backend/src/workers/index.ts

\- Express application must be separated from the server entrypoint.

\- PostgreSQL is the source of truth for application data.

\- Redis is used for BullMQ and rate-limiting operations.

\- BullMQ must handle delayed and scheduled email jobs.

\- Do not use cron jobs for email scheduling.

\- Job payloads should contain only the emailId.

\- Use the emailId as the BullMQ jobId wherever appropriate

&#x20; to prevent duplicate jobs.

\- Email processing must be idempotent.

\- Rate limiting must be atomic and concurrency-safe.

\- Use Redis Lua scripting for rate-limiting logic.

\- Design for recovery after server or worker restarts.



\---



\## 4. Folder Structure



Follow the approved project folder structure.



Do not reorganize folders or change architectural boundaries

without prior approval.



Backend and frontend must remain separate.



\---



\## 5. Phase-Based Development



Implement the project strictly according to the approved

12-phase development plan.



Never implement multiple phases at once.



Before starting a phase:



1\. Explain the objectives of the phase.

2\. Explain the files that will be created or modified.

3\. Explain the implementation approach.

4\. Identify dependencies on previous phases.

5\. Present the implementation plan for review.



Wait for explicit approval before implementation.



After completing a phase:



1\. Run the appropriate tests.

2\. Run build or type-check commands where applicable.

3\. Verify the implementation.

4\. Explain what was implemented.

5\. Provide verification instructions.

6\. Provide a CHECKPOINT summary.

7\. Identify known issues, if any.



Do not begin the next phase until explicitly instructed.



\---



\## 6. Environment



Environment: Windows 11 + PowerShell + Docker Desktop (WSL2).



All terminal commands must be compatible with Windows PowerShell.



Avoid Linux-only commands.



Use Windows-compatible commands such as:



\- copy instead of cp

\- PowerShell-compatible environment commands

\- Windows-compatible paths



Do not assume the user is using Git Bash.



\---



\## 7. Terminal and Command Safety



Before running commands that:



\- Install packages

\- Modify the system

\- Modify environment configuration

\- Delete files

\- Change Docker configuration

\- Modify databases

\- Perform migrations

\- Modify Git history



Explain the command and its purpose.



Request approval before executing sensitive or destructive commands.



Never delete project files or folders without explicit approval.



Never overwrite user changes without checking them first.



Do not expose secrets in terminal output.



\---



\## 8. Git Workflow



\- Do not commit directly to remote repositories.

\- Do not push code unless explicitly requested.

\- Use Git checkpoints after approved phases.

\- Do not overwrite or reset existing Git history.

\- Explain the changes before committing.

\- Avoid committing .env files, credentials, or secrets.

\- Keep .env.example safe for version control.



\---



\## 9. Coding Standards



\- Use TypeScript wherever required.

\- Prefer modular, maintainable code.

\- Keep controllers, services, routes, workers and configuration

&#x20; responsibilities separate.

\- Avoid unnecessary abstractions.

\- Avoid duplicated logic.

\- Validate external input.

\- Handle errors consistently.

\- Use environment variables for configuration.

\- Do not hardcode secrets.

\- Add meaningful logs.

\- Avoid silent failures.

\- Use graceful shutdown for server and worker processes.



\---



\## 10. Database and Reliability



\- PostgreSQL must persist application data.

\- Redis must persist queue data using appropriate Docker

&#x20; persistence configuration.

\- Use Prisma migrations rather than manually modifying the

&#x20; database schema.

\- Ensure scheduled jobs survive application restarts.

\- Avoid duplicate email sending.

\- Handle worker crashes and retries safely.

\- Handle job failures explicitly.

\- Ensure rate limiting remains safe under concurrent workers.



\---



\## 11. Testing



\- Write tests for important functionality.

\- Test API validation and error handling.

\- Test queue scheduling.

\- Test worker processing.

\- Test duplicate prevention.

\- Test rate limits.

\- Test restart and recovery behavior.

\- Do not claim a test passed unless it was actually executed.



\---



\## 12. Communication



Act as a collaborative engineering agent, not an autonomous

developer who makes every decision independently.



Explain technical decisions in understandable language.



The user is learning the project architecture and implementation.



Prefer simple, reliable solutions over unnecessary complexity.



Do not skip explanations merely because the implementation

is technically straightforward.



When something fails:



1\. Explain the actual error.

2\. Identify the likely cause.

3\. Propose the smallest reasonable fix.

4\. Explain the fix before applying it.



\---



\## 13. Scope Control



Build only what is required by the original assignment

and the approved plan.



Optional features must not be implemented before mandatory

requirements are completed and tested.



Do not add:



\- Unrequested authentication systems

\- Unrequested cloud services

\- Unrequested monitoring systems

\- Unrequested frameworks

\- Unrequested deployment platforms

\- Unrequested architectural complexity



Ask before adding anything that significantly changes

the project scope.

