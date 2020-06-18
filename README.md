# api.sms
Tài liệu hướng dẫn kết nối gửi SMS (OTP) qua automodem.net
```
Lưu ý: Bạn cần tạo tài khoản ĐẠI LÝ tại trang [naptiendientu.com](https://naptiendientu.com) 
để lấy API TOKEN và nạp tiền gửi tin nhắn.
```

### ENDPOINT
```
https://automodem.net:12368/sms/v1.1
```

### HEADER PARAMETERS
```
Content-Type: application/json
Authorization: $API_TOKEN
```

### PAYLOAD
```ruby
{
  sender: "SENDER_PHONE",
  phoneNumber: "PHONE_NUMBER",
  message: "SMS_BODY",  
}
```

### RESPONSE
* HTTP CODE 200: Success
```ruby
{
  messageId: NUMBER,
}
```
* HTTP CODE 400: Bad request
* HTTP CODE 401: Unauthorized
* HTTP CODE 402: Payment Required

### JAVA EXAMPLE (USING APACHE HTTP CLIENT)
```ruby
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

public class AutomodemSendSmsExample {

    public static void main(String[] args) {

        String AUTOMODEM_ENPOINT = "https://automodem.net:12368/sms/v1.1";
        String API_TOKEN = "AUTHORIZATION_TOKEN";
        String SENDER = "0912345678";
        String PHONE_NUMBER = "0987654321";
        String MESSAGE = "Hello from automodem.net";

        HttpPost httpPost = null;

        try {

            CloseableHttpClient httpClient = HttpClients.createDefault();

            // create request
            String msgPayloadJSON =
                    "{"
                        + "\"sender\": \"" + SENDER + "\", "
                        + "\"phoneNumber\": \"" + PHONE_NUMBER + "\", "
                        + "\"message\": \"" + MESSAGE + "\", "
                    + "}";

            httpPost = new HttpPost(AUTOMODEM_ENPOINT);
            httpPost.setEntity(new StringEntity(msgPayloadJSON, "UTF-8"));
            httpPost.setHeader("Content-Type", "application/json");
            httpPost.addHeader("Authorization", API_TOKEN);

            // execute message
            HttpResponse httpResponse = httpClient.execute(httpPost);
            int statusCode = httpResponse.getStatusLine().getStatusCode();
            if(statusCode == 200) {
                String response = EntityUtils.toString(httpResponse.getEntity());
                System.out.println("SMS sent. Response = " + response);
            } else {
                System.out.println("Send SMS error with code: " + statusCode);
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // Release the connection.
            if(httpPost != null) {
                httpPost.releaseConnection();
                httpPost = null;
            }
        }
    }

}
```
