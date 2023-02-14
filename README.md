# Software Design Principles

_The following examples are written in php, but can be applied outside_

<br>

## Single Responsibility Principle (SRP)

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

<br/>

## Open/Closed Principle (OCP)

<br/>

A principle that states that a class or module should be open for extension but closed for modification. The behavior of a class can be extended without having to modify its source code.
In PHP, this principle can be applied using inheritance or interface.

<br/>

```php
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

```

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

<br/>

## Interface Segregation Principle (ISP)

A principle that states clients should not be forced to depend on interfaces they do not use. A class should not be forced to implement interfaces it does not use. Use smaller interfaces instead of a single, large one.

<br/>

```php
interface Flyable
{
    public function fly();
}

interface LaserEye
{
    public function blast();
}

class Superman implements Flyable, LaserEye
{
    public function fly()
    {
        return 'Superman is flying.';
    }

    public function blast()
    {
        return 'Superman is using laser eyes';
    }
}

class Aquaman implements Flyable
{
    public function fly()
    {
        return 'Aquaman is flying.';
    }
}
```

<br/>

## Dependency Inversion Principle (DIP)

<br/>

It states that high-level modules should not depend on low-level modules, but both should depend on abstractions. The implementation details of a class should be abstracted and defined in separate, interchangeable modules.

<br/>

```php
interface Hero
{
    public function attack();
}

class Superman implements Hero
{
    public function attack()
    {
        return 'Superman is using heat vision';
    }
}

class Batman implements Hero
{
    public function attack()
    {
        return 'Batman is using batarang';
    }
}

class HeroAttack
{
    protected $hero;

    public function __construct(Hero $hero)
    {
        $this->hero = $hero;
    }

    public function attack()
    {
        return $this->hero->attack();
    }
}

$attack = new HeroAttack(new Superman());
```

### **All of those five principles are called SOLID**