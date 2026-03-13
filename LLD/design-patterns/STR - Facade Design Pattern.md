## When ?
- Hide the complex logic after a simple interface that gives steps as methods in it.
## Why ?
- Client will only know the abstract process and not the complex implementation behind that.

~~~ Java 

interface OrderProcessing{
	
	boolean checkInventory();
	
	boolean processPayment();
	
	boolean generateInvoice();
}

class AmazonOrderProcessing implements OrderProcessing{
	
	private  NotificationClient notCLient;
	private PaymentClient payClient;
	private warehouseClient warClient;
	
	@Override
	boolean checkInventory(){
		warClient.checkAvailability();
		warClient.updateInventory();
		watClient.updateSeller();
	}
	
	@Override
	boolean processPayment(){
		payClient.getPayment();
		payClient.processInvoice();
		payClient.sendInvoice();
	}
	
	@Override
	boolean generateInvoice(){}
}


~~~
