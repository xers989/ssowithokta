## Single Sign On Atlas with OKTA
MongoDB Atlas 에 Single Sign On으로 로그인 하기 위한 설정으로 대중적인 OKTA를 이용합니다.
Atlas의 경우 SAML을 이용한 SSO를 제공 합니다. OKTA와 SAML Federation을 이용하여 연계 함으로 OKTA 로그인 후 Atlas를 추가 로그인 없이 사용 합니다.
OKTA 와 MongoDB Atlas 의 계정을 모두 소유 하고 있어야 하며 관리자 권한을 필요로 합니다.


### OKTA Application 등록
OKTA의 계정을 생성 하고 로그인 합니다.
https://www.okta.com/
Workforce Identity의 30일 무료 계정 생성 합니다.
계정이 생성 되면 전용 도메인이 생성 됩니다
<img src="/images/image01.png" width="90%" height="90%">    
제공되는 URL을 클릭 하여 OKTA에 로그인을 합니다. (계정이 생성 되면 임시 패스워드로 로그인이 가능 하며 활성화 과정에서 초기 패스워드 설정 및 추가 인증 수단 - MFA를 등록하여야 합니다.)
로그인이 된 화면은 다음과 같습니다.    
<img src="/images/image02.png" width="90%" height="90%">    

SSO 로 접근하기 위한 Application 등록을 위해 Admin 버튼을 클릭 합니다. (관리자 권한이 있어야 하며 다시 한번 인증을 하게 됩니다.)
로그인 후 Use Single Sign On 의 Add App 버튼을 클릭 합니다.
<img src="/images/image03.png" width="90%" height="90%">    

MongoDB 로 검색 하면 MongoDB Atlas 를 찾을 수 있습니다. 검색 후 이를 클릭 합니다.
<img src="/images/image04.png" width="90%" height="90%">    

Add Integration 을 클릭 하여 추가 하여 줍니다.    
<img src="/images/image05.png" width="90%" height="90%">    

Single Sign On 애플리케이션을 등록 하여야 합니다. 이름 및 Icon 표시를 선택 할 수 있습니다.
Application Label은 사용자가 OKTA에 로그인 후에 보게 되는 접근 가능한 애플리케이션 목록에 표시되는 이름 입니다.
테스트에서는 확인을 위해 이름을 MongoDB Atlas-SSO로 변경 하며 Visibility를 uncheck 하여 아이콘이 보여지도록 설정 합니다.    
<img src="/images/image06.png" width="90%" height="90%">   

Sign-On Option 에서 SAML2.0 을 선택 하여 주고 완료 하여 줍니다.    
<img src="/images/image30.png" width="80%" height="80%">    

저장 후에 Application 에 Active 상태로 등록 한 MongoDB Atlas-SSO가 등록 된 것을 확인 합니다.    
<img src="/images/image31.png" width="80%" height="80%">   

생성한 SAML의 인증서를 받기 위해 이를 클릭 합니다.    

Sign On 탭에 하단에 SAML Signing Certificates 를 볼 수 있습니다. 이중 Active 상태의 인증서(SHA-2)를 선택 하여 Action을 클릭 하여 인증서를 다운 받습니다.   
<img src="/images/image32.png" width="90%" height="90%">    

인증서는 okta.cert 로 받아집니다.

Okta의 추가 정보를 얻기 위해 인증서의 Meta 정보를 클릭 합니다.    
<img src="/images/image33.png" width="80%" height="80%">    

Meta 정보 페이지에서 entityID 와 SingleSignOneServie 의 Location 주소를 메모 하여 줍니다.    
<img src="/images/image34.png" width="90%" height="90%">   

### MongoDB Atlas 설정
Atlas 계정은 이메일 형태로 구성되며 로그인시 이메일의 도메인을 이용하여 로그인 방법이 지정 됩니다. 따라서 이메일의 도메인에 대해 관리 권한을 가지고 있어야 합니다. 도메인은 nosql.site를 이용할 것이며 사용자 계정은 ***@nosql.site 가 됩니다.

Atlas 에 관리자 권한으로 로그인 합니다.    
로그인 후 관리자 페이지로 이동 합니다. (상단의 톱니 아이콘 클릭)    
<img src="/images/image07.png" width="90%" height="90%">      

관리자 페이지에서 Settings 에 Setup Federation Settings 의 Visit Federation Management App 를 클릭 합니다.
<img src="/images/image08.png" width="90%" height="90%">      

#### Domain 설정
Domain 탭에서 Add Domains 를 클릭 합니다.   
<img src="/images/image20.png" width="85%" height="85%">    

이름과 도메인 명을 입력 하여 줍니다.
<img src="/images/image21.png" width="85%" height="85%">    

도메인 소유 방법을 증명 하기 위한 방법을 선택 합니다. 별도 웹서버 없이 도메인관리 툴에서 지정된 텍스트를 도메인에 등록 하는 방법을 선택 합니다. (도메인 웹서버가 있는 경우 지정된 html 을 올려서 할 수도 있습니다)
<img src="/images/image22.png" width="90%" height="90%">    

도메인 소유를 증명하기 위해 임의의 텍스트 스트링이 생성 되며 이를 도메인에 Txt 레코드로 등록 하여야 합니다.

<img src="/images/image23.png" width="90%" height="90%">    

GoDaddy 에서 구매한 도메인으로 해당 사이트에서 DNS 레코드를 등록 할 수 있습니다.  
godaddy.com 에 로그인 후 소유 도메인(nosql.site)에서 DNS 관리를 선택 합니다.
<img src="/images/image24.png" width="80%" height="80%">    

