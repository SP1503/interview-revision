- Creating a class of huge number of objects with same properties with same values for some properties, we can use Flyweight.

```Java
class IntrinsicBullet{
	
	private int radius;
	
	private int weight;
	
	private int length;
	
	private int breadth;
	
	private String color;
	
	public IntrinsicBullet(
	int radius, 
	int weight, 
	int length, 
	int breadth, 
	String color){
		this.radius = radius;
		this.weight = weight;
		this.length = length;
		this.breadth = breadth;
		this.color = color;
	}
	
}

class ExtrinsicBullet{

	private IntrinsicBullet bullet;

	private Location target;
	
	private boolean isUsed;
	
	public ExtrinsicBullet(
	IntrinsicBullet bullet, 
	Location target, 
	boolean isUser){
		this.bullet = bullet;
		this.target = target;
		this.isUser = isUser;
	}
}

```

