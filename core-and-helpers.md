# Core and Helpers

Below we will let you know more details about the namespaces `App\Core` and `App\Helpers`.

## Core constants

The **Core** namespace (`App\Core`) mainly contains the **Enumerations** `enum` that are used everywhere in the application source code, to make it simple to manage constants for **Roles**, **Configurations**, **Notifications**, ...

### Configurations

The first enumeration in this namespace is `ConfigurationConstants`:

```php
<?php

namespace App\Core;

use App\Models\Configuration;

enum ConfigurationConstants
{

    public static function case(string $name): bool
    {
        return Configuration::where('name', $name)->first()?->is_enabled ?? false;
    }

}
```

This enumeration will let you retrieve if a configuration is enabled or not from the database.

> This configurations are managed in the **Administration** section of Forumium

#### Usage

Below is an example of usage:

```php
use App\Core\ConfigurationConstants;

// "Enable public discussions" is a configuration created by the administrator
$can_make_discussions_public = ConfigurationConstants::case('Enable public discussions');
```

#### Alias

You can use the following alias to call this enumeration:

```php
// "Enable public discussions" is a configuration created by the administrator
$can_make_discussions_public = Configurations::case('Enable public discussions');
```

### Followers

```php
<?php

namespace App\Core;

enum FollowerConstants: string
{

    case NONE = "none";
    case FOLLOWING = "following";
    case NOT_FOLLOWING = "not-following";
    case IGNORING = "ignoring";

}
```

This enumeration define the different type of following a discussion:
- `case NONE = "none"`: means the discussion is not followed by the user (no status is registered yet in the database)
- `case FOLLOWING = "following"`: means the discussion is followed by the user (and want to receive all notification about this discussion)
- `case NOT_FOLLOWING = "not-following"`: means the discussion is not followed by the user (the user want to be notified only when he is mentioned)
- `case IGNORING = "ignoring"`: means the discussion is ignored by the user (the user will never be notified about the discussion)

> The values `following`, `not-following` and `ignoring` is the values that will be used in the column `type` of the table `followers`

#### Usage

Below is an example of usage:

```php
use App\Core\FollowerConstants;

// To get the value to store in the column `type` of the table `followers` for the ignoring case
$type = FollowerConstants::IGNORING->value;
```

#### Alias

You can use the following alias to call this enumeration:

```php
// To get the value to store in the column `type` of the table `followers` for the ignoring case
$type = Followers::IGNORING->value;
```

### Notifications

```php
<?php

namespace App\Core;

enum NotificationConstants: string
{

    case MY_DISCUSSION_EDITED = "Someone edited my discussion";
    case POST_IN_DISCUSSION = "Someone posts in a discussion I'm following";
    case MY_REPLY_BEST_ANSWER = "Someone sets my reply as a best answer";
    case MY_POSTS_COMMENTED = "Someone commented to one of my posts";
    case MY_POSTS_LIKED = "Someone likes one of my posts";
    case POINTS_UPDATED = "My account points are updated after an action";
    case DISCUSSION_LOCKED = "My discussion is locked by a moderator / administrator";
    case DISCUSSION_UNLOCKED = "My discussion is unlocked by a moderator / administrator";

}
```

> This notifications constants are managed in the **Administration** section of Forumium
>
> **IMPORTANT:** if you want to add / update / remove some notifications in the database, you need to make sure the notification value (column `name` of the table `app_notifications`) is the same as the value of the enumeration case

#### Usage

Below is an example of usage:

```php
use App\Core\NotificationConstants;

// To get a specific notification value
$notification = NotificationConstants::POST_IN_DISCUSSION->value;
```

#### Alias

You can use the following alias to call this enumeration:

```php
// To get a specific notification value
$notification = Notifications::POST_IN_DISCUSSION->value;
```

### Permissions

