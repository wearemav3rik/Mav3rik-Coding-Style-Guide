# Naming Convention

## Class Names
1. Naming our classes should be in `Pascal Case`.
2. It should also begin with an `Upper case` character.
3. Special characters should be avoided such as Dash `-` or Underscore `_`.

```
Examples:

AccountController
AccountControllerTest
TestDataFactory
ContactUpdateBatch
```
```
Wrong:

Account_Controller
Account_Controller_Test
AccountController_Test
accountController
Test_Data_Factory
TestData_Factory
Testdatafactory
Contact_Update_Batch
ContactUpdate_Batch
contactupdatebatch
```

<br>

## Class Methods
1. Naming our classes should be in `Camel Case`.
2. It should also begin with an `Lower Case` character.
3. Special characters should be avoided such as Dash `-` or Underscore `_`.

```
Examples:

getAllAccounts
findAllAccountsWithActivities
```
```
Wrong:

get_All_Accounts
get_AllAccounts
GetAllAccounts
GETALLACCOUNTS
getallaccounts
FindAllAccountsWithActivities
find-All-Accounts-With-Activities
find-all-accounts-with-activities
findAllAccountsW/Activities
```

<br>

## Mutable Variables (Non-Final)
1. Naming our final variables should be in `Camel Case`.
2. It should also begin with an `Lower Case` character.
3. Special characters should be avoided such as Dash `-` or Underscore `_`.
4. Avoid single letter variable names.
```
Examples:

String accountName = 'Lorem Ipsum';
Integer numberWhichWillBeIncremented = 1;
Boolean isActivated = true;
for (List<Account> account : accounts) {}
for (List<Account> acct : accounts) {}
```
```
Wrong:

String AccountName = 'Lorem Ipsum';
String ACCOUNTNAME = 'Lorem Ipsum';
String ACCOUNT_NAME = 'Lorem Ipsum';
String a = 'Lorem Ipsum';
Integer NumberWhichWillBeIncremented = 1;
Integer number_Which_Will_Be_Incremented = 1;
Integer n = 1;
Boolean is_Activated = true;
Boolean i = true;
```
```
Potentially up for discussion:

for (Integer i=0; i<10; i++) {}
for (List<Account> a : accounts) {}
```

<br>

## Immutable Variables (Final)
1. Naming our non-final variables should be in `Snake Case`.
2. It should also be `All Upper Case` characters.
```
Examples:

final String PAGE_LONG_DESCRIPTION = 'Lorem Ipsum';
final Integer BATCH_LIMIT = 50;
final Boolean ENABLE_LOG_EXCEPTION = true;
```
```
Wrong:

final String PAGELONGDESCRIPTION = 'Lorem Ipsum';
final String lagelongdescription = 'Lorem Ipsum';
final Integer batchLimit = 50;
final Integer BatchLimit = 50;
final Boolean ENABLELOG_EXCEPTION = true;
final Boolean enable_log_exce[topm] = true;
```