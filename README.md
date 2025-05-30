# decode-window-31j

```
if (oSession.oResponse.headers.ExistsAndContains("Content-Type", "Windows-31J")
            || oSession.oResponse.headers.ExistsAndContains("charset", "Windows-31J") 
            || oSession.oResponse.headers.ExistsAndContains("Content-Type", "shift_jis") 
        || oSession.oResponse.headers.ExistsAndContains("Content-Encoding", "Window 31-J")) {
            oSession.utilDecodeResponse(); // GZip 등 압축 해제
            try {
                var sjisEncoding = System.Text.Encoding.GetEncoding("shift_jis");
                var bodyBytes = oSession.responseBodyBytes;
                var decodedText = sjisEncoding.GetString(bodyBytes);
                // 원한다면 인코딩을 UTF-8로 변환해 다시 저장 가능
                oSession.utilSetResponseBody(decodedText);
                // Content-Type도 적절히 수정
                oSession.oResponse["Content-Type"] = "text/html; charset=UTF-8";
                // UI 표시용
                oSession["ui-color"] = "green";
                oSession["ui-comments"] = "Shift_JIS decoded and converted to UTF-8";
            } catch (ex) {
         oSession["ui-color"] = "red";
                oSession["ui-comments"] = "Shift_JIS decoding error: " + ex.message;
            }
        }
```

