# Trigger Handler Interface

We use the following Trigger interface with our projects:
- https://github.com/kevinohara80/sfdc-trigger-framework/tree/master/src/classes


## Naming Convention
If the Object is called `Example`, we will use the class name `ExampleTriggerHandler`.


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
}
```