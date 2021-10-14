# Trigger Handler Interface

## Which Trigger Handler Interface?
We will use the following Trigger interface for our projects:
- https://github.com/kevinohara80/sfdc-trigger-framework/tree/master/src/classes

<br>

## Naming Convention
If the Object is called `Example`, we will use the class name `ExampleTriggerHandler`.

<br>

## Style
```java
public with sharing class ExampleTriggerHandler extends TriggerHandler {

	private Map<Id, Example__c> newMap;
	private Map<Id, Example__c> oldMap;
	private List<Example__c> news;
	private List<Example__c> olds;

	public ExampleTriggerHandler() {
		this.newMap 	= (Map<Id, Example__c>) Trigger.newMap;
		this.oldMap 	= (Map<Id, Example__c>) Trigger.oldMap;
		this.news		= (List<Example__c>) Trigger.new;
		this.olds		= (List<Example__c>) Trigger.old;
	}

	public static void yourMethod() {
		// your code here
	}
}
```

## Accessing the Lists
e.g. Accounts
```java
public static void yourMethod() {
	for (Account account : this.news) {
		// your code here
	}

	for (Account account : this.olds) {
		// your code here
	}
}
```

## Accessing Maps
e.g. Accounts
```java
public static void yourMethod() {
	// Get unique set of Account IDs
	Set<Id> uniqueNewAccountIds = this.newMap.keySet();

	// Iterate over list of Accounts from the New Map
	for (Account account : this.newMap.values()) {
		
		// Checking existence of an Account Id from the Old Map
		if (this.oldMap.containsKey(account.Id))) {

			// Accessing a value from the New Map
			String someField1 = this.newMap.get(account.Id).Some_Field1__c;

			// Accessing a value from the Old Map
			String someField2 = this.oldMap.get(account.Id).Some_Field2__c;
		}
	}
}
```