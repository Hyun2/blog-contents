permission_handler 패키지 사용 시 사용하지 않은 퍼미션에 대해서는 사용하지 않는다고 명시해주어야 합니다. 명시하지 않을 경우 앱스토어 커넥트에 빌드 파일 올릴 때 아래와 같은 수정 요구 이메일이 옵니다.

![](E:\blog\플러터 permission_handler 세팅, Podfile 수정\permission_handler 수정 요구 메일.JPG)



**ios/Podfile 파일 수정**

```sh
post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
  end
end
```

위 코드를 아래와 같이 바꿔줍니다.

```sh
post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
    target.build_configurations.each do |config|
      config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
        '$(inherited)',
      
        ## dart: PermissionGroup.calendar
        'PERMISSION_EVENTS=0',
      
        ## dart: PermissionGroup.reminders
        'PERMISSION_REMINDERS=0',
      
        ## dart: PermissionGroup.contacts
        'PERMISSION_CONTACTS=0',
      
        ## dart: PermissionGroup.camera
        'PERMISSION_CAMERA=0',
      
        ## dart: PermissionGroup.microphone
        # 'PERMISSION_MICROPHONE=0',
      
        ## dart: PermissionGroup.speech
        # 'PERMISSION_SPEECH_RECOGNIZER=0',
      
        ## dart: PermissionGroup.photos
        'PERMISSION_PHOTOS=0',
      
        ## dart: [PermissionGroup.location, PermissionGroup.locationAlways, PermissionGroup.locationWhenInUse]
        'PERMISSION_LOCATION=0',
        
        ## dart: PermissionGroup.notification
        'PERMISSION_NOTIFICATIONS=0',
      
        ## dart: PermissionGroup.mediaLibrary
        'PERMISSION_MEDIA_LIBRARY=0',
      
        ## dart: PermissionGroup.sensors
        'PERMISSION_SENSORS=0'
      ]
    end
  end
end
```

전 PERMISSION_MICROPHONE, PERMISSION_SPEECH_RECOGNIZER 기능을 사용하기 때문에 이 부분은 주석처리하였습니다. 나머지 기능은 모두 사용하지 않기 때문에 0으로 변경하였습니다.

물론 사용하는 퍼미션이 있는 경우에는 info.plist 에 사용한다는 것을 명시해주어야 합니다.

![](E:\blog\플러터 permission_handler 세팅, Podfile 수정\info.plist for permission_handler.JPG)

Podfile 파일을 수정하고 다시 아카이브를 업로드 하여도 동일한 문제가 발생하면, 아래 코드를 입력해서 pod을 새로 세팅해줍니다.

```shell
flutter clean && flutter pub get && cd ios/ && pod install 
```

