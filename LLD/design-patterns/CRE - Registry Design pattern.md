
## Why ?
- We don't need to create a object in classic way and need to set values in the classic way.
- Provides reusability of code and object that acts as a template.

## When ?
- When we know in future my current object that can act as template for the future creation object.

~~~ Java 

interface Registry<T, K>{
	
	void addPrototye(T template);
	
	T getPrototype(K type);
	
	T clone(K type);
}

class UserRegistry implements Registry<User, UserType>{
	
	Map<UserType, User> templates = new HashMap<>();
	
	@Override
	void addPrototye(User template){
		this.template.add(template.getUserType(), template);
	}
	
	@Override
	User getPrototype(K type){
		return this.template.get(type);
	}
	
	@Override
	User clone(K type){
		return this.template.get(type).clone();
	}
}
~~~

