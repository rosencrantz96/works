## Backend response format

#### schemes:

이 툴은 다음 스키마들 중 하나를 통해서 작동한다

1. 기기에서 파일을 업로드하기
2. URL을 통해서 업로드하기 (handle image-like URL's pasting)
3. 드래그-앤-드랍을 통해서 파일을 업로드하기
4. 클립보드에서 붙여넣기를 통해 업로드하기

### Uploading files from device

#### 시나리오

1. 유저가 기기에서 파일을 선택한다
2. 선택된 파일을 툴이 **당신의** 백엔드에게 보낸다 (on `config.endpoints.byFile` route)
3. 당신의 백엔드가 파일을 저장하고, 지정된 JSON 포맷으로 파일 데이터를 리턴할 것이다
4. Image too이 저장된 이미지를 보여주고 서버의 응답을 저장한다

그러므로 당신은 파일 저장을 위한 백엔드를 원하는 방식으로 구현할수 있다

이것은 당신의 환경과 스택에 따라서 구체적이고 사소한 작업이다

이 툴은 request를 `multipart/form-data`로 실행한다, 이는 구성에서 키를 `field` 값으로 사용한다

당신의 uploader 응답은 다음 포맷을 **포함해야 한다**

```javascript
{
    "success": 1,
    "file": {
        "url": "https://www.tesla.com/tesla_theme/assets/img/_vehicle_redesign/roadster_and_semi/roadster/hero.jpg",
        // ... 그리고 너비나 높이, 색상, 확장명 등과 같이 저장하려는 추가 필드에 대한 내용들
    }
}
```

**success** - 업로드 상태. 1은 성공, 0은 실패

**file** - 업로드 된 파일 데이터. **반드시** 업로드 된 이미지에 대한 퍼블릭 경로 전체를 지닌 `url` 필드를 포함해야 한다. 또한 당신이 저장하길 원하는 추가적인 필드 또한 담을 수 있다. 예를 들어, 넓이나 높이, 아이디 값 등이 있다. 모든 추가된 필드는 출력 데이터 객체 `file`에 저장될 것이다.

### Uploading by pasted URL

#### 시나리오

1. 사용자가 이미지 파일의 URL을 복사하여 에디터에 붙여넣는다.
2. 에디터는 붙여넣은 문자열을 Image Tool에 전달한다.
3. 툴은 그것을 request body에 있는 'url'을 통해 **당신의** 백엔드에게 전달한다. (on `config.endpoints.byUrl` route)
4. 백엔드는 URL을 받은후, **전달받은 URL을 원본 파일로 다운로드 받은 후 저장하고** 지정된 JSON 포맷으로 파일 데이터를 응답한다.
5. Image tool은 저장된 이미지를 보여준 후 서버 응답을 저장한다.

uploader 의 응답은 **Uploading files from device** 섹션에서 설명한 것과 동일한 포맷이어야 한다.

### Uploading by drag-n-drop or from Clipboard

당신의 백엔드는 `config.field`로 지정된(by default, `imgage`) 필드 이름의 FormData 객체로 파일을 받을것이다. 이를 저장하고 위에서 설명했던것과 같은 응답 포맷으로 리턴해야 한다.

## 참고 문서

[Edior-js/image](https://github.com/editor-js/image)
