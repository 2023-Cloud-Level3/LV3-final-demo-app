<style>
.burk {
    background-color: red;
    color: yellow;
    display:inline-block;
}
</style>


# Amazon IVS with CloudFront

IVS: Amazon Interactive Video Service

정리 자료 원본: 콘텐츠 스트리밍 with CloudFront.pptx

강사 제공 자료
- 2023-06-12 PPT를 따라하면서 정리 했음

## 1. IVS 환경설정하기

### 1.1 채널 만들기

1. Amazon Interactive Video Service 접속
2. 왼쪽  Video > Channels
   - 'Create channel'
3. 기본 정보 입력
   - Channel name: user07-ivs
   - 'Create Channel'
4. 정보 저장:Stream configurationInfo
   - 아래 2개 항목 메모장 복사 
   - Stream key: sk_ap-northeast-2_1.................
   - Ingest server: rtmps://12c93fa046aa.global-contribute.live-video.net:443/app/

   -  Playback URL: https://12c93fa046aa.ap-northeast-2.playback.live-video.net/api/video/v1/ap-northeast-2.034677339045.channel.iv60RkkusMld.m3u8
     - vod_sample.html에서 테스트 

### 1.2 OBS 설정
OBS는 Open Broadcaster Software의 약자이다. 
- 방송 보조 및 동영상 캡처(녹화) 등 인터넷 방송을 위한 기능을 제공하는 오픈 소스
- 미 설치되었으면 설치 : https://obsproject.com/ko

1. 서버 설정
   - File > Settings > Stream
     - Service: Customer ..
     - Server: 위 의에서 복사한  Ingest Server 
     - Stream Key: 위 의에서 복사한  Stream Key
2. Sources에 비디오 캡처 장치(Video Capture Device) 추가
   
### 브라우저에서 확인하기
1. vod_sample.html
   - 2 곳 URL 수정   
    ```html
   <html>
     <body>
       <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
       <video id="video" controls></video>
       <script>
       if(Hls.isSupported())
       {
           var video = document.getElementById('video');
           var hls = new Hls();
           <!-- hls.loadSource('요기에 URL 수정')           -->
           hls.loadSource('https://12c93fa046aa.ap-northeast-2.playback.live-video.net/api/video/v1/ap-northeast-2.034677339045.channel.V3OoggURlZxY.m3u8');
           hls.attachMedia(video);
           hls.on(Hls.Events.MANIFEST_PARSED,function()
           {
               video.play();
           });
       }
       else if (video.canPlayType('application/vnd.apple.mpegurl'))
       {
           <!-- hls.loadSource('요기에 URL 수정')           -->
           video.src = 'https://12c93fa046aa.ap-northeast-2.playback.live-video.net/api/video/v1/ap-northeast-2.034677339045.channel.V3OoggURlZxY.m3u8';
           video.addEventListener('canplay',function()
           {
               video.play();
           });
       }
       </script>
     </body>
   </html>
   ```

2. 위 URL 부라우저에서 실행
   동영상이 보여진다 

3. 다른 테스트
   - JWPlayer에서 간단히 테스트
   - JWPLAYER를 통해 웹상에서 라이브 스트리밍 테스트:  https://developer-tools.jwplayer.com/stream-tester
   - HLS Stream URL에   Playback URL을 입력하면 웹에서 보여진다
                                                               

### 고가용성 웹서버 구축 후 라이브 스트리밍
 PPT 15 Page 이 이후는 아직 미 실
 
## 2. AWS 사용 설명서 정리
https://docs.aws.amazon.com/ko_kr/ivs/latest/userguide/getting-started-create-account.html
Amazon IVS 시작하기
1. AWS 계정 생성
2. 루트 및 관리 사용자 설정
3. IAM 권한 설정
4. 선택적 레코딩을 사용하여 채널 생성
5. 스트리밍 소프트웨어 설정
6. 라이브 스트림 보기
7. 서비스 할당량 한도 확인(선택 사항)
8. 레코딩을 비활성화하는 방법

### 2.1 AWS 계정 생성
 기존 계정으로 ..(Root이외)
### 2.2 루트 및 관리 사용자 설정

### 2.3 IAM 권한 설정
IVS을 위한 별도의 권한 생성 필요

1. AWS IAM 콘솔로 이동

2. 탐색 창에서 정책을 선택
   - Create policy(정책 생성)
