## When ?
- When we need to. execute some logic based on the given input string in command line.
## Why ?
- Abstraction and the code implementation is easy.

~~~ Java 

interface Command{
	
	boolean matches();
	
	void execute();
}

class CommandExecutor{
	
	List<Command> commands = null;
	
	public CommandExecutor(){
		this.commands = new ArrayList<>();
	}
	
	public void addCommand(Command command){
		this.commands.add(command);
	}
	
	public void removeCommand(Command command){
		for(Command currCommand : commands)
			if(currCommand.equals(command))
				this.commands.remove(currcommand);
	};
	
	public void execute(String input){
		for(Command curr : commands){
			if(curr.matches(input)) curr.execute(input);
		}
	}
}

~~~
