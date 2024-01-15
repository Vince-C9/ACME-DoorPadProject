# Tech Test - ACME Corps - Door Access Key

This tech test is the result of a week of struggles and pain!   It has dropped at a time when I am juggling house sales and large projects at work.

I've worked odd hours as and when they're available and have done what I can to get this as far along as possible.

This is the starter project for the app itself, which should come with all the requisite config in place for you to be able to quickstart installing the  key code package, which can be found [here.](https://github.com/Vince-C9/ACME-DoorPad)


If you'd prefer to set the plugin up on your own project, then follow these instructions.  Note that if you are installing this using using the docker-compose file provided here, you may need to prefix some of this with `docker-compose exec api `

Note:  Assumptions are that you have already set up your base laravel project with a key and any standard migrations from out of the box.  If not, [the guide is here](https://laravel.com/docs/10.x/installation)

1. Add the following config to your composer.json.  If you already have a repositories section, make sure that you refactor this to fit in with your current settings.
 ```
"repositories": {
        "vince/acme-door-pad": {
            "type": "git",
            "url": "https://github.com/Vince-C9/ACME-DoorPad.git"
        }
    },
```

2. Now you can run `composer require vince/acme-door-pad` to include the package in your project.
3. You will need to publish the config: `php artisan vendor:publish --tag=acme-config`
4. Finally you will need to migrate the data tables.  These are not yet configurable and one is called "keys" and the other "keypad_users". `php artisan migrate`

You should now have your new tables and config files installed.  Unfortunately I ran out of time to debug a few of the issues with the 'runner' project, and when running on this project, if you try to access the login page, you'll be redirected back to the home page.  Not ideal...

# Console Commands
There are three console commands you should be able to use with this plugin in place to generate keys.

1. You can use `(docker prefix) php artisan security:keyAdd --amount=x` to queue up X amount of keys.  This will batch into chunks of 100 and dispatch them asyncrhonously.
2. You can use `(docker prefix) security:associate {user_id} {key}` to associate a user in the KeypadUsers table with the provided key.  You will need to use Tinker to add a user at this time.
3. You can use `(docker prefix) security:dissociateKey {user_id}` to remove a users assigned key.

 At this time, there are no Admin API pages that allow the addition and removal of users or keys due to time constraints.

# Custom Functionality / other usage
The package comes with a service which can be instantiated, so that the project owner unto which the package is attached can use these methods to customise their own functionality.

The service can be instantiated thus:  `$keyService = new Vince\AcmeDoorPad\Services\KeyCodes\KeyCodeService` (do feel free to use case or alias this).
The primary method is called `generateUniqueKey()` and this will make a key that fits the required criteria of the task:
- No palindromes
- No sequences above 3
- No repitition above 3
- Must be unique

The other methods may be called at your leisure.

# Sign off 
For the bulk of the work, and to see the tests run and functionality itself, please visit the above link, or click [here.](https://github.com/Vince-C9/ACME-DoorPad)

