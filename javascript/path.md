# path 모듈


### join

- OS의 파일 구분자를 이용해 파일 위치를 조합한다

```javascript
path.join('/foo', 'bar', 'baz/asdf');
```

> return : /foo/bar/baz/asdf
  
 
 
### resolve

- 오른쪽에서 왼쪽으로 유효한 위치를 가지고 파일 위치를 구성한다

```javascript
path.resolve('/foo/bar', './baz');
```

return : /foo/bar/baz
 
- 만약 결과 값이 유효하지 않으면 현재 디렉토리가 사용된다 (absolute URL 반환)

```javascript
path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
```

> return : /home/myself/node/wwwroot/static_files/gif/image.gif (현재 디렉토리 = /home/myself/node)
 
 
---
- [Node Path](https://nodejs.org/api/path.html)
