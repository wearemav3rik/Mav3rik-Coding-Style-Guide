# Trigger Pattern
We always want our triggers to be lean and minimalistic. Learn how we declare our triggers in coordination with our Trigger Handler Interface.

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

## Using AccountTriggerHandler with AccountTrigger
```java
trigger AccountTrigger on Account (
    before insert,
    before update,
    before delete,
    after insert,
    after update,
    after delete
) {
    new AccountTriggerHandler().run();
}
```

<br>

## Custom Metadata Toggle On/Off
Example TBD by Raj
