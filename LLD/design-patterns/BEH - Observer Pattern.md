- When a change of state in the publisher needs to be notified to multiple subscribers we need to use observer design pattern.

```Java
import java.util.concurrent.CopyOnWriteArrayList;

public abstract class Publisher{
	protected List<Subscriber> subscribers = new CopyOnWriteArrayList<>();
	
	abstract void add(Subscriber subscriber);
	
	abstract void remove(Subscriber subscriber);
	
	abstract void notifySubsribers(String message);
	
}

public interface Subscriber{
	void notify(String message);
}

class ZomatoOrderService extends Publisher{
	
	@Override
	public void add(Subscriber subscriber){
		subscribers.add(subscriber);
	}
	
	@Override
	public void remove(Subscriber reqSubscriber){
		subscribers.remove(reqSubscriber);
	}
	
	@Override
	public void notifySubsribers(String message){
		for(Subscriber subscriber: subscribers){
			subscriber.notify(message);
		}
	}
}

class Customer implements Subscriber {

    @Override
    public void notify(String message) {
        System.out.println("Customer Notification: " + message);
    }
}

class DeliveryBoy implements Subscriber {

    @Override
    public void notify(String message) {
        System.out.println("DeliveryBoy Notification: " + message);
    }
}

class ZomatoServer implements Subscriber {

    @Override
    public void notify(String message) {
        System.out.println("ZomatoServer Notification: " + message);
    }
}

```


