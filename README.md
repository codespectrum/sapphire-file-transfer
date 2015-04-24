# Sapphire File Transfer

## What you should know

The application, in it's original configuration, requires very minimum configuration to work properly. Note however, that by being based on Laravel 5, a lot of alternative configurations are possible.

The installation chapter deals with the very minimum steps you need to get your application up and running. If you want to go further, and switch the local storage for an AWS S3 one for example, read the appropriate sections.


## Requirements

Php >= 5.5 (if you need Php 5.4, drop us a line), with the following extensions

* Mcrypt PHP Extension
* OpenSSL PHP Extension
* Mbstring PHP Extension
* Tokenizer PHP Extension

Apache 2 (with mod_rewrite enabled) or Nginx

MySQL or any other database supported by Laravel

## Installation

First of all, note that the installation process doesn't cover the server setup.

Here you go:

1. Deploy the files on your server.
2. Set the document root of your project on the "public" folder of the application.
3. Give server write/read access to the storage folder recursively (chmod 777 -R)
4. Create a MySQL database with the name of your choice.
5. Execute the following command from the root of your project: "php artisan migrate"
6. Rename the .env.example to .env, and replace the variables (the configuration file is documented).
7. Set-up your cron job as described in chapter "Cron jobs"
8. Enjoy

If from here you struggle with getting it up and running, send us a quick mail with a detailed explanation of what's going wrong and your eventual errors. (You can also set the debug to true, to see more explanation on the faced error). Without clear information we won't be able to assist you.

We will enhance this installation process with all your feedback, so please, give us your feedback. 

## Configuration

### Change the appearance of your application

To change your logo, background image, or even the html, look inside the 'resources/views' directory of the application. Replace whatever you need.

If you feel the need of a dedicated, user friendly admin interface, drop us a line, we might integrate this feature in an upcoming release.

### Upload Max Size

There is actually no hard limit in the application for the file sizes. However you need to set an upload limit in your php.ini configuration.

You have to set the 2 following entries to the value of your choice: upload_max_filesize and post_max_size.

If you are using Nginx, please configure your server client_max_body_size accordingly.

Here is some [nice article](https://rtcamp.com/tutorials/php/increase-file-upload-size-limit/).

### Upload expiration

You can set the files lifetime en the .env configuration file.
The value must be set in hours, and defaults to a month (720hours = 24hours * 30days) 

Each time a file is uploaded, the expiration is stored on the file's row in the database, so if you change the lifetime, this will not affect existing files.

### Cron jobs

The application must check on a regular basis if there are any close to expiration files, or expired files on the server to send appropriate notifications. Actually those checks are made every hour.

You only have to set-up one cron job as follow:

```
#!shell

*/1 * * * * php /path/to/your/project/artisan schedule:run
```

This will automatically know what to execute, at what frequency. You will only need to set this up once. All upcoming scheduled features will automatically be integrated without any further infrastructure considerations.

We plan on integrating a way to avoid needing to setup a cron job. We know it can be painful to set-up in some environments, and sometimes, it just isn't possible to do so. Tell us if you would love to see this feature, and we'll integrate it right away.



### Localization

Every string in the application is localizable. Those localization files consist of simple array formatted as key/values.
Those translations can be found in the following folder:

```
/resources/lang

```

You can easily override existing translations, or create a new directory with the key of your chosen language.

Not that if a translation for a given key is not available for your language, the application will fallback to the english translation.

Remember to send us your translations if you want to share them with the Envato community. We will list you in the contributors file.


### Emails sending

#### Email Sending

By default, all emails will be sent from your server, but everyone knows that this is not always desired/reliable.
Instead you could use providers like [Mandrill](http://mandrill.com/) or [MailGun](http://www.mailgun.com/).

The easiest way to switch for such a provider, is by creating an account by the chosen provider, and replace the SMTP configuration of the .env file.

You can also change the email used to send the mail and the name of the sender from the config mail.

For more advanced configuration, feel free to checkout [Laravel's documentation on emails](http://laravel.com/docs/5.0/mail).

#### Email Templating

The email templates can be found in 

```
/resources/views/emails/{language}/{email-template-name}.blade.php

```

You change the content and display of those emails there, or even add new translations by copy pasting the "en" directory and replacing the content of the templates.

If email templates are not available in the current configured language, the application will fallback to the english template.



### Queues

ATTENTION: The Swift_Mailer library is not compatible with queues in daemon mode. This will throw SSL exceptions. Please do not run your queues daemon mode.


By default, queues are disabled, because the queue driver is set to "sync", meaning that all tasks will be executed synchronously. This means that for example, when a user uploads files, he has to wait till all email notifications are sent, and the files archive is built before being able to see the actual download page.

If you would like to optimize your application performances, and run those queuable tasks behind the scenes, you could use any other queue driver made available by Laravel 5.
Please take a look here: [Laravel Queues Documentation](http://laravel.com/docs/5.0/queues)

Note that all external php required packages for all queues are already imported into the vendor directory. We also included the default "database" driver migration files. For infrastructure issues, please refer directly to the chosen driver's vendor documentation.

## Advanced

### Extend the backend

If you wish to extend the backend, feel free to do so. The application actually embraces the Laravel coding standards, so your best resource would be the [official Laravel documentation](http://laravel.com/).

If you think some implementations could be done better, drop us a line with your own implementation. We would love to integrate your version and add you to the contributors file.

### Extend the frontend

For any frontend evolution, note that we leveraged the Laravel Elixir package to:

* Compile the less files
* Version the files

Once again, your best documentation resource would be the [official one](http://laravel.com/).


We handled our frontend dependencies with [bower](http://bower.io/).


## Final note

If you have anything to say about this documentation, drop us a line at hello@codespectrum.io

We would gladly improve it with your very own recommendations and feedback.
