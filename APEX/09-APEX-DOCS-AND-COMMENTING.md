# Apex Docs and Commenting

We will be using the conventions set by ApexDocs so we can generate them as needed. To learn more about ApexDocs, visit [SalesforceFoundation/ApexDoc](https://github.com/SalesforceFoundation/ApexDoc).

<br>

## Class Comments
```java
/**
 * @description       : Controller class for Some_Object__c
 * @author            : Ashwini Raman (Mav3rik)
 * @group             : Mav3rik
 * @last modified on  : 10-15-2021
 * @last modified by  : glenn.dimaliwat@mav3rik.com
 **/
 public with sharing class SomeObjectController {
 }
```

<br>

## Class Method Comments
```java
/**
* @description Query list of Some_Object__c
* @author kimwan.ogot@mav3rik.com | 10-12-2021
* @param contact - Contact Id
* @return List<Some_Object__c>
**/
public static List<Some_Object__c> getSomeObjectList(Id contact) {
}
```

<br>

## Useful plugins for Apex Docs
```
VSCode Salesforce Documenter
```