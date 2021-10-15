# Apex Security Pattern

## Enforcing Object and Field Permissions

By default, apex doesn't enforce object or field-level permissions. Salesforce offers 3 different ways to verify access and how to ensure your user will not see, what he's not supposed to see.

1. **WITH SECURITY_ENFORCED** clause on SOQL queries
2. **DescribeFieldResult** class and its method isAccessible
3. **Security** class and its method **stripInaccessible**

Reference URL: https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_security_sharing_chapter.htm

###
1. WITH SECURITY_ENFORCED

The first option to verify object and field permission is **WITH SECURITY_ENFORCED** keyword on SOQL query. This will raise an exception if SOQL tries to access something that's not accessible to the user.

- How to?
  - Insert this keyword after WHERE Clause or after FROM clause.
  - And, before ORDER BY, LIMIT, OFFSET or any aggregate function.

Example:
##
```java
List<Account> act1 = [SELECT Id, (SELECT LastName FROM Contacts) FROM Account WHERE Name like 'Acme' WITH SECURITY_ENFORCED]
```

###

2. Schema.DescribeSObjectResult and Schema.DescribeFieldResult

This method is the oldest of all and verifies permissions using DescribeSObjectResult and DescribeFieldResult.
Before performing any DML operation, call any of these methods isAccessible, isCreateable, or isUpdateable to verify if the user has required access.
This option is CPU intensive.

Example:

The following example verifies if the user has update access to specific objects and fields.

##
```java
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
###

3. Security.stripInaccessible

This option was recently introduced by Salesforce to verify user permission.

This method can be used to strip the fields and relationship fields from query and subquery results that the user can’t access.
All inaccessible objects and fields will be removed before DML operations.
Salesforce recommends using this method to verify user access.


- Class: Security.stripInaccessible
- Supported Access Types:
  - CREATABLE
  - READABLE
  - UPDATABLE
  - UPSERTABLE

##
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