Node:PATH模块

###path.dirname(path)
```
path.dirname('/foo/bar/baz/asdf/quux')
// returns '/foo/bar/baz/asdf'
```
###path.basename
```
path.basename('/foo/bar/baz/asdf/quux.html') // returns 'quux.html'

path.basename('/foo/bar/baz/asdf/quux.html', '.html') // returns 'quux'
```
###path.extname(path)
```
path.extname('index.html')
// returns '.html'

path.extname('index.coffee.md')
// returns '.md'

path.extname('index.')
// returns '.'

path.extname('index')
// returns ''

path.extname('.index')
// returns ''
```

###path.delimiter
```
console.log(process.env.PATH)
// '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin'

process.env.PATH.split(path.delimiter)
// returns ['/usr/bin', '/bin', '/usr/sbin', '/sbin', '/usr/local/bin']
```
###path.sep
```
'foo/bar/baz'.split(path.sep)
// returns ['foo', 'bar', 'baz']
```
###path.format(pathObject)

+ dir <String>
+ base <String>
+ name <String>
+ ext <String>
+ root <String>

```
// If `dir` and `base` are provided,
// `${dir}${path.sep}${base}`
// will be returned.
path.format({
  dir: '/home/user/dir',
  base: 'file.txt'
});
// returns '/home/user/dir/file.txt'

// `root` will be used if `dir` is not specified.
// If only `root` is provided or `dir` is equal to `root` then the
// platform separator will not be included.
path.format({
  root: '/',
  base: 'file.txt'
});
// returns '/file.txt'

// `name` + `ext` will be used if `base` is not specified.
path.format({
  root: '/',
  name: 'file',
  ext: '.txt'
});
// returns '/file.txt'

// `base` will be returned if `dir` or `root` are not provided.
path.format({
  base: 'file.txt'
});
// returns 'file.txt'

```
###path.join([path[, ...]])
```
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..')
// returns '/foo/bar/baz/asdf'

path.join('foo', {}, 'bar')
// throws TypeError: Arguments to path.join must be strings
```
###path.normalize(path)
```
path.normalize('/foo/bar//baz/asdf/quux/..')
// returns '/foo/bar/baz/asdf'
```
###path.parse(path)
+ dir <String>
+ base <String>
+ name <String>
+ ext <String>
+ root <String>

```
path.parse('/home/user/dir/file.txt')
// returns
// {
//    root : "/",
//    dir : "/home/user/dir",
//    base : "file.txt",
//    ext : ".txt",
//    name : "file"
// }

```
###path.isAbsolute(path)
```
path.isAbsolute('/foo/bar') // true
path.isAbsolute('/baz/..')  // true
path.isAbsolute('qux/')     // false
path.isAbsolute('.')        // false
```
###path.relative(from, to)
```
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb')
// returns '../../impl/bbb'
```
###path.resolve([path[, ...]])
```
path.resolve('/foo/bar', './baz')
// returns '/foo/bar/baz'

path.resolve('/foo/bar', '/tmp/file/')
// returns '/tmp/file'
```
