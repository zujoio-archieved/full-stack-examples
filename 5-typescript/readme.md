# Javascript that scales

Typescript is a language created by Microsoft and released under an open-source (Microsoft + open source ?!?!?!?!) Apache 2.0 License. The language is foused on making the development of Javascript programs scale to many thousand lines of code. The language attacks the large-scale Javascript programming problem by offering better-design time tooling, compile-time checking and dynamic module loading at runtime.

The typescript language is a typed superset of Javascript, which is compiled to plain Javascript. This makes programs written in Typescript highly portable as they can run on almost any machine, web browser, web server and NodeJs.

Which problems does Typescript solves?
Typescript solves a lot of problems, especially in the following areas:

**Prototypal inheritance**: <br/>
Typescript solves this problem by adding classes, modules, and iterfaces. This allows programmers to transfer their existing knowledge of OOP;

**Equality and type juggling**: <br/>
Typescript introduces type checking which can provide warnings at design and compile time to pick up potential unintended juggling;

**Management of modules**: <br/>
Typescript makes module loaders to the normal way of working and allows your modules to be compiled to suit the two most prevalent module loading styles without requiring changes to your code;

**Scope**: <br/>
Typescript warning you about implicit global variabiles;

## Why use TypeScript?
Typescript is an application-scale programming language that provides early access to proposed new Javascript features and powerful additional features. 

Typescript is useful in large-scale applications that have an OOP approach, classes and interfaces can be re-used between browser and server applications. 

Typescript is becoming more and more widespread, and it’s also used by a lot of companies and frameworks

**Simple Example**
```typescript
class Telefilm {
    title : string;
    description: string;
    characters: Character[];
    
    constructor( title: string,  description: string,  characters: Character[]) {
       this.title= title;
       this.description= description;
       this.characters= characters;
    }
    
     toString() : string{
         var characterStr="";
         this.characters.forEach((cha) =>{ characterStr+= cha.toString() + "<br/>";});
         
         return "<b>"+this.title +" "+this.description+"</b><br/>" +characterStr;
    }
}

class  Character{
    name: string;
    surname: string;
    
    constructor(name: string, surname: string){
        this.name=name;
        this.surname=surname;
    }
    
    toString() : string{
        return this.name +" "+ this.surname;
    }
}

var listCharacter:Array<Character> = [
    new Character("John","Dorian"),
    new Character("Elliot","Reid"),
    new Character("Carla","Espinosa")
];

var telefilm= new Telefilm("Scrubs","Medical situations", listCharacter);

document.body.innerHTML = telefilm.toString();
```

## The problem
Design an OO parking lot using typescript language features

## The solution
### Model overview
We’re going to implement a generic parking lot ticketing system, using the following classes and interfaces:

- Ticket
- Vehicle, Car
- Parking Lot

### Ticket
**The Ticket class is used by Vehicle class:**
it describes the associations between the parked Vehicle and the entering ticket. It defines an id, entry and exit Date. The id is calculated using the combination between current time and license plate of car.

```typescript
class Ticket {
	private _id: string;
	private _enterDate: Date;
	private _exitDate: Date;

	constructor(parking_lot: string) {
		this._id = parking_lot + "-" + ((new Date()).getTime().toString());
		this._enterDate = new Date(); //GET CURRENT DATE
		this._exitDate = null;
	}

	get Id() { return this._id; }

	get EnterDate() { return this._enterDate; }
	set EnterDate(date: Date) { this._enterDate = date; }

	get ExitDate() { return this._exitDate; }
	set ExitDate(date: Date) { this._exitDate = date; }
}
```

### Vehicle
The Vehicle class contains some attributes about Vehicle dimensions, and brand and the license plate. It also contains the methods to park/exit the Vehicles from Parking lot.

The Car extends Vehicle class, and it adds additional information: car insurance. The Vehicle class can be eventually extended to add other vehicle types.