```php
<?php

namespace App\Core;

enum PermissionConstants: string
{

    case START_DISCUSSIONS = 'Start discussions';
    case REPLY_TO_DISCUSSIONS = 'Reply to discussions';
    case LIKE_POSTS = 'Like posts';
    case COMMENT_POSTS = 'Comment posts';
    case PIN_DISCUSSIONS = 'Pin discussions';
    case EDIT_POSTS = 'Edit posts';
    case DELETE_POSTS = 'Delete posts';
    case VIEW_POSTS_STATS = 'View posts stats';
    case LOCK_DISCUSSIONS = 'Lock discussions';

}
```

> This permissions constants are managed in the **Administration** section of Forumium
>
> **IMPORTANT:** same as the notifications constants, if you want to add / update / remove some permissions in the database, you need to make sure the notification value (column `name` of the table `permissions`) is the same as the value of the enumeration case

#### Usage

Below is an example of usage:

```php
use App\Core\PermissionConstants;

// To get a specific permission value
$notification = PermissionConstants::EDIT_POSTS->value;
```

#### Alias

You can use the following alias to call this enumeration:

```php
// To get a specific permission value
$notification = Permissions::EDIT_POSTS->value;
```

### Points

```php
<?php

namespace App\Core;

enum PointsConstants: string
{

    case START_DISCUSSION = 'start-discussion';
    case DISCUSSION_DELETED = 'discussion-deleted';
    case DISCUSSION_LIKED = 'discussion-liked';
    case DISCUSSION_DISLIKED = 'discussion-disliked';

    case NEW_REPLY = 'new-reply';
    case REPLY_DELETED = 'reply-deleted';
    case BEST_REPLY = 'best-reply';
    case BEST_REPLY_REMOVED = 'best-reply-removed';
    case REPLY_LIKED = 'reply-liked';
    case REPLY_DISLIKED = 'reply-disliked';

    case NEW_COMMENT = 'new-comment';
    case COMMENT_DELETED = 'comment-deleted';
    case COMMENT_LIKED = 'comment-liked';
    case COMMENT_DISLIKED = 'comment-disliked';

    public static function value(string $name): int
    {
        $points = [
            self::START_DISCUSSION->value => 10,
            self::DISCUSSION_DELETED->value => -10,
            self::DISCUSSION_LIKED->value => 5,
            self::DISCUSSION_DISLIKED->value => -5,

            self::NEW_REPLY->value => 2,
            self::REPLY_DELETED->value => -2,
            self::BEST_REPLY->value => 10,
            self::BEST_REPLY_REMOVED->value => -10,
            self::REPLY_LIKED->value => 2,
            self::REPLY_DISLIKED->value => -2,

            self::NEW_COMMENT->value => 1,
            self::COMMENT_DELETED->value => -1,
            self::COMMENT_LIKED->value => 1,
            self::COMMENT_DISLIKED->value => -1,
        ];
        return $points[$name] ?? 0;
    }

    public static function caseToDelete(string $name): bool
    {
        return self::value($name) < 0;
    }

}
```

> This enumeration is mainly used in the application to make it simple to calculate the users points on the forum different actions: **start discussion**, **new reply**, **new comment**, ...
>
> It contains constants to let you define and use the different forum actions, and also contains two functions:
>
> - `value(string $name)`: that returns the action points to add (if value is positive) or subtract (if value is negative)
> - `caseToDelete(string $name)`: returns `true` if the value of the given action is negative, and `false` if this value is positive

#### Usage

Below is an example of usage:

```php
use App\Core\PointsConstants;

// To get the action name
$action = PointsConstants::START_DISCUSSION->value;

// To get the points value of an action
$value = PointsConstants::value($action);
```

#### Alias

You can use the following alias to call this enumeration:

```php
// To get the action name
$action = Points::START_DISCUSSION->value;

// To get the points value of an action
$value = Points::value($action);
```

### Roles

```php
<?php

namespace App\Core;

enum RoleConstants: string
{

    case ADMIN = 'Admin';
    case MOD = 'Mod';
    case MEMBER = 'Member';

}
```

