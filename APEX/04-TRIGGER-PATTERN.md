# Trigger Pattern


## Custom Metadata Toggle On/Off

## Style
```java
trigger ExampleTrigger on Example__c (
    before insert,
    before update,
    before delete,
    after insert,
    after update,
    after delete
) {

    new ExampleTriggerHandler().run();

}
```