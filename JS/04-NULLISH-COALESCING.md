```javascript
const objTree = {
  person: {
    name: 'John';
  }
}
console.log(objTree.person.name??'default string'); // John
console.log(objTree.person.age??'default string'); // default string
```