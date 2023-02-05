## Declaring a class

```typescript
class MyClass extends BaseClass implements Interface1, Interface2
{
    // private field
    private item1: any;

    // private field
    #item2: any;

    // public field
    item3: any;

    // required field
    id: string;

    // optional field
    displayName?:boolean;

    // initialized field
    name!: string;

    // private field
    #attributes: Map<any, any>;

    // field with default value
    roles = ["foo"];

    // readonly field with default value
    readonly createdAt = new Date();

    // constructor
    constructor(id: string, email: sting) {
        super(id);
        this.email = email;
    }

    // Function
    setName(name: string) { this.name = name; }
    
    // Function with Lambda operator
    verifyName = (name: string) => { /**/ }

    // Getter
    get accountId() { /**/ }

    // Setter
    set accountId(value: string) { /**/ }

    // Private function
    private fooBar() { /**/ }

    // Public function
    protected fooBar() { /**/ }
    
    // private static field
    static #userCount = 0;

    // public static function
    static registerUser(user: User) { /**/ }

    // static initializer/constructor
    static { 
	    this.#userCount = 0;
	};
}

const myclass:MyClass = new MyClass();
```

## Generic classes

```typescript
class Foo<T> {
    value: Type

    constructor(value: Type)
    {
        this.value = value;
    }
}
```

## Parameter Properties

```typescript
class Location {
    constructor(public: x: number, public y: number) {
    }
}

const location = new Location(20, 40);
```

## Abstract Classes

```typescript
abstract class Animal {
    abstract getName(): string;

    printName() {
        console.log(this.getName());
    }
}

class Dog extends Animal {
    getName(): {
        // ...
    }
}
```

## Decorators and Attributes

```typescript
import { Syncable, triggersSync, preferCache, required } from "libraryName";

@Syncable
class User
{
    @triggersSync()
    save() {
        // ...
    }

    @preferCache(false)
    get displayName() {
        // ...
    }

    update(@required info: Partial<User>){
        // ...
    }
}
```
