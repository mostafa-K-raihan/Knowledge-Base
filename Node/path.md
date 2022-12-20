- `path.join`
	- create a new path by concatenating first and second argument
	- usually the first argument is the current directory and the last one is the file/folder name
```js
const (entry in fs.readdirSync(dir)) {
	const fullpath = path.join(dir, entry)
	console.log(fullpath)
}
```