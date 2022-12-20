- `readdirSync`
	- Read dir given as a param, synchronusly
	
```js
const entries = fs.readdirSync('.')
console.log(entries)
```
- `readdir`
	- its async version, which takes a callback as an additional param 

```js
	fs.readdir('.', (err, entries) => {
		console.log(entries)
	})
```
- `stat` vs `lstat` vs `fstat`
	- return some statistics about the given file
	- if the file is a symbolic link, `stat` will return info about the target file(actual file not the link)
	- `lstat` will do that for the symbolic link (NOT the actual target)
	- `fstat` takes a file descriptor, rather than a path
```
fs.statSync(path)
// or
fs.stat(path, (err, statistics) => {
//
})
```