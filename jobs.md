# Jobs

In this section we will define the jobs used by the platform to perform different tasks, as:

- **Calculate user points**: calculate a user points after he did an action in the platform
- **Calculate all users points**: it's a batch version of the previous task to calculate all database users points, mainly used by the `Database\Seeders\Content\PointsSeeder` that injects demo data into your database (refer to the [Database](/setting-up?id=database) section)
- **Dispatch notifications**: dispatch notification based on it's type via web and email based on the users configurations

## Calculate user points

The job `App\Jobs\CalculateUserPointsJob` is used to calculate the new points of a user after he did an action in the platform.

### Usage

To use this job, you need to call it like below:

```php
use App\Core\PointsConstants;
use App\Jobs\CalculateUserPointsJob;

// Calculate the authenticated user points after he started a new discussion
// "user" parameter must contain the User|Authenticatable object, this is the user to calculate his points
// "source" parameter must contain the object that the action was performed on it
// "type" parameter must contain a  constant from PointsConstants depending on what the user did as an action
dispatch(
    new CalculateUserPointsJob(
        user: auth()->user(),
        source: $discussion,
        type: PointsConstants::START_DISCUSSION->value
    )
);
```

## Calculate all users points

The job `App\Jobs\CalculateAllUsersPointsJob` is mainly used by the seeder `Database\Seeders\Content\PointsSeeder`, it's a **Batch** version of the previous job `CalculateUserPointsJob`, and it calculate the points for all the database users.

### Usage

To use this job, you only need to call it as a dispatched job:

```php
use App\Jobs\CalculateAllUsersPointsJob;

// Dispatch the job
dispatch(new CalculateAllUsersPointsJob);
```

> This job will reset all users total points before recalculating them again
> 
> **IMPORTANT:** depending on how many users you got on the database, this job can take several minutes, so we recommend that you have **Laravel queues** system enabled and running

## Dispatch notifications

The job `App\Jobs\DispatchNotificationsJob` is used to send notification via web or via emails to a user based on this user notifications configuration.

### Usage

Here is how you can use this job:

```php
use App\Core\NotificationConstants;
use App\Jobs\DispatchNotificationsJob;

// Send a notification to the authenticated user after a post (reply or comment) is added to a discussion
// "user" parameter must contain the User|Authenticatable object, this is the user who did the action
// "notification" parameter must contain notification type
// "object" parameter must contain the object that the action was performed on it
dispatch(
    new DispatchNotificationsJob(
        user: auth()->user(),
        notification: NotificationConstants::POST_IN_DISCUSSION->value,
        object: $discussion
    )
);
```

> Based on the user's notifications configuration, the job will send a web notification, send an email or both