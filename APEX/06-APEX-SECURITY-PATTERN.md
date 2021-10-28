# Apex Security Pattern
Learn about the security patterns we use to ensure we are adhering to the Sharing Model and Field Level Security.

## Enforcing Row Level Security
To enforce row level security on objects within Apex classes, we first need to know if the Apex class is querying any Objects. If our Apex Class is performing a query, Row level security should always be enforced.

Apex classes such as utility classes which do not perform queries do not need row level security.



### Wrong
```java
// This does not enforce row level security
public class ExampleClass {
    public static void queryContacts() {
        List<Contact> contacts = [SELECT Id, Name FROM Contact];
    }
}
```

### With Sharing Rules
```java
// The running user will have access to Contacts which he has access to (by ownership/sharing)
public with sharing class ExampleClass {
    public static void queryContacts() {
        List<Contact> contacts = [SELECT Id, Name FROM Contact];
    }
}
```

### Without Sharing Rules
```java
// The running user will have access every Contacts regardless of ownership/sharing access
public without sharing class ExampleClass {
    public static void queryContacts() {
        List<Contact> contacts = [SELECT Id, Name FROM Contact];
    }
}
```

### This is Ok
```java
// This does not enforce row level security but it is just a utility class
// and there are no queries in this class
public class ExampleClassWithoutQueries {
    public Integer void doubleTheValue(Integer x) {
        return x + x;
    }
}
```

<br>

## Enforcing Object and Field Permissions

By default, apex doesn't enforce object or field-level permissions. Salesforce offers 3 different ways to verify access and how to ensure your Apex security requirements are met.

1. **WITH SECURITY_ENFORCED** - provides security as a clause on SOQL queries.
2. **DescribeFieldResult** - provides the  `isAccessible`, `isCreateable`, or `isUpdateable` methods.
3. **Security** - provides the `stripInaccessible` method.

Reference URL: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_security_sharing_chapter.htm

<br>

### WITH SECURITY_ENFORCED

The first option to verify object and field permission is **WITH SECURITY_ENFORCED** keyword on SOQL query. This will raise an exception if SOQL tries to access something that's not accessible to the user.

- How to?
  - Insert this keyword after WHERE Clause or after FROM clause.
  - And, before ORDER BY, LIMIT, OFFSET or any aggregate function.

Example:
```java
List<Account> act1 = [
  SELECT Id, (SELECT LastName FROM Contacts)
  FROM Account
  WHERE Name like 'Acme'
  WITH SECURITY_ENFORCED
];
```

<br>

### Schema.DescribeSObjectResult and Schema.DescribeFieldResult

This method verifies permissions using DescribeSObjectResult and DescribeFieldResult.
Before performing any DML operation, call any of these methods `isAccessible`, `isCreateable`, or `isUpdateable` to verify if the user has required access. This option is CPU intensive.

Example:
### Read Access
```java
// The following example verifies if the user has read access to specific objects and fields.
public static Boolean hasReadAccess(String sObjectName, Set<String> sFields){
    if(Schema.sObjectType.Case.isAccessible()){
        return hasFieldReadAccess(sObjectName, sFields);
    }
    else{
        return false;
    }
}

public static Boolean hasFieldReadAccess(String sObjectName, Set<String> sFields){
    Schema.SObjectType obj = Schema.getGlobalDescribe().get(sObjectName);
    for(String field: sFields){
        Schema.SObjectField sObjectField = obj.getDescribe().Fields.getMap().get(field);
        if(!sObjectField.getDescribe().isAccessible()){
            return false;
        }
    }
    return true;
}
```
### Create Access
```java
// The following example verifies if the user has create access to specific objects and fields.
public static Boolean hasCreateAccess(String sObjectName, Set<String> sFields){
    if(Schema.sObjectType.Case.isCreateable()){
        return hasFieldCreateAccess(sObjectName, sFields);
    }
    else{
        return false;
    }
}

public static Boolean hasFieldCreateAccess(String sObjectName, Set<String> sFields){
    Schema.SObjectType obj = Schema.getGlobalDescribe().get(sObjectName);
    for(String field: sFields){
        Schema.SObjectField sObjectField = obj.getDescribe().Fields.getMap().get(field);
        if(!sObjectField.getDescribe().isCreateable()){
            return false;
        }
    }
    return true;
}
```
### Update Access
```java
// The following example verifies if the user has update access to specific objects and fields.
public static Boolean hasUpdateAccess(String sObjectName, Set<String> sFields){
    if(Schema.sObjectType.Case.isUpdateable()){
        return hasFieldUpdateAccess(sObjectName, sFields);
    }
    else{
        return false;
    }
}

public static Boolean hasFieldUpdateAccess(String sObjectName, Set<String> sFields){
    Schema.SObjectType obj = Schema.getGlobalDescribe().get(sObjectName);
    for(String field: sFields){
        Schema.SObjectField sObjectField = obj.getDescribe().Fields.getMap().get(field);
        if(!sObjectField.getDescribe().isUpdateable()){
            return false;
        }
    }
    return true;
}
```

<br>

### Security.stripInaccessible

This option was recently introduced by Salesforce to verify user permission.

This method can be used to strip the fields and relationship fields from query and subquery results which the user does not have access to. All inaccessible objects and fields will be removed before DML operations.
Salesforce recommends using this method to verify user access.

- Class: Security.stripInaccessible
- Supported Access Types:
  - `CREATABLE`
  - `READABLE`
  - `UPDATABLE`
  - `UPSERTABLE`

Example:
### Read stripInaccessible
```java
// The following example will strips all the records from recordset before returning the list.
public static void queryWithStripInaccessible(List<Account> accountsList){
    try{
        accountsList = (List<Account>) Security.stripInaccessible(AccessType.READABLE, accountsList).getRecords();
        if(!accountsList.isEmpty()){
            update accountsList;
        }
    }
    Catch (DMLException ex){
        new LogException().log(ex);
    }
}
```
### Create stripInaccessible
```java
// The following example will strips all the records from recordset before creation.
public static void createWithStripInaccessible(List<Account> accountsList){
    try{
        accountsList = (List<Account>) Security.stripInaccessible(AccessType.CREATABLE, accountsList).getRecords();
        if(!accountsList.isEmpty()){
            update accountsList;
        }
    }
    Catch (DMLException ex){
        new LogException().log(ex);
    }
}
```
### Update stripInaccessible
```java
// The following example will strips all the records from recordset before update.
public static void updateWithStripInaccessible(List<Account> accountsList){
    try{
        accountsList = (List<Account>) Security.stripInaccessible(AccessType.UPDATABLE, accountsList).getRecords();
        if(!accountsList.isEmpty()){
            update accountsList;
        }
    }
    catch (DMLException ex){
        new LogException().log(ex);
    }
}
```
### Upsert stripInaccessible
```java
// The following example will strips all the records from recordset before upsert operation.
public static void upsertWithStripInaccessible(List<Account> accountsList){
    try{
        accountsList = (List<Account>) Security.stripInaccessible(AccessType.UPSERTABLE, accountsList).getRecords();
        if(!accountsList.isEmpty()){
            update accountsList;
        }
    }
    Catch (DMLException ex){
        new LogException().log(ex);
    }
}

```
