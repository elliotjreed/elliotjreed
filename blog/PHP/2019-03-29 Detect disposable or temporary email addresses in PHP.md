# Detecting disposable / temporary email addresses in PHP

If you run a competition page or a voucher service for example, you might want to detect whether the email address is a "real" one and not a temporary or disposable one.

There is an excellent list curated by Martin Cech at [github.com/martenson/disposable-email-domains](https://github.com/martenson/disposable-email-domains) which I've used in a PHP package for anyone to use.

To install the [package](https://packagist.org/packages/elliotjreed/disposable-emails-filter) simply add it via [`composer`](https://getcomposer.org/download/):

```bash
composer require elliotjreed/disposable-emails-filter
```

And use it like the following:

```php
<?php
require 'vendor/autoload.php';

use ElliotJReed\DisposableEmail\Email;

if ((new Email())->isDisposable('email@temporarymailaddress.com')) {
    echo 'This is a disposable / temporary email address';
}
```

If an invalid [email address](https://www.ietf.org/rfc/rfc0822.txt) is provided then an `InvalidEmailException` is thrown, so it is advisable to check that the email address is valid first. For example:

```php
$email = 'not-a-real-email-address#example.net'

if (filter_var($email, FILTER_VALIDATE_EMAIL)) {
    if ((new Email())->isDisposable($email)) {
        echo 'This is a disposable / temporary email address';
    }

} else {
    echo 'This is not a valid email address';
}
```

The source code is available at [github.com/elliotjreed/disposable-emails-filter-php](https://github.com/elliotjreed/disposable-emails-filter-php).

Check out [github.com/martenson/disposable-email-domains](https://github.com/martenson/disposable-email-domains) for examples in other languages, including [Python](https://pypi.org/project/disposable-email-domains).
