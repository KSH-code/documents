### 발신번호 등록 요청

관련 정보는 http://www.coolsms.co.kr/SenderID_API를 참고해주세요.
postRequest('register', data, 'senderid')로 요청을 해서 넘어오는 ars_number로 전화를 걸어주시면 됩니다.

```delphi
uses
  System.Json, coolsms in '..\..\coolsms.pas', https in '..\..\https.pas', System.SysUtils, Classes;
var
  coolsms: resource;
  data: TStringList;
  jsonObject: TJSONObject;
begin
  try
    // http://www.coolsms.co.kr/SenderID_API#POSTregister 참조
    // api_key, api_secret 설정
    coolsms := resource.Create('NCS55882FB7DE511A', '4FB5FF82B9AB7D0E0AEB840D403DE0F74');
    // parameters
    data := TStringList.create;
    data.Values['phone'] := '01090683469'; // 발신번호로 등록할 전화번호를 입력해주세요.
    // 발신번호등록 요청
    jsonObject := coolsms.register(data);
    if strToBool(jsonObject.GetValue('status').ToString) = TRUE then
    begin
      Writeln('성공');
      Writeln('handle_key : ' + jsonObject.Get('handle_key').JsonValue.ToString);
      Writeln('ars_number : ' + jsonObject.Get('ars_number').JsonValue.ToString);
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