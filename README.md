# Software Design Principles

_The following examples are written in php, but can be applied outside_

<br>

## Single Responsibility Principle

  <br> 
    
  It states that a class should have only one reason to change. It applies by separating different concerns into separate classes or components.

  <br/>
    
```php
    class Order
    {
        public function createOrder(array $data)
        {
          // Create order logic

          // Send email
          $mailer = new Mailer();
          $mailer->send($data)
        }
    }

    class Mailer()
    {
      public function send(array $data)
      {
          // Send email
      }
    }

```

```
