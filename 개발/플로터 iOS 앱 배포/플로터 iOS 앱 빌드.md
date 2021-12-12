## 앱 아이콘 추가

1024px 크기의 이미지를 이용해서 iOS 앱 아이콘을 만들어 추가할 수 있습니다.

아래 사이트를 이용해서 아이콘을 만들어 다운로드 받으면 AppIcons 디렉토리를 얻을 수 있습니다.

https://appicon.co/

![](E:\blog\플로터 iOS 앱 배포\ios 앱 아이콘 생성.JPG)

Xcode에서 ios 디렉터리를 열어 Runner > Runner > Assets.xcassets 으로 이동합니다. 그리고 AppIcon.appiconset 디렉터리를 드래그해서 넣습니다. 마지막으로 원래 AppIcon을 삭제해줍니다.

![](E:\blog\플로터 iOS 앱 배포\iOS 앱 아이콘 xcode에서 추가.gif)



## Xcode 프로젝트 설정

좌측 Runner 메뉴의 General 탭에서 Display Name, Bundle Identifier, Version, Build 인풋에 값을 설정해줍니다.

![](E:\blog\플로터 iOS 앱 배포\Xcode 프로젝트 설정1.JPG)

Signing & Capabilities 탭으로 이동한 후 Team 인풋에 계정을 지정해줍니다.

![](E:\blog\플로터 iOS 앱 배포\Xcode 프로젝트 설정2.JPG)



## 앱스토어 커넥트에서 앱 생성

https://appstoreconnect.apple.com/apps 에 접속 후 앱 옆의 플러스 버튼을 누르고 신규 앱을 눌러줍니다. 필요한 정보들을 작성합니다. 번들 ID는 이전 과정인 Xcode 설정에서 Bundle Identifier로 설정했던 것으로 선택해줍니다.

![](E:\blog\플로터 iOS 앱 배포\신규 앱 생성 정보 창.JPG)



## 빌드 아카이브 생성하기

Xcode에서 상단 메뉴 바에서 Product > Scheme > Runner 를 선택합니다. 

이어서 Product > Destination > Any iOS Device 를 선택합니다.

이어서 Product > Archive 를 선택합니다. 

Archive 생성이 완료되면, Validate app 버튼을 누릅니다. 

아래와 같이 체크하고 Next 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\validate app1.JPG)

Automatically manage signing 선택 후 Next 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\validate app2.JPG)

Validate 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\validate 버튼 누르기 전.JPG)

Validate가 끝났으면 Distribute App 버튼을 누르고 App Store Connect 선택 후 Next 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\select a method of distribution.JPG)

Upload 선택 후 Next 버튼을 누릅니다.

![select a destination](E:\blog\플로터 iOS 앱 배포\select a destination.JPG)

계속해서 Next 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\next1.JPG)

![](E:\blog\플로터 iOS 앱 배포\next2.JPG)

Upload 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\upload.JPG)

업로드를 완료하면 앱스토어 커넥트 홈페이지에 접속해서 앱 정보를 입력합니다.



## 앱 정보 입력

앱스토어 커넥트 홈페이지에서 아까 생성했던 앱의 페이지로 들어옵니다. 좌측 메뉴의 앱 정보를 선택합니다. 자동으로 채워져 있는 부분들은 확인하고

![](E:\blog\플로터 iOS 앱 배포\앱 정보1.JPG)

가운데 콘텐츠 권한 정보 설정 글자를 클릭합니다.

![](E:\blog\플로터 iOS 앱 배포\앱 정보2.JPG)

타사 콘텐츠에 대한 권한 설정을 선택하고 완료버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\앱 정보3.JPG)

상단 우측의 저장 버튼을 눌러 수정한 내용을 저장합니다.

좌측 메뉴의 앱이 수집하는 개인정보 메뉴를 클릭하고 개인정보 처리방침 글자 옆의 편집을 누릅니다.

앱에서 사용하는 개인정보에 대한 처리방침이 적힌 URL을 넣고 저장 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\개인정보처리방침 URL.JPG)



## iOS 앱 제출

좌측 iOS 앱 하단에 제출 준비중 메뉴를 누릅니다. 앱 스크린샷을 올립니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 스크린샷.JPG)

이어서 앱에 대한 간단한 설명을 작성합니다. 저는 지원 URL은 제 깃헙 URL로, 마케팅 URL은 구글 애드몹 정보를 깃헙에 올려서 얻은 URL로 작성하였습니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 앱 정보.JPG)

아까 업로드한 빌드 파일을 선택합니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 빌드 파일 업로드.JPG)

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 빌드 파일 업로드0.JPG)

암호화를 사용하는지 여부에서 암호화를 사용하지 않으면 아니요를 선택합니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 수출 규정 준수 정보.JPG)

로그인 기능이 없는 경우 로그인 필요에 체크 해제합니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 로그인 체크 해제.JPG)

그 외엔 수정 없이 저장 버튼을 누르고, 심사를 위해 제출 버튼을 누릅니다. 아직 덜 작성된 부분이 있으면 아래와 같은 방식으로 알려줍니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 심사 제출 불가.JPG)

가격 글자를 눌러서 가격을 설정하고 저장 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 가격 설정.JPG)

이어서 좌측에서 앱이 수집하는 개인정보 메뉴를 클릭합니다. 시작하기 버튼을 눌러서 개인정보 수집 여부에 대한 설정을 진행합니다. 게시 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 개인정보 설정.JPG)

다시 좌측의 제출 준비 중 메뉴를 누른 중간 쯤으로 이동하면, 등급이라는 글자 옆에 편집이 보이는데, 편집을 누릅니다. 등급 지정에 필요한 내용을 작성 후 완료 버튼을 누릅니다.

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 등급.JPG)

![](E:\blog\플로터 iOS 앱 배포\제출 준비 - 등급 편집.JPG)

저장 버튼을 누르고, 심사를 위해 제출 버튼을 누릅니다. 좌측 메뉴를 확인하면 제출 준비 중이 심사 대기 중으로 변한 것을 확인할 수 있습니다.