# Optional Chaining

Suppose we have to capture data from an object structure such as `event.currentTarget.dataset.id` and we need to validate `undefined` on each level, we would normally go for the following conditions:

## Option 1 - Lengthy Conditional (harder to read)
```javascript
if (event && event.currentTarget && event.currentTarget.dataset && event.currentTarget.id) {
	console.log('event.currentTarget is not undefined')
} else {
	console.log('event.currentTarget is undefined')
}
```

## Option 2 - Optional Chaining (easier to read)
```javascript
if (event?.currentTarget?.dataset?.id) {
	console.log('event.currentTarget is not undefined')
} else {
	console.log('event.currentTarget is undefined')
}
```