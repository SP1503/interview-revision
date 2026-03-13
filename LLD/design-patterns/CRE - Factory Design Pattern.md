## When ?
- Pattern can be used where we need to create object whose steps to create are same but each steps having different flavours or logic to do based on the run time input.

## Why ?
- Helps to avoid OCP violation.
- Helps to reduce code duplication where we need to check type repeatedly and process the current step based on the type.
- Helps to avoid SRP violation.
	- Factory class helps to create a object of different child which have same steps with different implementations.

~~~ Java 

#Simple Factory Design Pattern

#Factory for a single product
interface Seat{
	
	bulidSeatCusion();
	
	buildHandles();
	
	buildHeadRest();
	
	buildBack();
}

class RacingSeat implements Seat{}

class LuxurySeat implements Seat{}

class LeatherSeat implements Seat{}

public class SeatFactory{
	
	public static Seat createSeat(SeatType seatType){
		return switch(seatType){
			case RACING -> new RacingSeat();
			case LUXURY -> new LuxurySeat();
			default -> leatherSeat(); 
		}
	}
}

# Abstract Factory Method

# Factory for a package that gives different product based on package type.
interface CarSetup{
	
	void createWheels();
	
	void createBody();
	
	void createInterior();
	
	void createExterior();
}

class LowEnd implements CarSetup{

	@Override
	void createWheels(){}
	
	@Override
	void createBody(){}
	
	@Override
	void createInterior(){}
	
	@Override
	void createExterior(){}

}

class MiddleEnd implements CarSetup{

	@Override
	void createWheels(){}
	
	@Override
	void createBody(){}
	
	@Override
	void createInterior(){}
	
	@Override
	void createExterior(){}

}

class TopEnd implements CarSetup{

	@Override
	void createWheels(){}
	
	@Override
	void createBody(){}
	
	@Override
	void createInterior(){}
	
	@Override
	void createExterior(){}

}

public class CarSetupFactory{
	
	public static CarSetup(CarType type){
		return switch(type){
			case TOP_END -> return new TopEnd();
			case MIDDLE_END -> return new MiddleEnd();
			default return new LowEnd();
		}
	}
}
~~~

