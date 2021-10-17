# Apex Security Pattern

## Enforcing Row Level Security
To enforce row level security on objects within Apex classes, we need to query Objects inside the scope of an Apex class. Row level security should always be enforced on an apex class.

Apex classes without queries do not need row level security.

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
```java
public static void updateWithStripInaccessible(List<Account> accountsList){
    try{
        accountsList = (List<Account>) Security.stripInaccessible(AccessType.UPDATABLE, accountsList).getRecords();
        if(!accountsList.isEmpty()){
            update accountsList;
        }
    }
    Catch (DMLException ex){
        new LogException().log(ex);
    }
}
```
