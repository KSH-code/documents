### 발송내역

data에 mid나 gid를 입력하신후 request('sent', data, 'sms')를 통해 발송내역을 가져옵니다.
Return 값은 jsonObject 안에 jsonArray로 받습니다.

```delphi
uses
  System.Json, coolsms in '..\..\coolsms.pas', https in '..\..\https.pas', System.SysUtils, Classes;
var
  coolsms: resource;
  data: TStringList;
  jsonObject: TJSONObject;
  jsonArray: TJSONArray;
  jsonObjectData: TJSONObject;
  i: integer;
begin
  try
    // api_key, api_secret 설정
    coolsms := resource.Create('NCS55882FB7DE511A', '4FB5FF82B9AB7D0E0AEB840D403DE0F74');
    //https.create('NCS55882FB7DE511A', '4FB5FF82B9AB7D0E0AEB840D403DE0F74');
    // parameters, data 설정 mid나 gid 둘 중 하나는 들어가야 합니다.
    data := TStringList.create;
    data.Values['mid'] := 'R1M55B1FFB27E5308'; // 메시지ID
    // 그외 parameters, http://www.https.co.kr/SMS_API#GETsent 참조
    {
        data.Values['count']
          기본값 20이며 20개의 목록을 받을 수 있음. 40입력시 40개의 목록이 리턴
      data.Values['page']
          1부터 시작하는 페이지값
      data.Values['rcpt']
          수신번호로 검색
      data.Values['start']
          검색 시작일시 접수 날짜와 시간으로 검색 YYYY-MM-DD HH:MI:SS 포맷의 날짜와 시간
      data.Values['end']
          검색 종료일시 접수 날짜와 시간으로 검색 YYYY-MM-DD HH:MI:SS 포맷의 날짜와 시간
      data.Values['status']
          메시지 상태 값으로 검색
      data.Values['resultcode']
          전송결과 값으로 검색
      data.Values['notin_resultcode']
          입력된 전송결과 값 이외의 건들만 조회
      data.Values['gid']
          그룹ID
    }
    // sent Messages
    jsonObject := coolsms.sent(data);
    if strToBool(jsonObject.GetValue('status').ToString) = TRUE then
    begin
      Writeln('성공');
      jsonArray := JsonObject.Get('data').JsonValue as TJSONArray;
      Writeln('total_count : ' + jsonObject.Get('total_count').JsonValue.ToString);
      Writeln('list_count : ' + jsonObject.Get('list_count').JsonValue.ToString);
      Writeln('page : ' + jsonObject.Get('page').JsonValue.ToString);
      for i := 0 to jsonArray.Size - 1 do
      begin
          jsonObjectData := TJSONObject.Create;
          jsonObjectData := jsonArray.Get(i) as TJSONObject;
          Writeln('-----------------------------------------');
          Writeln('type : ' + jsonObjectData.Get('type').JsonValue.ToString);
          Writeln('recipient_number : ' + jsonObjectData.Get('recipient_number').JsonValue.ToString);
          Writeln('text : ' + jsonObjectData.Get('text').JsonValue.ToString);
          Writeln('carrier : ' + jsonObjectData.Get('carrier').JsonValue.ToString);
          Writeln('status : ' + jsonObjectData.Get('status').JsonValue.ToString);
          Writeln('result_code : ' + jsonObjectData.Get('result_code').JsonValue.ToString);
          Writeln('result_message : ' + jsonObjectData.Get('result_message').JsonValue.ToString);
          Writeln('message_id : ' + jsonObjectData.Get('message_id').JsonValue.ToString);
          Writeln('group_id : ' + jsonObjectData.Get('group_id').JsonValue.ToString);
          Writeln('sent_time : ' + jsonObjectData.Get('sent_time').JsonValue.ToString);
          Writeln('accepted_time : ' + jsonObjectData.Get('accepted_time').JsonValue.ToString);
      end;
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