---
type: feature
title: Background Jobs
description: The background job infrastructure in the FridgePilot backend.
tags: ['backend', 'jobs', 'background']
---
# Background Jobs

The FridgePilot backend includes a base class for background jobs, `ApplicationJob`, which provides common error handling for deadlock retries and deserialization errors. While no specific background jobs are currently defined in the codebase, the infrastructure is in place for future implementation.

## ApplicationJob

Located at `/backend/app/jobs/application_job.rb`, the `ApplicationJob` class inherits from `ActiveJob::Base` and includes modules for retrying on deadlocks and handling deserialization errors.

### Key Features

- **Deadlock Retry**: Automatically retries the job if a deadlock is detected (using `retry_on ActiveRecord::Deadlocked`).
- **Deserialization Error Handling**: Retries the job if a deserialization error occurs (using `retry_on ActiveJob::DeserializationError`).

### Code Example

```ruby
class ApplicationJob < ActiveJob::Base
  retry_on ActiveRecord::Deadlocked
  retry_on ActiveJob::DeserializationError
end
```

## Usage

To create a background job, inherit from `ApplicationJob` and implement the `perform` method.

```ruby
class ExampleJob < ApplicationJob
  def perform(*args)
    # Job logic here
  end
end
```

Jobs can be enqueued using:
- `ExampleJob.perform_later(*args)` for asynchronous execution.
- `ExampleJob.perform_now(*args)` for synchronous execution (mainly for testing).

## Configuration

Background job processing requires a queue adapter (e.g., Sidekiq, Resque, or the built-in async adapter). The adapter is configured in `config/active_job.rb` and set per environment in `config/environments/*.rb`.

In the development and test environments, the `:async` adapter is used by default, which runs jobs inline. For production, a persistent queue adapter like Sidekiq should be configured.

## Related Files

- `/backend/app/jobs/application_job.rb`: The base job class.
- `config/active_job.rb`: ActiveJob configuration.
- `config/environments/*.rb`: Environment-specific ActiveJob configuration.

## Testing

Background jobs can be tested by:
- Calling `perform_now` in unit tests to test the job logic directly.
- Using ActiveJob's test helper to assert that jobs were enqueued (when using a queue adapter in tests).

## Future Work

As of the current codebase, no specific background jobs are implemented. Potential use cases for background jobs in FridgePilot include:
- Periodic synchronization with external services (e.g., TheMealDB).
- Sending email notifications.
- Expired pantry item cleanup.
- Image processing for uploaded recipe photos.