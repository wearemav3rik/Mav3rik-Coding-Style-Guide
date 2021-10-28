# Accessing Record Types
We like to access our record types without the need for extra queries.

<br>

## Example on Standard Object Account
```java
Id recordTypeId =
  Schema.SObjectType.Account
  .getRecordTypeInfosByDeveloperName()
  .get('Some_Account_Record_Type_API_Name')
  .getRecordTypeId();
```

<br>

## Example on Custom Object Some_Object__c
```java
Id recordTypeId =
  Schema.SObjectType.Some_Object__c
  .getRecordTypeInfosByDeveloperName()
  .get('Some_Custom_Record_Type_API_Name')
  .getRecordTypeId();
```