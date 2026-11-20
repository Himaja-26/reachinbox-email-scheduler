\# Environment Configuration



\## 1. Purpose



Environment variables will be used to configure the application

without hardcoding credentials or environment-specific settings.



The backend will validate required environment variables

at startup using Zod.



\## 2. Backend Configuration



\### Application



\- NODE\_ENV: Application environment

\- PORT: Backend API port



\### Database



\- DATABASE\_URL: PostgreSQL connection URL



\### Redis



\- REDIS\_HOST: Redis hostname

\- REDIS\_PORT: Redis port

\- REDIS\_PASSWORD: Optional Redis password



\### SMTP Credential Encryption



\- ENCRYPTION\_KEY: Secret key used to encrypt sender SMTP passwords



\### Queue



\- EMAIL\_QUEUE\_NAME: BullMQ queue name



\### Email Processing



\- WORKER\_CONCURRENCY: Number of jobs a worker can process concurrently



\## 3. Example Configuration



Example values for local development:



\- NODE\_ENV=development

\- PORT=3000

\- REDIS\_HOST=localhost

\- REDIS\_PORT=6379

\- EMAIL\_QUEUE\_NAME=email-scheduler

\- WORKER\_CONCURRENCY=5



Sensitive values must not be committed to Git.



\## 4. Security



\- Secrets must be stored in environment variables.

\- The `.env` file must not be committed.

\- `.env.example` may contain placeholder values.

\- SMTP passwords must be encrypted before database storage.

\- Encryption keys must not be exposed in logs.

\- Environment variables must be validated at application startup.