> This roles are managed in the **Administration** section of Forumium
>
> **IMPORTANT:** same as the notifications constants, if you want to add / update / remove some roles in the database, you need to make sure the role value (column `name` of the table `roles`) is the same as the value of the enumeration case

#### Usage

Below is an example of usage:

```php
use App\Core\RoleConstants;

// Get the member role name
$member_role = RoleConstants::MEMBER->value;
```

#### Alias

You can use the following alias to call this enumeration:

```php
// Get the member role name
$member_role = Roles::MEMBER->value;
```

### Socials

```php
<?php

namespace App\Core;

enum SocialConstants: string
{

    case FACEBOOK = "facebook";
    case GITHUB = "github";
    case GOOGLE = "google";
    case TWITTER = "twitter";

    case ENABLED_SOCIALS = "facebook,github,google";

    public static function color(string $name)
    {
        return match ($name) {
            self::FACEBOOK->value => '3b5998',
            self::GITHUB->value => '24292F',
            self::GOOGLE->value => 'DB4437',
            self::TWITTER->value => '00ACEE',
        };
    }

    public static function isEnabled(string $name)
    {
        $enabledSocials = explode(',', self::ENABLED_SOCIALS->value);
        return in_array($name, $enabledSocials);
    }

    public static function enabledCases()
    {
        $socials = [];
        foreach (array_column(self::cases(), 'value') as $social) {
            if (self::isEnabled($social)) {
                $socials[] = $social;
            }
        }
        return $socials;
    }

}
```

> This enumeration contains constants defining the social network managed by the platform, with a constants `ENABLED_SOCIALS` containing the enabled (to be shown) socials (as a `string` separated by a comma `,`)
>
> And also three methods:
>
> - `color(string $name)`: defines and returns a color for the social passed in parameter
> - `isEnabled(string $name)`: returns `true` if the given social is enabled (exists in the constant `ENABLED_SOCIALS`), and `false` otherwise
> - `enabledCases()`: returns an array of the enabled socials cases values
>
> **IMPORTANT:** if you want to add other social networks, after you update this enumeration you must add the service array into `config/services.php` file (you can check the `config('services.facebook')` to get an example)

#### Usage

Below is an example of usage:

```php
use App\Core\SocialConstants;

// Get the facebook social name
$facebook = SocialConstants::FACEBOOK->value;

// Get the color of the social network
$facebook_color = SocialConstants::color($facebook);

// Check if the social network is enabled
$facebook_is_enabled = SocialConstants::isEnabled($facebook);
```

#### Alias

You can use the following alias to call this enumeration:

```php
// Get the facebook social name
$facebook = Socials::FACEBOOK->value;

// Get the color of the social network
$facebook_color = Socials::color($facebook);

// Check if the social network is enabled
$facebook_is_enabled = Socials::isEnabled($facebook);
```

## Helpers

The **Helpers** namespace (`App\Helpers`) mainly contains the classes / traits used as helpers inside the application source code.

> For now only the `CustomUserAvatar` helper is used
>
> But if you want to add other helpers, or in the futur if the application will get other helpers, they need to be in this namespace

### Custom user avatar

This helper is used by the package `devaslanphp/filament-avatar`, and it contains only one function that lets this package retrieve the user's avatar from database or use the `UiAvatarsProvider()` of this same package if the user's avatar is `null`.

```php
<?php

namespace App\Helpers;

use Devaslanphp\FilamentAvatar\Core\UiAvatarsProvider;
use Illuminate\Database\Eloquent\Model;

class CustomUserAvatar
{

    /**
     * Get the user avatar (picture) url from a user object
     *
     * @param Model $user
     * @return string
     */
    public function get(Model $user): string
    {
        $field = config('filament-avatar.providers.custom-avatar.name_field');
        if ($user->{$field}) {
            return asset('storage/' . $user->{$field});
        }
        return (new UiAvatarsProvider())->get($user);
    }

}
```

> For more details about this package, please see: [Package repository](https://github.com/devaslanphp/filament-avatar)