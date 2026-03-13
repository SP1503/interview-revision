- Helps to use different algorithms that is there for a specific single task and which algorithms needs to be used is decided in run time.
- Different Algorithms to find path and which algorithm to use is defined by client in runtime.

```Java

private interface PathFinder{
	Path findPath(Location source, Location destination);
}

private class WalkingPathFinder implements PathFinder{
	@Override
	Path findPath(Location source, Location destination){
		System.out.println("Finding path for walking");
	}
}

private class BikePathFinder implements PathFinder{
	@Override
	Path findPath(Location source, Location destination){
		System.out.println("Finding path for Bike");
	}
}

private class CarPathFinder implements PathFinder{
	@Override
	Path findPath(Location source, Location destination){
		System.out.println("Finding path for Car");
	}
}

private class TrainPathFinder implements PathFinder{
	@Override
	Path findPath(Location source, Location destination){
		System.out.println("Finding path for Train");
	}
}

```


