# DTO-in-laravel

Data Transfer Objects help in "structuring" the data coming from different types of requests ( Http request, Webhook request etc. ) into single form which we can use in various parts of our application. With DTOs, we have confidence that we will not get unexpected data in our application logic.

```
NOTE: Below code is valid only for PHP 7.4
```

```php
class CheckoutData extends DataTransferObject
{

   public int $checkout_id;

   public Carbon $completed_at;


   public static function fromRequest(Request $request){ ... }

   public static function fromWebhook(array $params)

   {

      return new self([
        'checkout_id' => $params['id'],
        'completed_at' => $params['completed_at']
      ]);

   }

}
```

```php

abstract class DataTransferObject
{

    public function __construct(array $parameters = [])
    {
        $class = new ReflectionClass(static::class);

        foreach ($class->getProperties(ReflectionProperty::IS_PUBLIC) as $reflectionProperty){
            $property = $reflectionProperty->getName();
            $this->{$property} = $parameters[$property];
        }
    }

}

```

This is how we can easily construct our Data Transfer Objects with PHP7.4.

