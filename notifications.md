# Notifications

The platform uses the **Laravel notifications** to send email notifications to the users.

In this section we will define the different notifications  managed by the platform.

## Email notifications

The notification class `App\Notifications\EmailNotification` is the main class used to send email notification to users after an action is done in the platform.

This notification is mainly called by the job [Dispatch notifications](/jobs?id=dispatch-notifications), and send the notification using the default Laravel mail template.

### Usage

Inside the `sendNotification(string $title, string $body, $recipient, string|null $url)` function of the `App\Jobs\DispatchNotificationsJob` class, this notification is called as the following:

```php
use App\Notifications\EmailNotification;

// $this->user is the user object passed to the constructor of the notification class
// $title is a string containing the email subject
// $body is a string that will be displayed in the body of the email to sent
// $url is a string containing the action url to add to the email body (if null the action button will not be added to the email)
$this->user->notify(new EmailNotification($title, $body, $url));
```