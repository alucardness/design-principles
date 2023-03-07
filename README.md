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

<br/>
<br/>

## Don't Repeat Yourself (DRY)

<br/>

Principle that states that any piece of knowledge or logic should be written once to avoid duplication and inconsistences.

<br/>

```php
function getFullName($firstName, $lastName)
{
    return "$firstName $lastName";
}

$fullName = getFullName("Alucard", "Dracula");
```

<br/>

## You Aren't Gonna Need It (YAGNI)

<br/>

You should not add functionality until it is necessary. It helps avoid unnecessary complexity, time-consuming design and implementation of features that are not required.

<br/>

We can apply this principle by only implementing the features and functionalities that are currently needed by the application and not implementing ones that may need in the future.

<br/>

```php
class Order
{
    private $items = [];

    public function addItem($item)
    {
        $this->items[] = $item;
    }

    public function getTotal()
    {
        $total = 0;

        foreach ($this->items as $item) {
            $total += $item['price'] ?? 0;
        }

        return $total;
    }
}

$order = new Order();
$order->addItem(['name' => 'test', 'price' => 10]);
$order->addItem(['name' => 'test', 'price' => 5]);
$total = $order->getTotal();
```

<br>

## Keep It Simple Stupid (KISS)

<br/>

By adhering to the KISS principle, we can write code that is easier to maintain, debug, and understand. By avoiding unnecessary complexity, we can reduce the likelihood of bugs and make it easier for other developers to work with our code.

<b>Remember to break the logic on simple chunks.</b>

<br>

```php
$superHeroes = [
    "Superman",
    "Batman",
    "Joker"
];

$superPowers = [
    "Heat Vision", 
    "Laugh", 
    "Agility"
];

$batman = extractHero($superHeroes, "Batman");

function extractHero($array, $name)
{
    foreach ($array as $hero) {
        if ($hero === $name) {
            return $hero;
        }
    }
}
```

<br>

## Law of Demeter (LoD)

<br/>

A principle that states that an object should have limited knowledge about other objects and should communicate only with its immediate neighbors.

<br>
<b>Guidelines to follow:</b>
<ul>
    <li>Avoid chaining method calls. Chaining methods create dependencies between objects, instead try to use intermediate variables to pass data between objects.</li>
    <br>
    <li>Use dependency injection. Don't create objects directly in a class.</li>
    <br>
    <li>Use interfaces. They provide a level of abstraction that allows objects to communicate without direct knowledge of each other.</li>
</ul>

<br>

```php
class User
{
    private $name;
    private $email;
    private $address;

    public function __construct($name, $email, $address)
    {
        $this->name = $name;
        $this->email = $email;
        $this->address = $address;
    }

    public function getName()
    {
        return $this->name;
    }

    public function getEmail()
    {
        return $this->email;
    }

    public function getAddress()
    {
        return $this->address;
    }
}

class Address
{
    private $street;
    private $city;
    private $state;

    public function __construct($street, $city, $state)
    {
        $this->street = $street;
        $this->city = $city;
        $this->state = $state;
    }

    public function getFullAddress()
    {
        return $this->street . ', ' . $this->city . ', ' . $this->state;
    }
}

class UserController
{
    public function getUserInfo(User $user)
    {
        $userInfo = [
            'name' => $user->getName(),
            'email' => $user->getEmail(),
            'address' => $user->getAddress()
        ];

        return $userInfo;
    }
}

$user = new User('John Doe', 'mail@test.com', new Address('main street', 'town', 'CA'));
$controller = new UserController();
$userInfo = $controller->getUserInfo($user);
```

<br>

## Composition over Inheritance

<br/>

A principle that suggests combining simple objects into more complex ones and avoiding inheritance of properties and methods from an existing class.

<br>

