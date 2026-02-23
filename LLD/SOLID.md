```Java
class Bird{
	
	public Bird(String type){
		this.type = type;
	}
	
	private String type;
	
	void makeSound(){
		if(type.equals("Eagle")){
			makeEagleSound();
		}
		else if(type.equals("penguine")){
			makePenquineSound();
		}
		else makeNormalSound();
	}
	
	void fly(){
		if(type.equals("Eagle")){
			makeEagleFly();
		}
		else if(type.equals("penguin")){
			makePenquineFly();
		}
		else makeNormalFly();
	}
}

```


### Problems in the above code
- Bird class having more responsibility as Eagle, Penguin etc - violated ==single responsibility== - Every class should have a single reason to change.
- The same class handles different behaviours for different type of bird - violates ==*open for extension and closed for modification*== The implemented code should be extensible and should not be modify able.
## Solution for not violating SRP and OCP

```Java
abstract class Bird{
	abstract void makeSound();
	abstract void fly();
}

class Eagle extends Bird{
	
	@override
	void makeSound(){
		System.out.println("Make Eagle Sound");
	}
	
	@override
	void fly(){
		System.out.println("Make Eagle fly high");
	}
}

class Sparrow extends Bird{
	
	@override
	void makeSound(){
		System.out.println("Make Sparrow Sound");
	}
	
	@override
	void fly(){
		System.out.println("Make Sparrow fly low");
	}
}

class Penguin extends Bird{
	
	@override
	void makeSound(){
		System.out.println("Make Penguin Sound");
	}
	
	@override
	void fly(){
		throw RuntimeException("Penguine won't fly");
	}
}
```
## Problems with above code
- Every bird will not fly, Calling fly method on penguin object will throw exception or will introduce surprise to the user - violates ==Liskov's substitution principle== - All the methods implemented by the parent class should be supported by the child classes and should not introduce surprises.
## Solution for LSP violation

```Java
abstract class Bird{
	abstract void makeSound();
}

interface FlyableBehaviour{
	void fly();
}

class Eagle extends Bird implements FlyableBehaviour{
	
	@override
	void makeSound(){
		System.out.println("Make Eagle Sound");
	}
	
	@override
	void fly(){
		System.out.println("Make Eagle fly high");
	}
}

class Sparrow extends Bird implements FlyableBehaviour{
	
	@override
	void makeSound(){
		System.out.println("Make Sparrow Sound");
	}
	
	@override
	void fly(){
		System.out.println("Make Sparrow fly low");
	}
}

class Penguin extends Bird{
	
	@override
	void makeSound(){
		System.out.println("Make Penguin Sound");
	}
}
```

## Problem
- What if all the flyable birds should have danceable behaviour as well ?
	- We can have a dance method in the interface flyable.
	- But is that violates SRP.
	- SRP - handles fly as well as dance, No single responsibility.
- Tomorrow the requirement changes that says there are birds that only fly and cannot dance
	- Now the changes to decouple dance behaviour from flyable interface is more instead make the interface always light weighted.
- ==Interface Segregation principle - All the interfaces that implemented should be light weighted==
## Solution for ISP violation

```Java
abstract class Bird{
	abstract void makeSound();
}

interface FlyableBehaviour{
	void fly();
}

interface DancableBehaviour{
	void dance();
}

class Eagle extends Bird implements FlyableBehaviour{
	
	@override
	void makeSound(){
		System.out.println("Make Eagle Sound");
	}
	
	@override
	void fly(){
		System.out.println("Make Eagle fly high");
	}
}

class Sparrow extends Bird implements FlyableBehaviour, {
	
	@override
	void makeSound(){
		System.out.println("Make Sparrow Sound");
	}
	
	@override
	void fly(){
		System.out.println("Make Sparrow fly low");
	}
	
	@override
	void dance(){
		System.out.println("Make Sparrow dance");
	}
}

class Penguin extends Bird implements DancableBehaviour{
	
	@override
	void makeSound(){
		System.out.println("Make Penguin Sound");
	}
	
	@override
	void dance(){
		System.out.println("Make Penguin dance");
	}
}
```

## Problem
- Eagle will fly high and Sparrow will fly low. 
- The implementation gets duplicated where ever required.

```Java
abstract class Bird{
	abstract void makeSound();
}

interface Flyable{
	void fly();
}

class FlyHigh {
	
	@Override
	void flyHigh(){
		System.out.println("Make bird fly high");
	}
	
}

class FlyLow {
	
	@Override
	void flyLow(){
		System.out.println("Make bird fly low");
	}
	
}

interface DancableBehaviour{
	void dance();
}

class Eagle extends Bird implements Flyable{

	private FlyableBehaviour flyHigh = new FlyHigh();
	
	@override
	void makeSound(){
		System.out.println("Make Eagle Sound");
	}
	
	@override
	void fly(){
		flyHigh.flyHigh();
	}
}

class Sparrow extends Bird implements Flyable{

	private FlyableBehaviour flyLow = new FlyLow();
	
	@override
	void makeSound(){
		System.out.println("Make Sparrow Sound");
	}
	
	@override
	void fly(){
		flyLow.flyLow();
	}
	
	@override
	void dance(){
		System.out.println("Make Sparrow dance");
	}
}

class Penguin extends Bird implements DancableBehaviour{
	
	@override
	void makeSound(){
		System.out.println("Make Penguin Sound");
	}
	
	@override
	void dance(){
		System.out.println("Make Penguin dance");
	}
}
```

## Problem
- The Eagle and Sparrow gets tightly coupled with FlyHigh and FlyLow class.
- This is Dependency inversion violation. No two concrete classes should be tightly coupled with each other. if needed both can be coupled through interfaces.

## Solution for DIP violation

```Java

abstract class Bird{
	abstract void makeSound();
}

interface Flyable{
	void fly();
}

interface DancableBehaviour{
	void dance();
}

class Eagle extends Bird implements Flyable{

	private FlyableBehaviour flyHigh = new FlyHigh();
	
	@override
	void makeSound(){
		System.out.println("Make Eagle Sound");
	}
	
	@override
	void fly(){
		flyHigh.fly();
	}
}

class Sparrow extends Bird implements Flyable{

	private FlyableBehaviour flyLow = new FlyLow();
	
	@override
	void makeSound(){
		System.out.println("Make Sparrow Sound");
	}
	
	@override
	void fly(){
		flyLow.fly();
	}
	
	@override
	void dance(){
		System.out.println("Make Sparrow dance");
	}
}

class Penguin extends Bird implements DancableBehaviour{
	
	@override
	void makeSound(){
		System.out.println("Make Penguin Sound");
	}
	
	@override
	void dance(){
		System.out.println("Make Penguin dance");
	}
}

// to have different fly behvaiour and avoid code duplication
interface FlyableBehaviour{
	void fly();
}

class FlyHigh implements FlyableBehaviour{
	
	@Override
	void fly(){
		System.out.println("Make bird fly high");
	}
	
}

class FlyLow implements FlyableBehaviour{
	
	@Override
	void fly(){
		System.out.println("Make bird fly low");
	}
	
}
```

