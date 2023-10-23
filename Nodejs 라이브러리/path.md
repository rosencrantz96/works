# Path 라이브러리

Node.js의 내부 라이브러리 → npm install 필요 없이 바로 import 후 사용 가능

path 라이브러리를 사용하면 좀 더 쉽게 디렉토리 및 경로를 컨트롤 할 수 있다

## `path.join()`과 `path.resolve()`

```javascript
// 디렉토리를 붙여서 경로를 만들어줄 때 사용
path.join(); // '/'를 만날 경우 절대경로 무시
path.resolve(); // 절대경로 존중
```

: 인자로 받은 경로들을 하나로 합쳐서 문자열 형태로 path(경로)를 리턴한다

## `path.extname('filePath')`

: 파일의 확장자명을 반환

## `path.dirname('filePath')`

: 파일의 디렉토리 주소를 반환

## `path.basename('filePath', path.extname('filePath'))`

: 현재 디렉토리에서 파일명을 반환하는 함수<br>
인자값으로 하나를 더 받을 수 있는데 확장자명을 넣으줄 경우 확장자명을 제외한 파일명을 반환