```php
class Character
{
    protected $name;
    protected $alias;

    public function __construct($name, $alias)
    {
        $this->name = $name;
        $this->alias = $alias;
    }

    public function getName()
    {
        return $this->name;
    }

    public function getAlias()
    {
        return $this->alias;
    }
}

class Ability
{
    protected $name;
    protected $description;

    public function __construct($name, $description)
    {
        $this->name = $name;
        $this->description = $description;
    }

    public function getName()
    {
        return $this->name;
    }

    public function getDescription()
    {
        return $this->description;
    }
}

class Superhero
{
    protected $character;
    protected $abilities = [];

    public function __construct(Character $character, array $abilities)
    {
        $this->character = $character;
        $this->abilities = $abilities;
    }

    public function getName()
    {
        return $this->character->getName();
    }

    public function getAlias()
    {
        return $this->character->getAlias();
    }

    public function getAbilities()
    {
        $abilities = [];
        foreach ($this->abilities as $ability) {
            $abilities[] = $ability->getName();
        }

        return implode(', ', $abilities);
    }
}

$character = new Character('Bruce Wayne', 'Batman');
$abilities = [
    new Ability('Agility', 'The ability to be endurant in fights.'),
    new Ability('Rich', 'The ability to have a lot of money.'),
    new Ability('Martial Arts', 'The ability to fight in different styles.')
];
$batman = new Superhero($character, $abilities);
```

<br>

## Encapsulate What Varies

<br/>

A principle of hiding implementation details from other parts of the code, and exposing only what is necessary for external use. Make use of access modifiers: `public`, `protected` and `private`.

<br>

```php
class Superhero
{
    protected $name;
    protected $alias;
    protected $abilities;

    public function __construct($name, $alias, array $abilities)
    {
        $this->name = $name;
        $this->alias = $alias;
        $this->abilities = $abilities;
    }

    public function getName()
    {
        return $this->name;
    }

    public function getAlias()
    {
        return $this->alias;
    }

    public function getAbilities()
    {
        return implode(', ', $this->abilities);
    }
}

class KryptonianSuperhero extends Superhero
{
    protected $weakness;

    public function __construct($name, $alias, array $abilities, $weakness)
    {
        parent::__construct($name, $alias, $abilities);
        $this->weakness = $weakness;
    }

    public function getWeakness()
    {
        return $this->weakness;
    }
}

class MutantSuperhero extends Superhero
{
    protected $power;

    public function __construct($name, $alias, array $abilities, $power)
    {
        parent::__construct($name, $alias, $abilities);
        $this->power = $power;
    }

    public function getPower()
    {
        return $this->power;
    }
}

$superman = new KryptonianSuperhero(
    'Clark Kent',
    'Superman',
    ['Flight', 'Super Strength', 'Heat Vision'],
    'Kryptonite'
);

$cyclops = new MutantSuperhero(
    'Scott Summers',
    'Cyclops',
    ['Optic Blast', 'Leadership', 'Lover'],
    'Optic Blast'
);
```

<br>

## Hollywood Principle

<br>

Rather than having low-level components call high-level ones directly, high-level components should define abstract interfaces that low-level components can implement. The high level components can then call the low-level ones through these interfaces.

It's more like <b>"Don't call us, we'll call you."</b>

<br>

```php
interface Superpower
{
    public function usePower();
}

class FlightPower implements Superpower
{
    public function usePower()
    {
        echo "Flying! \n";
    }
}

class StrengthPower implements Superpower
{
    public function usePower()
    {
        echo "Lifting heavy objects! \n";
    }
}

class Superhero
{
    protected $name;
    protected $alias;
    protected $superpowers = [];

    public function __construct($name, $alias)
    {
        $this->name = $name;
        $this->alias = $alias;
    }

    public function addSuperpower(Superpower $power)
    {
        $this->superpowers[] = $power;
    }

    public function useSuperpowers()
    {
        echo $this->alias . " using superpowers: \n";
        foreach ($this->superpowers as $power) {
            $power->usePower();
        }
    }
}

$superman = new Superhero('Clark Kent', 'Superman');
$superman->addSuperpower(new FlightPower());
$superman->addSuperpower(new StrengthPower());
$superman->useSuperpowers();
```