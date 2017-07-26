A Guide to Unit Testing
=================================

### The goal of these guidelines is to make your tests:

* Readable
* Maintainable
* Trustworthy

### Unit tests are not for finding bugs!!!

Goal | Strongest technique
------------ | -------------	
Finding bugs (things that don’t work as you want them to)| Manual testing (sometimes also automated integration tests)  
Detecting regressions (things that used to work but have unexpectedly stopped working| Automated integration tests (sometimes also manual testing, though time-consuming)
Designing software components robustly|	Unit testing (within the TDD process)

### Unit Testing Best Practices
1. **Always Write Isolated Test Cases**  
The order of execution has to be independent between test cases.
2. **Test One Thing Only in One Test Case**  
If a method has several end results, each one should be tested separately. 
Whenever a bug occurs, it will help you locate the source of the problem.
**:(**
```javascript
it('should send the data to the server and update the view properly', () => 
{
  // expect(...)to(...);
  // expect(...)to(...);
});
```
**:)**
```javascript
it('should send the data to the server', () => {
  // expect(...)to(...);
});

it('should update the view properly', () => {
  // expect(...)to(...);
});
```
Beware that writing "AND" or "OR" when naming your test smells bad...

4. **Describe your tests properly**  
This helps to avoid comments and increases the maintainability and in the case a test fails you 
know faster what functionality has been broken. Keep in mind that someone else will read it too.
Tests can be the live documentation of the code.  

In order to help you write test names properly, you can use the "unit of work - scenario/context - expected behaviour" pattern:

```javascript
describe('[unit of work]', () => {
  it('should [expected behaviour] when [scenario/context]', () => {
  });
});
```
Or whenever you have many tests that follow the same scenario or are related to the same context:

```javascript
describe('[unit of work]', () => {
  describe('when [scenario/context]', () => {
    it('should [expected behaviour]', () => {
    });
  });
});
```

```javascript
describe("binding dependencies", () => 
{
    it("should remove handler when element was removed", () => 
    {
        // code
    });

    it("should add handler when element was added", () => 
    {
        // code
    })
})
```
5. **Use the Arrange-Act-Assert Style**
6. **Measure Code Coverage to Find Missing Test Cases**  
Use [wallaby app](http://wallabyjs.com/app/#/tests).
7. **Don't Forget to Refactor the Test Code**
Also maintain your test code (especially when after refactoring the code under test).
8. **Limit Use of Mocks**  
In some cases absolutely necessary, but with better design stubs should be enough.  
*Mocks vs stubs*  
*Mock* objects are used to define *expectations* i.e: In this scenario I expect method A() to be 
called with such and such parameters.Mocks record and verify such expectations.
*Stubs*, on the other hand have a different purpose: they do not record or verify expectations, 
but rather allow us to *“replace”* the behavior, state of the “fake” object in order to 
utilize a test scenario.
9. **Avoud logic in tests**  
**:(**
```javascript
it('should properly sanitize strings', () => {
  let result;
  const testValues = {
    'Avion'         : 'Avi' + String.fromCharCode(243) + 'n',
    'The-space'     : 'The space',
    'Weird-chars-'  : 'Weird chars!!',
    'file-name.zip' : 'file name.zip',
    'my-name.zip'   : 'my.name.zip'
  };

  for (result in testValues) {
    expect( sanitizeString(testValues[result]) ).toEqual(result);
  }
});
```
**:)**
```javascript
it('should sanitize a string containing non-ASCII chars', () => {
  expect( sanitizeString('Avi'+String.fromCharCode(243)+'n') ).toEqual('Avion');
});

it('should sanitize a string containing spaces', () => {
  expect( sanitizeString('The space') ).toEqual('The-space');
});

it('should sanitize a string containing exclamation signs', () => {
  expect( sanitizeString('Weird chars!!') ).toEqual('Weird-chars-');
});

it('should sanitize a filename containing spaces', () => {
  expect( sanitizeString('file name.zip') ).toEqual('file-name.zip');
});

it('should sanitize a filename containing more than one dot', () => {
  expect( sanitizeString('my.name.zip') ).toEqual('my-name.zip');
});
```
10. **Don't write unnecessary expectations**  
Remember, unit tests are a design specification of how a certain behaviour should work, not a 
list of observations of everything the code happens to do.
11. **Cover the general case and the edge cases**
"Strange behaviour" usually happens at the edges... 
Remember that your tests can be the live documentation of your code.
12. **Test the behaviour, not the internal implementation**
**:(**

```javascript
it('should add a user in memory', () => {
  userManager.addUser('Dr. Falker', 'Joshua');

  expect(userManager._users[0].name).toBe('Dr. Falker');
  expect(userManager._users[0].password).toBe('Joshua');
});
```
A better approach is to test at the same level of the API:

**:)**

```javascript
it('should add a user in memory', () => {
  userManager.addUser('Dr. Falker', 'Joshua');

  expect(userManager.loginUser('Dr. Falker', 'Joshua')).toBe(true);
});
```
*Advantage*:
Changing the internal implementation of a class/object will not necessarily force you to 
refactor the tests
*Disadvantage*:
If a test is failing, we might have to debug to know which part of the code needs to be fixed
Here, a balance has to be found, unit-testing some key parts can be beneficial.
13. **Create new tests for every defect**  
Whenever a bug is found, create a test that replicates the problem before touching any code. From there, you can apply TDD as usual to fix it.

### Current problems in AngularJS

1. Only methods that are presented in the interfaces should be tested or mocked. The only 
exception now are controllers in AngularJS and their bindToController properties.
```javascript
bindToController: {
    isOpen: "=",
    viewModel: "="
}
```
2. Instead of first defining items and later testing them. Do define and testing together.
3. Wiseman said: “If you need to debug unit test then something is wrong with it’s unitness.”
Don't write too big classes or functions.


### Style
1. Alignment

```javascript
It("should do something", () =>
{
    //code
});
```

