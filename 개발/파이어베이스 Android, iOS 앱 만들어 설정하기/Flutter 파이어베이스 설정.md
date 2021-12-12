## 파이어베이스에서 프로젝트 생성

## Android 앱 생성 후 설정

다운로드 받은 google-services.json 파일을 android/app/google-services.json 경로에 붙여넣습니다.

`android/app/build.gradle` 파일 내에 아래 코드를 추가합니다.

```shell
apply plugin: 'com.google.gms.google-services'
```

`android/build.gradle` 파일 내에 아래 코드를 추가합니다.

```shell
dependencies {
		...
        classpath 'com.google.gms:google-services:4.3.4'
    }
```





## iOS 앱 생성 후 설정

ios 디렉터리를 Xcode로 열어서 Runner > Runner 하위에 GoogleService-Info.plist 파일을 드래그해서 위치시킵니다.