3. JSON 탭을 선택하고 JSON 탭에 다음 IVS 정책을 붙여 넣습니다. 

    ```json
    {
       "Version": "2012-10-17",
       "Statement": [
          {
             "Effect": "Allow",
             "Action": [
                "ivs:CreateChannel",
                "ivs:CreateRecordingConfiguration",
                "ivs:GetChannel",
                "ivs:GetRecordingConfiguration",
                "ivs:GetStream",
                "ivs:GetStreamKey",
                "ivs:GetStreamSession",
                "ivs:ListChannels",
                "ivs:ListRecordingConfigurations",
                "ivs:ListStreamKeys",
                "ivs:ListStreams",
                "ivs:ListStreamSessions"
              ],
              "Resource": "*"
          },
          {
             "Effect": "Allow",
             "Action": [
                "cloudwatch:DescribeAlarms",
                "cloudwatch:GetMetricData",
                "s3:CreateBucket",
                "s3:GetBucketLocation",
                "s3:ListAllMyBuckets",
                "servicequotas:ListAWSDefaultServiceQuotas",
                "servicequotas:ListRequestedServiceQuotaChangeHistoryByQuota",
                "servicequotas:ListServiceQuotas",
                "servicequotas:ListServices",
                "servicequotas:ListTagsForResource"
             ],
             "Resource": "*"
          },
          {
             "Effect": "Allow",
             "Action": [
                "iam:AttachRolePolicy",
                "iam:CreateServiceLinkedRole",
                "iam:PutRolePolicy"
             ],
             "Resource": 
    "arn:aws:iam::*:role/aws-service-role/ivs.amazonaws.com/AWSServiceRoleForIVSRecordToS3*"
          },
          {
            "Effect": "Allow",
            "Action": [
              "cloudwatch:GetMetricData"
            ],
            "Resource": "*"
          }
       ]
    }
    ```
4. 정책이름: user07-ivs
5. 생성
#### 기존 사용자에 권한 추가

1. IAM 콘솔을 엽니다.

2. 탐색 창에서 사용자(Users)를 선택
   - 사용자 이름을 선택합니다. (이름을 클릭하여 선택합니다. 선택 상자는 선택하지 마세요.)

   - 요약(Summary) 페이지의 권한(Permissions) 탭에서 권한 추가(Add Permissions)를 선택합니다.

   - Attach existing policies directly
   - 이전에 생성한 "user07-ivs" 추가

   - Add permissions


### 4. 선택적 레코딩을 사용하여 채널 생성
- Amazon S3에 자동 레코딩
- 콘솔 지침 ==> 이 부분만 따라하기 했음
- CLI 지침

### 4.1 콘솔 지침
초기 채널 설정

1. Amazon IVS 콘솔을 엽니다.

2. 탐색 모음에서 [리전 선택(Select a Region)] 드롭다운을 클릭하여 리전을 선택합니다. 이 리전에서 새 채널이 생성됩니다.

3. 채널 생성(Create Channel)]을 선택

4. [채널 구성(Channel configuration)]에서 [기본 구성(Default configuration)]을 승인
5. 채널 이름(Channel name)] = user07-ivs


### 5. 스트리밍 소프트웨어 설정
### OBS Studio에서 스트리밍

(OBS Studio)는 레코딩 및 라이브 스트리밍을 위한 무료 오픈 소스 소프트웨어 제품군입니다. OBS는 실시간 소스 및 디바이스 캡처, 화면 구성, 인코딩, 레코딩 및 스트리밍을 제공합니다.


1. 소프트웨어를 다운로드하여 설치합니다. https://obsproject.com/download.
   - 자동 구성 마법사(Auto-Configuration Wizard)를 실행

2. File > Settings > Stream
   - Service: Custom..
   - Server: 이전 IVS의 Server (rtmps://a1b2c3d4e5f6.global-contribute.live-video.net:443/app/)
   - Stream Key: 이전 IVS의 Key (sk_us-west-2_abcd1234efgh5678ijkl)

3. 비디오 출력 해상도(Video Output Resolution)와 비트레이트(Bitrate)는 Amazon IVS 스트리밍 구성의 채널 유형을 참조하세요. OBS 마법사에서 선택한 값 중 하나가 Amazon IVS에서 허용하는 값을 초과하는 경우 Amazon IVS 연결이 실패하지 않도록 수동으로 값을 조정해야 합니다. 마법사가 완료된 후 다음을 수행합니다.
4. 비디오 해상도를 조정하려면 설정(Settings) > 비디오(Video) > 출력(조정) 해상도(Output (Scaled) Resolution)를 사용합니다.

5. 비디오 비트레이트를 조정하려면 설정(Settings) > 출력(Output) > 스트리밍(Streaming) > 비디오 비트레이트(Video Bitrate)를 사용합니다.

6. 스트림 안정성 개선과 뷰어 재생에서 버퍼링 방지를 위해 2초 키프레임 간격(Keyframe Interval)이 권장됩니다. 마법사가 완료되면 설정(Settings) > 출력(Output) > 출력 모드(Output Mode)로 이동하여 고급(Advanced)을 선택하고 스트리밍(Streaming) 탭에서 키프레임 간격(Keyframe Interval)이 2인지 확인합니다.

7. OBS Studio 기본 창에서 [스트리밍 시작(Start Streaming)]을 선택합니다.

### 6. 라이브 스트림 보기

1. Amazon IVS 콘솔을 엽니다.

2. 탐색 창에서 라이브 채널(Live channels)을 선택합니다. (탐색 창이 축소된 경우 먼저 햄버거 아이콘을 선택하여 엽니다.)

3. 스트림을 보려는 채널을 선택하여 해당 채널의 세부 정보 페이지로 이동합니다.

4. 라이브 스트림이 페이지의 [라이브 스트림(Live stream)] 섹션에서 재생됩니다.
### 7. 서비스 할당량 한도 확인(선택 사항)
### 8. 레코딩을 비활성화하는 방법
