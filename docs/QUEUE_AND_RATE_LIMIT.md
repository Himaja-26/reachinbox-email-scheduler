\# Queue and Rate-Limit Design



\## 1. Queue Architecture



BullMQ will be used to manage scheduled email jobs.



Redis will act as the queue's underlying storage and coordination

mechanism.



The backend API will create jobs, while a separate worker process

will consume and execute them.



\## 2. Job Payload



Queue jobs will contain only the email ID.



Example:



{

&#x20; "emailId": "email-uuid"

}



The worker will retrieve the email and sender information

from PostgreSQL.



This avoids storing email content or SMTP credentials

in queue payloads.



\## 3. Scheduling



When an email is scheduled:



1\. Validate the request.

2\. Store the email in PostgreSQL.

3\. Create a BullMQ delayed job.

4\. Use the email ID as the deterministic job ID.

5\. Update the database with the queue reference if needed.



\## 4. Worker Processing



The worker will:



1\. Receive the email ID.

2\. Load the email from PostgreSQL.

3\. Check its current status.

4\. Claim it safely for processing.

5\. Apply sender-specific rate limits.

6\. Send the email through SMTP.

7\. Update the email status.

8\. Record delivery or failure details.



\## 5. Rate-Limiting Strategy



Each sender will have two rate-limit settings:



\- Hourly limit

\- Minimum delay between consecutive emails



Rate limits will be shared across workers using Redis.



\## 6. Atomic Rate-Limit Enforcement



A Redis Lua script will be used to perform rate-limit

checks and state updates atomically.



This prevents multiple concurrent workers from independently

passing the same rate-limit check.



\## 7. Hourly Limit



The hourly limit will be tracked per sender.



Redis will maintain a rolling or time-window counter

according to the implemented rate-limit strategy.



The counter will expire when its tracking window ends.



\## 8. Minimum Delay



The minimum delay between emails from the same sender

will be enforced using shared Redis state.



If an email cannot be sent yet, the worker will calculate

the next eligible delivery time.



\## 9. Rescheduling



When rate limits prevent sending:



\- The email will not be marked as sent.

\- The job will be delayed until its next eligible time.

\- The database status will remain consistent with pending delivery.



\## 10. Retries



Transient SMTP or infrastructure errors may be retried.



Retry configuration will include:



\- Maximum attempts

\- Backoff strategy

\- Failure tracking



Permanent errors should not be retried indefinitely.



\## 11. Idempotency and Duplicate Prevention



\- Queue job IDs will be deterministic.

\- Database processing claims will be guarded.

\- Completed or cancelled emails should not be processed again.

\- Worker logic will check database status before sending.



SMTP delivery cannot be guaranteed to be exactly once.



\## 12. Restart Recovery



The system must handle restarts of the API, worker, Redis,

and database as gracefully as practical.



Pending jobs should remain persistent.



Database and queue state should be reconciled when necessary.



\## 13. Cancellation



Scheduled emails may be cancelled before processing begins.



Cancellation must update database state and remove or invalidate

the corresponding queue job.



Workers must check cancellation state before sending.

