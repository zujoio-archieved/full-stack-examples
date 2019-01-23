# Design Patterns Basics
---
> A design pattern is a term used in software engineering for a general reusable solution to a commonly occurring problem in software design.


## SOLID Principles

SOLID is an acronym for the first five object-oriented design(OOD) principles by Robert C. Martin, popularly known as @UncleBob.

---

SOLID stands for:
- **S** - Single-responsiblity principle
- **O** - Open-closed principle
- **L** - Liskov substitution principle
- **I** - Interface segregation principle
- **D** - Dependency Inversion Principle

---

### Single-responsiblity principle
S.R.P for short - this principle states that:

>A class should have one and only one reason to change, meaning that a class should have only one job.

**Example: Wrong Way**

The class Task defines properties related to the model, but it also defines the data access method to save the entity on a generic data source:

```typescript
/*
* THE  CLASS DOESN'T FOLLOW THE SRP PRINCIPLE
*/
class Task {
    private db: Database;

    constructor(private title: string, private deadline: Date) {
        this.db = Database.connect("admin:password@fakedb", ["tasks"]);
    }

    getTitle() {
        return this.title + "(" + this.deadline + ")";
    }
    save() {
        this.db.tasks.save({ title: this.title, date: this.deadline });
    }
}
```
**Example: Right Way**
The Task class can be divided between Task class, that takes care of model description and TaskRepository that is responsabile for storing the data.

```typescript
class Task {

    constructor(private title: string, private deadline: Date) {
    }

    getTitle() {
        return this.title + "(" + this.deadline + ")";
    }


}


class TaskRepository {
    private db: Database;

    constructor() {
        this.db = Database.connect("admin:password@fakedb", ["tasks"]);
    }

    save(task: Task) {
        this.db.tasks.save(JSON.stringify(task));
    }
}
```

### The Open-closed Princple (OCP)
>Software entities should be open for extension but closed for modification.

The risk of changing an existing class is that you will introduce  an inadvertent change in behaviour. The solution is create another class that overrides the behaviour of  the original class. By following the OCP, a component is more likely to contain maintainable and re-usable code.

Example: Right Way
The CreditCard class describes a method to calculate the monthlyDiscount(). The monthlyDiscount() depends on the type of Card, which can be : Silver or Gold. To change the monthly discount calc, you should create another class which overrides the monthlyDiscount() Method.

The solution is to create two new classes: one for each type of card.

```typescript
class CreditCard {
    private Code: String;
    private Expiration: Date;
    protected MonthlyCost: number;

    constructor(code: String, Expiration: Date, MonthlyCost: number) {
        this.Code = code;
        this.Expiration = Expiration;
        this.MonthlyCost = MonthlyCost;
    }

    getCode(): String {
        return this.Code;
    }

    getExpiration(): Date {
        return this.Expiration;
    }

    monthlyDiscount(): number {
        return this.MonthlyCost * 0.02;
    }

}

class GoldCreditCard extends CreditCard {

    monthlyDiscount(): number {
        return this.MonthlyCost * 0.05;
    }
}


class SilverCreditCard extends CreditCard {

    monthlyDiscount(): number {
        return this.MonthlyCost * 0.03;
    }
}
```

### The Liskov Substitution Principle (LSP)
> Child classes should never break the parent class’ type definitions.

The concept of this principle was introduced by Barbara Liskov in a 1987 conference keynote and later published in a paper together with Jannette Wing in 1994.

As simple as that, a subclass should override the parent class methods in a way that does not break functionality from a client’s point of view.

Example:
In the following example ItalyPostalAddress, UKPostalAddress and USAPostalAddress extend one common class: PostalAddress.

The AddressWriter class refers PostalAddress: the writer parameter can be of three different sub-types.

```typescript
abstract class PostalAddress {
    Addressee: string;
    Country: string
    PostalCode: string;
    City: string;
    Street: string
    House: number;

    /*
    * @returns Formatted full address
    */
    abstract WriteAddress(): string;
}

class ItalyPostalAddress extends PostalAddress {
    WriteAddress(): string {
        return "Formatted Address Italy" + this.City;
    }
}
class UKPostalAddress extends PostalAddress {
    WriteAddress(): string {
        return "Formatted Address UK" + this.City;
    }
}
class USAPostalAddress extends PostalAddress {
    WriteAddress(): string {
        return "Formatted Address USA" + this.City;
    }
}


class AddressWriter {
    PrintPostalAddress(writer: PostalAddress): string {
        return writer.WriteAddress();
    }
}
```

### The Interface Segregation Principle (ISP)
It is quite common to find that an interface is in essence just a description of an entire class. The ISP states that we should write a series of smaller and more specific interfaces that are implemented by the class. Each interface provides an single behavior.

**Example – wrong way**
The following Printer interface makes it impossible to implement a printer that can print and copy, but not staple:

```typescript
interface Printer {
    copyDocument();
    printDocument(document: Document);
    stapleDocument(document: Document, tray: Number);
}


class SimplePrinter implements Printer {

    public copyDocument() {
        //...
    }

    public printDocument(document: Document) {
        //...
    }

    public stapleDocument(document: Document, tray: Number) {
        //...
    }

}
```
**Example – right way**
The following example shows an alternative approach that groups methods into more specific interfaces. It describe a number of contracts that could be implemented individually by a simple printer or simple copier or by a super printer:

```typescript
interface Printer {
    printDocument(document: Document);
}


interface Stapler {
    stapleDocument(document: Document, tray: number);
}


interface Copier {
    copyDocument();
}

class SimplePrinter implements Printer {
    public printDocument(document: Document) {
        //...
    }
}


class SuperPrinter implements Printer, Stapler, Copier {
    public copyDocument() {
        //...
    }

    public printDocument(document: Document) {
        //...
    }

    public stapleDocument(document: Document, tray: number) {
        //...
    }
}
```

### The Dependency inversion principle (DIP)
The DIP simply states that high-level classes shouldn’t depend on  low-level components, but instead depend on an abstraction.

Example – wrong way
The high-level WindowSwitch depends on the lower-level CarWindow class:

```typescript
class CarWindow {
    open() {
        //... 
    }

    close() {
        //...
    }
}


class WindowSwitch {
    private isOn = false;

    constructor(private window: CarWindow) {

    }

    onPress() {
        if (this.isOn) {
            this.window.close();
            this.isOn = false;
        } else {
            this.window.open();
            this.isOn = true;
        }
    }
}
```

**Example - right way**
To follow the DIP, the class WindowSwitch should references an interface (IWindow) that is implemented by the object CarWindow:

```typescript
interface IWindow {
    open();
    close();
}

class CarWindow implements IWindow {
    open() {
        //...
    }

    close() {
        //...
    }
}


class WindowSwitch {
    private isOn = false;

    constructor(private window: IWindow) {

    }

    onPress() {
        if (this.isOn) {
            this.window.close();
            this.isOn = false;
        } else {
            this.window.open();
            this.isOn = true;
        }
    }
}
```

Reference List:
---
1. [solid-principles-using-typescript](https://samueleresca.net/2016/08/solid-principles-using-typescript/)
2. [solid-javascript-single-responsibility-principle](http://aspiringcraftsman.com/2011/12/08/solid-javascript-single-responsibility-principle/)
3. []