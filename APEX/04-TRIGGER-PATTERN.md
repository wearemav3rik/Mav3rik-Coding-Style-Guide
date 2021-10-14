# Trigger Pattern


## Naming Convention
If the Object is called `Example`, we will use the class name `ExampleTrigger`.

## ExampleTrigger
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

<br>

## Custom Metadata Toggle On/Off
Example TBD by Raj