```typescript
/// <reference path="Ticket.ts"/>


class Vehicle {
	private _brand: string;
	private _height: number;
	private _weight: number;
	private _license_plate: string;

	private _isParked: boolean;
	private _ticket: Ticket;


	constructor(license_plate: string, brand: string, height: number, weight: number) {
		this._brand = brand;
		this._height = height;
		this._weight = weight;
		this._license_plate = license_plate;
	}

	parkingVehicle(): Ticket {

		if (!this._isParked) {
			var newTck = new Ticket(this._license_plate);

			this._ticket = newTck;
			this._isParked = true;
			return newTck;
		}
		return null;
	}

	get Brand() { return this._brand; }
	set Brand(brand: string) { this._brand = brand; }

	get Height() { return this._height; }
	set Height(height: number) { this._height = height; }

	get Weight() { return this._weight; }
	set Weight(weight: number) { this._weight = weight; }

	get Ticket() { return this._ticket; }

	public clearTicket(): void {
		this._ticket = null;
	}

}

/// <reference path="Vehicle.ts"/>
class Car extends Vehicle {

	private _car_insurance: string;

	constructor(license_plate: string, brand: string, height: number, weight: number, car_insurance: string) {
		super(license_plate, brand, height, weight);
		this._car_insurance = car_insurance;
	}

	get CarInsurance() { return this._car_insurance; }
	set CarInsurance(car_insurance: string) { this._car_insurance = car_insurance; }

}
```

### Parking Lot
The Parking lot is described using a combination between an interface and a class: the interface contains functions signatures, and the concrete class contains an array that is used as a Vehicles container.

ParkingLot uses custom find method that can find objects inside arrays.

```typescript
/// <reference path="..\Vehicle.ts"/>
/// <reference path="Utils.ts"/>

/*
*Interface ParkingLot
*/
interface ParkingLot<V, T> {
	parkVehicle(vehicle: V): T;
	exitVehicle(ticket: T): V;
}

/*
*Concrete class
*/
class MyParkingLot implements ParkingLot<Vehicle, Ticket>  {

	private _address: string;

	private _vehicles: Array<Vehicle>;

	constructor(address: string) {
		this._address = address;
		this._vehicles = new Array<Vehicle>();
	}


	get Vehicles() {
		return this._vehicles;
	}

	get Address() {
		return this._address;
	}

	parkVehicle(vehicle: Vehicle): Ticket {
		var tck = vehicle.parkingVehicle();
		if (tck == null) { return null; }

		this._vehicles.push(vehicle);
		return tck;
	}

	exitVehicle(ticket: Ticket): Vehicle {
		ticket.ExitDate = new Date();
		var target = this._vehicles.find(tmp=> typeof tmp !== null && tmp.Ticket.Id == ticket.Id);
		var index = this._vehicles.indexOf(target);

		if (index > -1) { this._vehicles.splice(index, 1); }
		if (target == null) { return null; }

		return target;
	}
}

/*
*Custom ARRAY INTERFACE
*/
interface Array<T> {
	find(predicate: (T) => boolean): T;
}

/*
* Custom array finder by Lambda
*/
if (!Array.prototype.find) {
	Array.prototype.find = function(predicate) {
		if (this == null) {
			throw new TypeError('Array.prototype.find called on null or undefined');
		}
		if (typeof predicate !== 'function') {
			throw new TypeError('predicate must be a function');
		}
		var list = Object(this);
		var length = list.length >>> 0;
		var thisArg = arguments[1];
		var value;

		for (var i = 0; i < length; i++) {
			value = list[i];
			if (predicate.call(thisArg, value, i, list)) {
				return value;
			}
		}
		return undefined;

	};
}
```

Reference List
===
1. [introducing-typescript](https://samueleresca.net/2015/11/introducing-typescript/)
2. [introducing-typescript-language-features](https://samueleresca.net/2015/11/introducing-typescript-language-features/)