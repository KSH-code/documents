# 기본 발신번호 설정

해당 발신번호의 handle\_key를 data에 넣으신후 postRequest\('set\_default', data, 'senderid'\)를 요청하시면 됩니다.

```text
uses
  System.Json, coolsms in '..\..\coolsms.pas', https in '..\..\https.pas', System.SysUtils, Classes;
var
  coolsms: resource;
  data: TStringList;
  jsonObject: TJSONObject;
begin
  try
    // http://www.coolsms.co.kr/SenderID_API#POSTset_default 참조
    // api_key, api_secret 설정
    coolsms := resource.Create('NCS55882FB7DE511A', '4FB5FF82B9AB7D0E0AEB840D403DE0F74');
    // parameters
    data := TStringList.create;
    data.Values['handle_key'] := 'SIDFFFFF614ED3'; // register 호출 후 리턴받은 핸들값
    // default 발신번호등록 요청
    jsonObject := coolsms.setDefault(data);
    if strToBool(jsonObject.GetValue('status').ToString) = TRUE then
    begin
      Writeln('성공');
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

