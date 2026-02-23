``` Java
public class Trip {
    private String id;
    private User rider;
    private Driver driver;
    private Location pickupLocation;
    private Location dropoffLocation;
    private TripState state;
    private double fare;
    
    public Trip(String id, User rider, Location pickup, Location dropoff) {
        this.id = id;
        this.rider = rider;
        this.pickupLocation = pickup;
        this.dropoffLocation = dropoff;
        this.state = TripState.REQUESTED;
    }
    
    public void assignDriver(Driver driver) {
        if (this.state != TripState.REQUESTED) {
            throw new IllegalStateException("Cannot assign driver to trip in state: " + state);
        }
        this.driver = driver;
        this.state = TripState.DRIVER_ASSIGNED;
    }
    
    public void startTrip() {
        if (this.state != TripState.DRIVER_ASSIGNED) {
            throw new IllegalStateException("Cannot start trip in state: " + state);
        }
        this.state = TripState.IN_PROGRESS;
    }
    
    public void completeTrip(PricingCalculator calculator) {
        if (this.state != TripState.IN_PROGRESS) {
            throw new IllegalStateException("Cannot complete trip in state: " + state);
        }
        this.fare = calculator.calculateFare(this);
        this.state = TripState.COMPLETED;
    }
    
    public void cancelTrip() {
        if (this.state == TripState.COMPLETED) {
            throw new IllegalStateException("Cannot cancel completed trip");
        }
        this.state = TripState.CANCELLED;
    }
    
    // Getters
    public String getId() { return id; }
    public TripState getState() { return state; }
    public double getFare() { return fare; }
    public Location getPickupLocation() { return pickupLocation; }
    public Location getDropoffLocation() { return dropoffLocation; }
}
```

``` Java
public enum TripState {
    REQUESTED,
    DRIVER_ASSIGNED,
    IN_PROGRESS,
    COMPLETED,
    CANCELLED
}
```

``` Java

public interface PricingCalculator {
    double calculateFare(Trip trip);
}

public class StandardPricingCalculator implements PricingCalculator {
    private static final double BASE_FARE = 2.50;
    private static final double COST_PER_MILE = 1.50;
    private static final double COST_PER_MINUTE = 0.25;
    
    @Override
    public double calculateFare(Trip trip) {
        double distance = calculateDistance(
            trip.getPickupLocation(), 
            trip.getDropoffLocation()
        );
        double duration = estimateDuration(distance);
        
        return BASE_FARE + 
               (distance * COST_PER_MILE) + 
               (duration * COST_PER_MINUTE);
    }
    
    private double calculateDistance(Location from, Location to) {
        // Simplified distance calculation
        double latDiff = to.getLatitude() - from.getLatitude();
        double lonDiff = to.getLongitude() - from.getLongitude();
        return Math.sqrt(latDiff * latDiff + lonDiff * lonDiff) * 69; // rough miles
    }
    
    private double estimateDuration(double distanceMiles) {
        // Assume average speed of 30 mph
        return (distanceMiles / 30.0) * 60; // minutes
    }
}
```

``` Java
public class Location {
    private double latitude;
    private double longitude;
    
    public Location(double latitude, double longitude) {
        this.latitude = latitude;
        this.longitude = longitude;
    }
    
    public double getLatitude() { return latitude; }
    public double getLongitude() { return longitude; }
}
```
