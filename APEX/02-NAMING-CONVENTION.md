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

<br>

## Mutable Variables (Non-Final)
1. Naming our final variables should be in `Camel Case`.
2. It should also begin with an `Lower Case` character.
3. Special characters should be avoided such as Dash `-` or Underscore `_`.
```
Examples:
String accountName = 'Lorem Ipsum';
Integer numberWhichWillBeIncremented = 1;
Boolean isActivated = true;
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