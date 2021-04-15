# Running Code Coverage for PHPUnit in Symfony

Finding out how much of your code is covered by your unit tests is important in identifying potentially weak areas of your codebase.

If you are using [`phpunit`](https://phpunit.de/) to run your PHP unit tests, you can add code coverage rather easily.

First you'll need to edit your `phpunit.xml` or `phpunit.xml.dist` file to include the following:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://schema.phpunit.de/6.5/phpunit.xsd" backupGlobals="false" colors="true"
         bootstrap="config/bootstrap.php">

    <testsuites>
        <testsuite name="Project Test Suite">
            <directory>./tests</directory>
        </testsuite>
    </testsuites>

    <filter>
        <whitelist processUncoveredFilesFromWhitelist="true">
            <directory>./src</directory>
            <exclude>
                <file>./src/Kernel.php</file>
            </exclude>
        </whitelist>
    </filter>

    <listeners>
        <listener class="Symfony\Bridge\PhpUnit\SymfonyTestsListener"/>
    </listeners>
</phpunit>

```

If you already have `phpunit.xml` or `phpunit.xml.dist` file the part you need to add from above is:

```xml
<whitelist processUncoveredFilesFromWhitelist="true">
    <directory>./src</directory>
    <exclude>
        <file>./src/Kernel.php</file>
    </exclude>
</whitelist>
```

Here we are excluding the `Kernel.php` from our code coverage reporting as that is tested by the Symfony framework itself.

To run the tests based on this configuration using standard PHPUnit run:

```bash
vendor/bin/phpunit -c phpunit.xml --coverage-html ./coverage --coverage-text
```

If you are using Symfony's `test-pack` or `simple-phpunit` (`phpunit-bridge`), instead run:

```bash
vendor/bin/simple-phpunit -c phpunit.xml --coverage-html ./coverage --coverage-text
```

This will generate an HTML report in the coverage directory in your project.

You'll probably want to add `/coverage` to your `.gitignore` if you are using `git` so you don't add it to your project sources.
