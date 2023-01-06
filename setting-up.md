# Setting Up

Forumium uses the **Laravel Framework** so you can refer to the [official documentation](https://laravel.com/docs) for more details on installation and configuration.

But we will put in this documentation only the steps needed to install and configure the Forumium platform.

## Installation

First of all you need to clone the Forumium repository, to do that go to Forumium [Github repository](https://github.com/devaslanphp/forumium) and clone it into your development environment : 

```bash
git clone https://github.com/devaslanphp/forumium.git
```

> Be sure to clone the `master` branch to get the latest developments

## Dependencies

As mentioned before, Forumium uses the **Filament TALLkit** and other dependencies like **TailwindCSS**...

So after you have cloned the repository, the first thing to do is to install *Back* and *Front* dependencies, to do so execute the following commands:

### Back dependencies

You need to install the *Back* dependencies using **Composer**, so if you don't have Composer installed yet on your development environment, please refer to this [documentation](https://getcomposer.org/download/) to install it.

```bash
composer install
```

### Front dependencies

Now, you need to install the *Front* dependencies using your favorite package manager **NPM** or **YARN**.

> Please refer to the following documentations to install NPM or YARN:
> - NPM: [https://docs.npmjs.com/downloading-and-installing-node-js-and-npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
> - YARN: [https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable](https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable)

Execute the following command in the Forumium root folder (cloned from Github).

Install dependencies with **NPM**:

```bash
npm install
```

Or, install dependencies with **YARN**:

```bash
yarn
```

## Assets

Now that you have *Back* and *Front* dependencies, you need to build the platform assets, to do so execute the following command in the Forumium root folder:

```bash
npm run build
```

Or, if you prefer **YARN** over **NPM** you can execute this command:

```bash
yarn build
```

> Forumium pltaform uses **Vite** to manage the assets, so you can refer to this [documentation](https://legacy.laravel-vite.dev/guide/introduction.html) for more details.

## Configuration

The next step is to create the Forumium environment file, to do so you need to copy the file `.env.example` into a new file named `.env`.

This file will already contain some pre-configured values like:

- **APP_NAME**: with the default value *Forumium*, but you can change it as you like
- **LOG_CHANNEL**: with the default value *daily*, we like this option because it makes more sense to have separated (daily) log files, but feel free to change it
- **QUEUE_CONNECTION**: with the default value *database*, you can change it to *sync* but we **highly recommand** to let it *database* and use the Laravel queue system at least for the database configuration step

After you have created the `.env` file, you need to generate the application key by executing the following command:

```bash
php artisan key:generate
```

## Database

There is two ways to configure the Forumium database for the first installation:

- **Manual configuration**: a few commands to execute in order
- **Automatic configuration**: a single *Composer* custom command that will execute all the commands for you

### Manual configuration

First, you need to run the database migrations:

```bash
php artisan migrate
```

Then, you need to run the database seeders:

```
php artisan db:seed
```

> Here the database configuration is done, you got the database structure and the **minimal data** inserted by the seeder to make the application work

If you want some **demo data** to be inserted, you need to run the following commands:

```
php artisan db:seed --class=ContentSeeder
php artisan db:seed --class=NotificationSeeder
```

> **IMPORTANT**: Before you execute demo data seeder, you need to run queue listener: `php artisan queue:work`, because there is some data that will be inserted via jobs and that will take couple minutes


### Automatic configuration

If you choose the automatic configuration, you must know that by executing the following *Composer* custom command, your database will be wiped and all the default and demo data will be inserted instead of your current data, so be CAREFUL!

All you need to do is execute the following command, and go take a coffee :coffee: and let the magic :sparkles: happen:

```
composer run app-install
```

> **IMPORTANT**: Before you execute the command above, you need to run queue listener: `php artisan queue:work`, because there is some data that will be inserted via jobs and that will take couple minutes
> 
> Please note that this command will insert also demo data into your database (if you don't need demo data, please use the **manual configuration** above)

## Serve

After you have configured all the steps above, you can serve the application by using the Laravel integrated server:

```bash
php artisan serve
```

> Or you can use your preferred web server, for that I will let you do it yourslef :sunglasses: 

The database seeder already executed will create at least two users:

- **Administrator**
  - Email address: *admin@forumium.app*
  - Password: *password*

- **Moderator**
  - Email address: *mod@forumium.app*
  - Password: *password*

> **IMPORTANT**: if you have executed the **demo data seeders**, you will have **30 other users** (with the *Member* role) inserted also
> 
> You can check the database (table: `users`) to get their *email addresses*, and all their passwords will be *password* (the same as *Administrator* and *Moderator*)

