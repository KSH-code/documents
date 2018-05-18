### SMS발송

uses에 coolsms.pas를 적용 시켜준뒤 setApiKey를 통해 api_key와 api_secret를 설정해 준 뒤 data에 전송정보들을 넣어서 postRequest('send', data, 'sms')로 전송요청을 합니다.

```delphi
uses
  System.Json, coolsms in '..\..\coolsms.pas', https in '..\..\https.pas', System.SysUtils, Classes;
var
  coolsms: resource;
  jsonObject: TJSONObject;
  data: TStringList;
begin
  try
    // api_key, api_secret 설정
    coolsms := resource.Create('NCS55882FB7DE511A', '4FB5FF82B9AB7D0E0AEB840D403DE0F74');
    // parameters
    data := TStringList.create;
    data.Values['to'] := '01000000000'; // 수신번호    
    * 10월 16일 이후로 발신번호 사전등록제로 인해 등록된 발신번호로만 문자를 보내실 수 있습니다. 바로가기    
    data.Values['from'] := '029302266'; // 발신번호    
    data.Values['text'] := 'test'; // 문자내용    
    data.Values['type'] := 'mms';  // 문자타입 sms, mms, lms    
    data.Values['image'] := '..\..\test.jpg'; // IMAGE 파일 (MMS일경우)
    // 그외 parameters, http://www.coolsms.co.kr/SMS_API#POSTsend 참조
    {
      data.Values['image_encoding']
          이미지 인코딩 방식 binary(Default), base64
      data.Values['refname']
          참조내용(이름)
      data.Values['country']
        한국: 82, 일본: 81, 중국: 86, 미국: 1, 기타 등등 (기본 한국)
        http://countrycode.org 참고
      data.Values['datetime']
          예약시간을 YYYYMMDDHHMISS 포맷으로 입력 (입력 없거나 지난날짜를 입력하면 바로 전송) 예) 20131216090510 (2013년 12월 16일 9시 5분 10초에 발송되도록 예약)
      data.Values['subject']
          LMS, MMS 일때 제목 (40바이트)
      data.Values['charset']
          한글 인코딩 방식 지정 유니코드 UTF-8 일 경우 utf8 완성형 한글(EUC-KR) 일 경우 euckr 으로 입력 입력 없으면 utf8가 기본
      data.Values['srk']
          솔루션 제공 수수료를 정산받을 솔루션 등록키
      data.Values['mode']
          test로 입력할 경우 CARRIER 시뮬레이터로 시뮬레이션됨 수신번호를 반드시 01000000000 으로 테스트하세요. 예약필드 datetime는 무시됨 결과값은 60 잔액에서 실제 차감되며 다음날 새벽에 재충전됨
      data.Values['delay']
          0~20 사이의 값으로 전송지연 시간을 줄 수 있음, 1은 약 1초 (기본값은 0)
      data.Values['force_sms']
        누리고푸시를 사용하더라도 SMS로 발송되도록 강제
        true 혹은 false(기본)
      data.Values['os_platform']
        클라이언트의 OS 및 플랫폼 버전) CentOS 6.6
        (v1.5에서 추가됨)
      data.Values['dev_lang']
        개발 프로그래밍 언어 예) PHP 5.3.3
        (v1.5에서 추가됨)
      data.Values['sdk_version']
        SDK 버전 예) PHP SDK 1.5
        (v1.5에서 추가됨)
      data.Values['app_version']
        어플리케이션 버전 예) Purplebook 4.1
        (v1.5에서 추가됨)
      data.Values['extension']
          JSON 포맷의 개별 메시지를 담을 수 있음
    }
    // send Messages
    jsonObject := coolsms.send(data);
    if strToBool(jsonObject.GetValue('status').ToString) = TRUE then
    begin
      Writeln('성공');
      Writeln('group_id : ' + jsonObject.Get('group_id').JsonValue.ToString);
      Writeln('success_count : ' + jsonObject.Get('success_count').JsonValue.ToString);
      Writeln('error_count : ' + jsonObject.Get('error_count').JsonValue.ToString);
      Writeln('result_code : ' + jsonObject.Get('result_code').JsonValue.ToString);
      Writeln('result_message : ' + jsonObject.Get('result_message').JsonValue.ToString);
    end
    else
    begin
      Writeln('실패');
      if jsonObject.Get('code').Equals(Nil) = FALSE then Writeln('code : ' + jsonObject.Get('code').JsonValue.ToString);
      if jsonObject.Get('message').Equals(Nil) = FALSE then Writeln('message : ' + jsonObject.Get('message').JsonValue.ToString);
    end;
    jsonObject.Free;
    Writeln('-----------------------------------------');
    Writeln('Press <enter> to quit...');
    Readln;
  except
    on E: Exception do
      Writeln(E.ClassName, ': ', E.Message);
  end;
end.
```