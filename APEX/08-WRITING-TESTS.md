# Writing Unit Tests
Writing tests is not just about Code Coverage. We want to adapt a test driven mindset and be able to ship our code with confidence. Our patterns ensure that we have considered both positive and negative scenarios of a feature.

<br>

## Basic Test Patterns
We want to be testing for `Positive`, `Negative`, `Null` and `Empty` scenarios as much as possible.
For the purpose of this example, let's say the method I am testing is:
```java
// The method I am testing
public class Chores {
  public static Boolean doDishes(List<String> thingsImBusyWith) {
    if (thingsImBusyWith == null) {
      throw new Exception('What is your excuse?');
    } else if (!thingsImBusyWith.isEmpty()) {
      return false;
    }
    
    System.debug('I have time to do the dishes and I will do it now!');
    return true;
  }
}
```

<br>

### Positive Test
```java
@IsTest
static void doDishesPositive() {
  List<String> givenThingsImBusyWith = new List<String>();
  Boolean expected = true;

  Test.startTest();
  Boolean actual = Chores.doDishes(givenThingsImBusyWith);
  Test.stopTest();

  System.assertEquals(expected, actual, 'Hooray I have done the dishes!');
}
```

<br>

### Negative Test
```java
@IsTest
static void doDishesNegative() {
  List<String> givenThingsImBusyWith = 
    new List<String>{ 'I have a meeting', 'I am sick' };
  Boolean expected = false;

  Test.startTest();
  Boolean actual = Chores.doDishes(givenThingsImBusyWith);
  Test.stopTest();

  System.assertEquals(expected, actual, 'Hooray I am excused!');
}
```

<br>

### Null Test
```java
@IsTest
static void doDishesNull() {
  List<String> givenThingsImBusyWith = null;
  String actualExceptionMessage = '';
  String expectedExceptionMessage = 'What is your excuse?';

  Test.startTest();
  try {
    Chores.doDishes(givenThingsImBusyWith);
  } catch(Exception e) {
    actualExceptionMessage = e.getMessage();
  }
  Test.stopTest();

  System.assertEquals(expectedExceptionMessage, actualExceptionMessage, 'You have no excuse not to do the dishes!');
}
```

<br>

### Empty Test
```java
@IsTest
static void doDishesEmpty() {
  List<String> givenThingsImBusyWith = new List<String>();
  String actualExceptionMessage = '';
  String expectedExceptionMessage = '';

  Test.startTest();
  try {
    Chores.doDishes(givenThingsImBusyWith);
  } catch(Exception e) {
    actualExceptionMessage = e.getMessage();
  }
  Test.stopTest();

  System.assertEquals(expectedExceptionMessage, actualExceptionMessage, 'I have nothing to do, so might as well do the dishes!');
}
```