추가 버튼을 클릭 후 record type 을 txt 로 선택 합니다.    
값 필드에 Atlas 에서 생성된 임의 스트링 값을 입력 하여 줍니다.    
<img src="/images/image25.png" width="80%" height="80%">    


TTL 은 최소 시간인 30분을 선택 하고 저장 합니다. 도메인이 적용 되는 시간은 최대 1시간 까지 걸립니다.
Atlas 페이지에서 Finish 버튼을 클릭 합니다.    
<img src="/images/image26.png" width="90%" height="90%">      

등록된 도메인이 확인 되지 않은 상태로 있으며 몇 분 후 verfiy 버튼을 클릭 합니다.
<img src="/images/image27.png" width="90%" height="90%">    

확인이 완료 되면 Verified 상태가 되며 등록이 완료 됩니다.
<img src="/images/image28.png" width="90%" height="90%">    

(Domain 에 등록한 Txt record 는 삭제 하여 줍니다.)

#### Identity Provider 등록
OKTA 정보(Identity Provider)를 등록 하여 줍니다.
Identity Providers 탭을 클릭 후 Setup Identity Provider 를 클릭 합니다.
<img src="/images/image40.png" width="90%" height="90%">    

IDP 이름을 입력하고 Issue URI 을 입력 하여 줍니다.
(이전 작업에서 기록한 EntityID를 입력 합니다.)
Single Sign-On URL은 Okta 의 Meta 데이터의 SingleSignOneServie 의 Location 주소를 입력 하여 줍니다.
<img src="/images/image42.png" width="90%" height="90%">    

인증서를 업로드는 Alternatively, paste the contents of the certifiacte directly 를 클릭 하여 줍니다.
다운받은 인증서 okta.cert을 오픈하여 전체 내용을 붙여 넣어 줍니다.
<img src="/images/image43.png" width="90%" height="90%">    

Request Binding 은 Http Post를 선택 하고 Response Signature Algorithm 은 SHA-256을 선택 하여 줍니다.
<img src="/images/image44.png" width="50%" height="50%">    

완료 버튼을 클릭하면 Atlas가 제공하는 SAML 의 Meta 정보를 받을 수 있는 페이지가 오픈 됩니다.
필요한 정보는 Assertion Consumer Service URL과 Audience URI 로 이를 복사하여 줍니다.
<img src="/images/image50.png" width="80%" height="80%">    

관련 도메인과 조직에 대한 정보를 입력 하여 줍니다.
<img src="/images/image46.png" width="75%" height="75%">    
버튼을 클릭 하면 기존에 입력 해준 도메인 및 Organization을 볼 수 있습니다. 알맞은 내용을 선택 하여 줍니다.    


#### Service Provider 정보 등록
최종 정보를 업데이트 하기 위해 OKTA 의 관리자 페이지로 이동 합니다.
<img src="/images/image51.png" width="90%" height="90%">  
MongoDB Atlas-SSO를 클릭 후 상단 메뉴 중 Sign On 탭을 선택 합니다.
<img src="/images/image52.png" width="90%" height="90%">  

Default Relay State 를 http://cloud.mongodb.com로 입력 하여 주며 Advanced Sign-on settings 에 Atlas 페이지의 Assertion Consumer Service URL 과 Audience URI를 각각 입력 하여 줍니다.
또한 아이디가 Email 형태로 전달 됨으로 username format 을 Email 로 선택 하여 줍니다.
<img src="/images/image53.png" width="80%" height="80%">  

데이터를 저장 하여 후에 상태가 Active 상태 인지 확인 합니다. (Deactive 상태인 경우 Active 로 변경)
<img src="/images/image54.png" width="70%" height="70%">  

### Single Sign On Test
Okta 에 사용자를 추가 하여 테스트 합니다.
<img src="/images/image90.png" width="90%" height="90%">    

Okta 에 Directory/People 에서 Add Person 으로 사용자를 추가 합니다. (사용자 ID가 반드시 이메일 형식으로 @nosql.site 여야 합니다.)    


<img src="/images/image91.png" width="90%" height="90%">    

생성된 사용자를 조회 한 후 Application (MOngoDB Atlas-SSO)를 추가해 줍니다.   
<img src="/images/image93.png" width="90%" height="90%">    


#### SP Initiate Single Sign On

MongoDB Atlas-SSO를 검색 후 Assign 하여 줍니다.
<img src="/images/image94.png" width="90%" height="90%">    


MongoDB Atlas 에 로그인을 위해 접속 합니다. 사용자는 입력한 사용자 atlas@nosql.site 를 입력합니다.
<img src="/images/image95.png" width="90%" height="90%">    

Single Sign On 설정에 따라 Okta 로그인 페이지로 이동 합니다. 로그인을 위해 사용자 ID를 입력 합니다.
<img src="/images/image96.png" width="90%" height="90%">   

추가 인증 (MFA)와 패스워드 인증이 완료 되면 Single Sign On 을 통해 추가 인증 없이 Atlas Console 로 이동 됩니다.

<img src="/images/image101.png" width="90%" height="90%">   

#### IDP Initiate Single Sign On

인증 테스트를 위해 Atlas를 로그 아웃 하고 New Private window 혹은 New Incognito window 로 브라우저를 오픈 합니다.
오픈된 브라우저에서 okta 로 직접 로그인 합니다.

<img src="/images/image99.png" width="90%" height="90%">   

인증이 완료 되면 접근 가능한 애플리케이션 페이지가 오픈됩니다. 테스트를 위해 MongoDB Atlas-SSO를 클릭 합니다.
<img src="/images/image100.png" width="90%" height="90%">   

로그인 된 상태로 Atlas에 접근 하는 것을 확인 할 수 있습니다.
<img src="/images/image102.png" width="90%" height="90%">   
