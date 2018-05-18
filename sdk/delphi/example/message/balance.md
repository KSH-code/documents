### 잔액정보

request('balance', Nil, 'sms')를 통해 잔액정보를 가져옵니다.

```delphi
uses
  System.Json, coolsms in '..\..\coolsms.pas', https in '..\..\https.pas', System.SysUtils;
var
  coolsms: resource;
  jsonObject: TJSONObject;
begin
  try
    // api_key, api_secret 설정
    coolsms := resource.Create('NCS55882FB7DE511A', '4FB5FF82B9AB7D0E0AEB840D403DE0F74');
    // 남은잔액 요청, http://www.coolsms.co.kr/SMS_API#GETbalance 참조
    jsonObject := coolsms.balance();
    if strToBool(jsonObject.GetValue('status').ToString) = TRUE then
    begin
      Writeln('성공');
      Writeln('cash : ' + jsonObject.Get('cash').JsonValue.ToString);
      Writeln('point : ' + jsonObject.Get('point').JsonValue.ToString);
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