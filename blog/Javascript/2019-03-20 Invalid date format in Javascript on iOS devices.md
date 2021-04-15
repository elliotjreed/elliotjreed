# Fixing invalid date format on Safari / iOS devices using Javascript

If you construct Javascript's `Date` object with a format such as `2019-03-20 11:30:00` you'll get an error in Apple's Safari browser on iOS devices.

Other broswers have their own implementation and accept this format, but for Safari you'll need to replace the space with a `T` to conform to a simplified version of [ISO-8601](https://ecma-international.org/ecma-262/5.1/#sec-15.9.1.15).

So the date would now look like:

```javascript
new Date("2019-03-20T11:30:00")
```

If you're injecting the `Date` with a string from PHP you can use the following to work cross-browser which will also be more human-readable:

```php
<script>
  new Date("<?php echo (new DateTime())->format('D M j G:i:s'); ?>")
</script>
```
