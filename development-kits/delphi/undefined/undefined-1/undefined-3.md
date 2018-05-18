# 발신번호 목록 보기

request\('list', Nil, 'senderid'\)를 요청하시면 됩니다. Return값은 jsonObject안에 jsonArray로 처리해주세요.

```text
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
    // http://www.coolsms.co.kr/SenderID_API#GETlist 참조
    // api_key, api_secret 설정
    coolsms := resource.Create('NCS55882FB7DE511A', '4FB5FF82B9AB7D0E0AEB840D403DE0F74');
    // 발신번호 리스트 요청
    jsonObject := coolsms.list();
    if strToBool(jsonObject.GetValue('status').ToString) = TRUE then
    begin
      Writeln('성공');
      jsonArray := JsonObject.Get('data').JsonValue as TJSONArray;
      for i := 0 to jsonArray.Size - 1 do
      begin
          jsonObjectData := TJSONObject.Create;
          jsonObjectData := jsonArray.Get(i) as TJSONObject;
          Writeln('-----------------------------------------');
          Writeln('idno : ' + jsonObjectData.Get('idno').JsonValue.ToString);
          Writeln('phone_number : ' + jsonObjectData.Get('phone_number').JsonValue.ToString);
          Writeln('flag_default : ' + jsonObjectData.Get('flag_default').JsonValue.ToString);
          Writeln('updatetime : ' + jsonObjectData.Get('updatetime').JsonValue.ToString);
          Writeln('regdate : ' + jsonObjectData.Get('regdate').JsonValue.ToString);
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

