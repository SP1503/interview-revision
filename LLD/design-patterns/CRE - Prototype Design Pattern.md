## When ?
- When we know in future we have a possibility to create a object of same class with same values but it should but deep copy.

## Why ?
- This helps to create deep copy of classes without client knows its implementation.
- The author only writing their own copy logic for the client.

~~~ java

interface Clonable<T>{
	T clone(T other);
}

class User implements Clonable<User>{
	
	private String name;
	private String userName;
	private String password;
	
	public User(String name, String userName, String password){
		this.name = name;
		this.userName = userName;
		this.password = password;
	}
	
	User clone(User other){
		return new User(this.name, this.userName, this.password);
	}
}
~~~

