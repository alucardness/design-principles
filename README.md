# Software Design Principles

_The following examples are written in php, but can be applied outside_

<br>

## Single Responsibility Principle (SRP)

  <br> 
    
  It states that a class should have only one reason to change. It applies by separating different concerns into separate classes or components.

  <br/>
    
````php
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

`````

<br/>

## Open/Closed Principle (OCP)

<br/>

A principle that states that a class or module should be open for extension but closed for modification. The behavior of a class can be extended without having to modify its source code.
In PHP, this principle can be applied using inheritance or interface.

<br/>

````php
abstract class Shape
{
    abstract public function area();
}

class Circle extends Shape
{
    public function area()
    {
        return pi() * pow(45, 2);
    }
}

class Rectangle extends Shape
{
    public function area()
    {
        return 2 * 2;
    }
}

`````

<br/>

## Liskov Substitution Principle (LSP)

<br/>
A principle that states objects of a superclass should be able to be replaced with objects of a subclass without affecting the correctness of the program. Subtypes must be suitable for their base types. The subclass should have the same method signatures and contracts as their base class.

<br/>

```php

interface Shape
{
    public function area();
}

class Circle implements Shape
{
    public function area()
    {
        return 2;
    }
}

class Rectangle implements Shape
{
    public function area()
    {
        return 4;
    }
}

function processShape(Shape $shape)
{
    return $shape->area();
}
```
