## When ?
- When we have two objects that are not structurally fit with each other.
- Code base directly depends on third party application.
## Why ?
- To avoid changed in the main class recursively based on the changes on the 3rd party.
- To avoid tightly coupling between two classes (avoid DIP violation)

~~~ Java 

interface Banking{
	
	int getBalance();
	
	boolean makeTransaction();
}

class IndianBankAdapter implements Banking{
	
	private IndianBanking banking;
	
	public IndianBankAdapter(){
		// tightly coupling but no other way
		this.banking = new IndianBanking();
	}
	
	@Override
	int getBalance(){
		return this.banking.findBalance();
	}
	
	@Override
	boolean makeTransaction(){
		return this.banking.doTransaction();
	}
}

class SBIBankAdapter implements Banking{
	
	private SBIBanking banking;
	
	public SBIBankAdapter(){
		// tightly coupling but no other way
		this.banking = new SBIBanking();
	}
	
	@Override
	int getBalance(){
		return this.banking.getBalance();
	}
	
	@Override
	boolean makeTransaction(){
		return this.banking.send();
	}
}
~~~

