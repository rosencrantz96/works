# FS 라이브러리

Node.js 내장 라이브러리

파일 입출력 처리에 사용

## 비동기함수 & 동기함수

fs 모듈은 비동기 API와 동기 API를 모두 제공

> 비동기 메서드: 마지막 인자로 콜백함수를 받고 아무 값도 반환하지 X
> 동기 메서드: 결과값을 반환하며 예외를 일으킬 수 있다 (`Sync`로 끝남)

## `fs.mkdir()`, `fs.mkdirSync()`

: 디렉토리를 만들 때 사용

## `fs.rmdir()`, `fs.rmdirSync()`

: 디렉토리 삭제할 때 사용

## `fs.writeFile()`, `fs.writeFileSync()`

: 파일에 데이터를 쓰기

```javascript
const file = 'test.dat';
const data = '테스트';
fs.writeFile(file, data, err => console.log(err));
```

```javascript
try {
   const file = 'test.dat';
   const data = '테스트';
   fs.writeFileSync(file, date);
} catch (err) {
   console.log(err);
}
```

$*$ 기존 파일에 데이터를 덮어쓰기 때문에 새로운 데이터를 추가하고 싶다면

`appendFile()` 또는 `appendFileSync()` 메서드를 사용해야 한다

## `fs.readFile()`, `fs.readFileSync()`

: 파일로부터 데이터 읽기

```javascript
fs.readFile('test.dat', 'utf8', (err, data) => {
   if (err) {
      console.error(err);
   } else {
      console.log(data);
   }
});
```

$*$ 반드시 두번째 인자를 `utf8`로 명시하여 인코딩이 되도록 해줘야 한다

만일 두 번째 인자를 생략할 경우 콜백 함수의 data로 문자열이 아닌 buffer가 넘어와서 육안으로 인식하기 어렵다

```javascript
try {
   const data = fs.readFileSync('test.dat', 'utf8');
   console.log(data);
} catch (err) {
   console.log(err);
}
```

## `fs.stat()`, `fs.statSync()`

: 파일/디렉토리 메타 정보 확인하기

```javascript
fs.stat('test.dat', (err, stats) => {
   if (err) {
      console.error(err);
   } else {
      console.log({
         size: stats.size, // 파일 크기
         mtime: stats.mtime, // 수정 시간
         isFile: stats.isFile(), // 파일인지 디렉토리인지 반환
      });
   }
});
```

$*$ 콜백 함수의 두번째 인자로 넘어오는 `stats` 객체를 통해 해당 파일이나 디렉토리에 대한 다양한 메타 정보에 접근할 수 있다

```javascript
try {
   const stats = fs.statSync('test.dat');
   console.log({
      size: stats.size,
      mtime: stats.mtime,
      isFile: stats.isFile(),
   });
} catch (err) {
   console.error(err);
}
```

출력 결과

```console
{ size: 18, mtime: 2021-11-13T03:14:37.596Z, isFile: true }
```
