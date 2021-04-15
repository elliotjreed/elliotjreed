# Using Symfony's Simple PHPUnit with PhpStorm

If you use JetBrains' PhpStorm IDE and you're developing with Symony, you'll likely want to be able to run your tests from within PhpStorm using the `simple-phpunit` command from the `symfony/phpunit-bridge` component.

It's not pbvious how to do this as the `simple-php` command doesn't get detected by PhpStorm using the `vendor/autoload` option for selecting PHPUnit.

It's a bit hidden, but you will need to go to `file > settings > Languages & Frameworks > PHP > Test Frameworks` and add or edit (`+`) a `PHPUnit Local` configuration.

Then select `Path to phpunit.phar` and enter the following relevant to your project directory: `/your-project-directory/vendor/bin/.phpunit/phpunit-6.5/phpunit`.

The actual PHPUnit script is in the hidden directory `vendor/bin/.phpunit/phpunit-6.5`.

![Screenshot of Symfony PHPUnit settings for PhpStorm](https://res.cloudinary.com/elliotjreed/image/upload/f_auto,q_auto/v1553714408/blog/simple-phpunit-symfony-4.png "Screenshot of Symfony PHPUnit settings for PhpStorm")
