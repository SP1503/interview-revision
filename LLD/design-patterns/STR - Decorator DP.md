- Adding a wrapper to existing object to enhance its capability without modifying it.

```Java
public interface Beverage{
	String getDescription();
	int getCost();
}

public class DarkRoast implements Beverage{
	@Override
	String getDescription(){
	
	}
	@Override
	int getCost(){
	}
}

public class Espresso implements Beverage{
	@Override
	String getDescription(){
	
	}
	@Override
	int getCost(){
	}
}

public abstract class AddOn implements Beverage{

	private Beverage beverage;
	
	public AddOns(Beverage beverage){
		this.beverage = beverage;
	}
	
}

public class Milk extends AddOn{
	
	public Milk(Beverage beverage){
		super(beverage);
	}
	
	@Override
	public String getDescription(){
		return this.beverage.getDescription() + "added with Milk."
	}
	@Override
	public int getCost(){
		return this.beverage.getCost() + 2;
	}
}

public class Mocha extends AddOn{
	
	public Mocha(Beverage beverage){
		super(beverage);
	}
	
	@Override
	public String getDescription(){
		return this.beverage.getDescription() + "added with Mocha."
	}
	@Override
	public int getCost(){
		return this.beverage.getCost() + 5;
	}
}
```



