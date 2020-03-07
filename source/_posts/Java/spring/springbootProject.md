---
title: springboot 实战经验
tags:
- springboot
---

# QQ互联登录

QQ互联官网: https://connect.qq.com/

<!--more-->	

## 前期准备

1. 开发者认证

![](applyForDeveloper.png)

2. 创建应用

![](applicatinCreate.png)

![](dateCreate.png)

![](websiteAdd.png)

## 流程

1. 生成code链接, 获取授权码

2. 使用获取的授权码, 获取对应的accessToken

3. 使用accessToken获取用户的openId

4. 使用openId获取用户的信息

## 访问流程的链接及返回结果

1. 获取Authorization Code

请求地址: https://graph.qq.com/oauth2.0/authorize

参数:

+ response_type: 必须, 授权类型， 此值固定为"code".
+ client_id: 必须, 申请QQ登录成功后, 分配给应用的appid.
+ redirect_uri: 必须, 成功授权后的回调地址
+ state: 必须, client端的状态值.
+ scope: 可选, 请求用户授权时向用户显示的可进行授权的列表. 如果要填写多个接口名称, 请用逗号隔开. 例如: scope=get_user_info,list_album,upload_pic,do_like. 不传则默认请求对接口get_user_info进行授权.
+ display: 可选, 仅PC网站接入时使用. 用于展示的样式. 不传则默认展示为PC下的样式. 如果传入"mobile", 则展示为mobile端下的样式.

访问链接: https://graph.qq.com/oauth2.0/authorize?response_type=code&client_id=123456789&redirect_uri=https://example.com/callback&state=123

返回链接及参数: https://example.com/callback?code=7241538976&state=123

2. 通过Authorization Code获取Access Token

请求地址: https://graph.qq.com/oauth2.0/token

+ grant_type: 必须, 授权类型, 在本步骤中, 此值为"authorization_code'.
+ client_id: 必须, 申请QQ登录成功后, 分配给网站的appid.
+ client_secret: 必须, 申请QQ登录成功后, 分配给网站的appkey.
+ code: 必须, 上一步返回的authorization code. 如果用户成功登录并授权, 则会跳转到指定的回调地址, 并在URL中带上Authorization Code
+ redirect_uri: 必须, 与上面一步中传入的redirect_uri保持一致.

访问链接: https://graph.qq.com/oauth2.0/token?grant_type=authorization_code&client_id=123456789&client_secret=1234567890asdf&code=7241538976&redirect_uri=https://example.com/callback

返回: access_token=E1860D7C46FFA&expires_in=7776000&refresh_token=D72D2CB0CBA627A07B1

+ access_token: 授权令牌, Access_Token.
+ expires_in: 该access token的有效期, 单位为秒.
+ refresh_token: 在授权自动续期步骤中, 获取新的Access_Token时需要提供的参数.
注：refresh_token仅一次有效

3. 获取用户OpenID

请求地址: https://graph.qq.com/oauth2.0/me

+ access_token: 必须, 获取到的access token.

访问链接: https://graph.qq.com/oauth2.0/me?access_token=E1860D7C46FFA

返回: callback( {"client_id":"123456789","openid":"BEC4933D"} );

4. OpenAPI调用

请求地址: https://graph.qq.com/user/get_user_info

+ access_token: 获取到的access token.
+ oauth_consumer_key: 申请QQ登录成功后, 分配给应用的appid.
+ openid: 用户的ID, 与QQ号码一一对应.

访问链接: https://graph.qq.com/user/get_user_info?access_token=E1860D7C46FFA&oauth_consumer_key=123456789&openid=BEC4933D

返回:
```json
{
    "ret": 0,
    "msg": "",
    "nickname": "张三",
    "gender": "男",
    "province": "湖南",
    "city": "长沙",
    "year": "1986",
    "figureurl": "http:/qzapp.qlogo.cn/qzaC45874BEACB832DD4FC"
}
```

## java代码实现

前端放置一个图标并设置a标签链接

此处redirect_uri使用了URLEncode进行编码
http://www.example.com/callback

```html
<a href="https://graph.qq.com/oauth2.0/authorize?response_type=code&client_id=123456789&redirect_uri=http%3A%2F%2Fwww.example.com%2Fcallback&state=123">
    <img class="qq-btn" src="http://qzonestyle.gtimg.cn/qzone/vas/opensns/res/img/Connect_logo_3.png"/>
</a>
```

我使用了两种方式进行qq的登录请求

一种是通过手动拼接url然后用okHttp来进行get请求,
另一种是直接使用qq提供的方法来进行请求

### 方式一: 拼接url, 通过okhttp进行get请求

在application.properties中编写几个必要的参数

```properties
app_ID=123456789
app_KEY=1234567890asdf
redirect_URI=http%3A%2F%2Fwww.example.com%2Fcallback
```

引入依赖

```xml
<dependency>
    <groupId>com.squareup.okhttp3</groupId>
    <artifactId>okhttp</artifactId>
    <version>4.4.0</version>
</dependency>
```

编写一个工具类进行qq的请求

QQConnection.java
```java
@Component
public class QQConnection {

    @Value("${app_ID}")
    private String appId;
    @Value("${app_KEY}")
    private String appKey;
    @Value("${redirect_URI}")
    private String redirectUri;

    public String getAccessToken(String code) {
        String url = "https://graph.qq.com/oauth2.0/token?grant_type=authorization_code&client_id=" + appId + "&client_secret=" + appKey + "&code=" + code + "&redirect_uri=" + redirectUri;
        Request request = new Request.Builder()
                .url(url)
                .build();
        OkHttpClient client = new OkHttpClient();
        try (Response response = client.newCall(request).execute()) {
            String string = response.body().string();
            String token = string.split("&")[0].split("=")[1];
            return token;
        } catch (IOException e) {
            return null;
        }
    }

    public String getOpenId(String accessToken) {
        String url = "https://graph.qq.com/oauth2.0/me?access_token=" + accessToken;
        Request request = new Request.Builder().url(url).build();
        OkHttpClient client = new OkHttpClient();
        try (Response response = client.newCall(request).execute()) {
            String[] string = response.body().string().split("\"");
            String openId = string[string.length - 2];
            return openId;
        } catch (IOException e) {
            return null;
        }
    }

    public String getUser(String code){
        String accessToken = getAccessToken(code);
        String openId = getOpenId(accessToken);
        String url = "https://graph.qq.com/user/get_user_info?access_token=" + accessToken + "&oauth_consumer_key=" + appId + "&openid=" + openId;
        Request request = new Request.Builder().url(url).build();
        OkHttpClient client = new OkHttpClient();
        try (Response response = client.newCall(request).execute()) {
            String res = response.body().string();
            return res;
        } catch (IOException e) {
            return null;
        }
    }
}
```

IndexController.java
```java
@GetMapping("/callback")
@ResponseBody
public String qqLogin(@RequestParam("code") String code, @RequestParam("state") String state) {
    return qqConnection.getUser(code);
}
```

这里没有进行过多的参数判断, 只是简单地实现过程
最终返回的是用户的一些json信息

下面来解释一下代码:

1. 通过@Value注解将配置文件中的相应值注入到变量中
```java
@Value("${app_ID}")
private String appId;
@Value("${app_KEY}")
private String appKey;
@Value("${redirect_URI}")
private String redirectUri;
```

2. 前端用户点击qq登录, 跳转到qq验证, qq接收到请求后会返回code和state

用户点击链接: https://graph.qq.com/oauth2.0/authorize?response_type=code&client_id=123456789&redirect_uri=https://example.com/callback&state=123

返回链接及参数: https://example.com/callback?code=7241538976&state=123

```java
@GetMapping("/callback")
@ResponseBody
public String qqLogin(@RequestParam("code") String code, @RequestParam("state") String state) {
    return qqConnection.getUser(code);
}
```

3. 这里接收controller传来的code值, 通过Code获取Access Token

访问链接: https://graph.qq.com/oauth2.0/token?grant_type=authorization_code&client_id=123456789&client_secret=1234567890asdf&code=7241538976&redirect_uri=https://example.com/callback

返回: access_token=E1860D7C46FFA&expires_in=7776000&refresh_token=D72D2CB0CBA627A07B1

```java
public String getAccessToken(String code) {
    String url = "https://graph.qq.com/oauth2.0/token?grant_type=authorization_code&client_id=" + appId + "&client_secret=" + appKey + "&code=" + code + "&redirect_uri=" + redirectUri;
    Request request = new Request.Builder()
            .url(url)
            .build();
    OkHttpClient client = new OkHttpClient();
    try (Response response = client.newCall(request).execute()) {
        String string = response.body().string();
        String token = string.split("&")[0].split("=")[1];
        return token;
    } catch (IOException e) {
        return null;
    }
}
```

通过OKHttp进行get请求

```java
Request request = new Request.Builder()
        .url(url)
        .build();
OkHttpClient client = new OkHttpClient();
try (Response response = client.newCall(request).execute()) {
    String string = response.body().string();
    String token = string.split("&")[0].split("=")[1];
    return token;
} catch (IOException e) {
    return null;
}
```

由于返回的是: access_token=E1860D7C46FFA&expires_in=7776000&refresh_token=D72D2CB0CBA627A07B1
而我只需要access_token, 所以使用了正则表达式对字符串进行截取

第一次分割&两边的内容, 第二次分割=两边的内容
```java
String string = response.body().string();
String token = string.split("&")[0].split("=")[1];
```

这段代码获取了qq的返回值
```java
String string = response.body().string();
```

4. 通过token获取openId

访问链接: https://graph.qq.com/oauth2.0/me?access_token=E1860D7C46FFA

返回: callback( {"client_id":"123456789","openid":"BEC4933D"} );

```java
public String getOpenId(String accessToken) {
    String url = "https://graph.qq.com/oauth2.0/me?access_token=" + accessToken;
    Request request = new Request.Builder().url(url).build();
    OkHttpClient client = new OkHttpClient();
    try (Response response = client.newCall(request).execute()) {
        String[] string = response.body().string().split("\"");
        String openId = string[string.length - 2];
        return openId;
    } catch (IOException e) {
        return null;
    }
}
```

这里通过正则表达式分割引号两边的内容, 最终获得openId
```java
String[] string = response.body().string().split("\"");
String openId = string[string.length - 2];
```

5. 最后通过openId得到用户的信息

访问链接: https://graph.qq.com/user/get_user_info?access_token=E1860D7C46FFA&oauth_consumer_key=123456789&openid=BEC4933D

返回:
```json
{
    "ret": 0,
    "msg": "",
    "nickname": "张三",
    "gender": "男",
    "province": "湖南",
    "city": "长沙",
    "year": "1986",
    "figureurl": "http:/qzapp.qlogo.cn/qzaC45874BEACB832DD4FC"
}
```

```java
public String getUser(String code){
    String accessToken = getAccessToken(code);
    String openId = getOpenId(accessToken);
    String url = "https://graph.qq.com/user/get_user_info?access_token=" + accessToken + "&oauth_consumer_key=" + appId + "&openid=" + openId;
    Request request = new Request.Builder().url(url).build();
    OkHttpClient client = new OkHttpClient();
    try (Response response = client.newCall(request).execute()) {
        String res = response.body().string();
        return res;
    } catch (IOException e) {
        return null;
    }
}
```

# github登录

GithubProvider.java
```java
@Component
public class GithubProvider {
    public String getAccessToken(AccessTokenDTO accessTokenDTO) {
        MediaType mediaType
                = MediaType.get("application/json; charset=utf-8");

        OkHttpClient client = new OkHttpClient();

        RequestBody body = RequestBody.create(mediaType, JSON.toJSONString(accessTokenDTO));
        Request request = new Request.Builder()
                .url("https://github.com/login/oauth/access_token")
                .post(body)
                .build();
        try (Response response = client.newCall(request).execute()) {
            String string = response.body().string();
            String token = string.split("&")[0].split("=")[1];
            return token;
        } catch (IOException e) {
            e.printStackTrace();
        }
        return null;
    }

    public GithubUser getUser(String accessToken){
        OkHttpClient okHttpClient = new OkHttpClient();
        Request request = new Request.Builder().url("https://api.github.com/user?access_token=" + accessToken).build();
        try (Response response = okHttpClient.newCall(request).execute()){
            String string = response.body().string();
            GithubUser githubUser = JSON.parseObject(string, GithubUser.class);
            return githubUser;
        } catch (IOException e) {
            return null;
        }
    }

}
```

GithubUser.java
```java
@Data
public class GithubUser {
    private String name;
    private Long id;
    private String bio;
    private String avatarUrl;
}
```

AuthorizeController.java
```java
@Controller
public class AuthorizeController {

    @Autowired
    private GithubProvider githubProvider;

    @Value("${github.client.id}")
    private String clientId;
    @Value("${github.client.secret}")
    private String clientSecret;
    @Value("${github.redirect.uri}")
    private String redirectUri;

    @Autowired
    private UserMapper userMapper;

    @GetMapping("/callback")
    public String callback(@RequestParam(name = "code") String code, @RequestParam(name = "state") String state, HttpServletRequest request, HttpServletResponse response){
        AccessTokenDTO accessTokenDTO = new AccessTokenDTO();
        accessTokenDTO.setClient_id(clientId);
        accessTokenDTO.setClient_secret(clientSecret);
        accessTokenDTO.setCode(code);
        accessTokenDTO.setRedirect_uri(redirectUri);
        accessTokenDTO.setState(state);
        String accessToken = githubProvider.getAccessToken(accessTokenDTO);
        GithubUser githubUser = githubProvider.getUser(accessToken);
        if(githubUser != null){
            User user = new User();
            String token = UUID.randomUUID().toString();
            user.setToken(token);
            user.setName(githubUser.getName());
            user.setAccountId(String.valueOf(githubUser.getId()));
            user.setGmtCreate(System.currentTimeMillis());
            user.setGmtModified(user.getGmtCreate());
            user.setAvatarUrl(githubUser.getAvatarUrl());
            userMapper.insert(user);
            response.addCookie(new Cookie("token", token));
        }
        System.out.println(githubUser);
        return "redirect:/";
    }
}
```

html
```html
<a href="https://github.com/login/oauth/authorize?client_id=I8d34a2&redirect_uri=http://localhost:8080/callback&scope=user&state=1">登录</a>
```

# springboot上传实例

```html
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" value="下载" name="file">
    <input type="submit" value="提交">
</form>
```

```properties
# 设置存放路径
downloadPlace=E:/workspace/test
# 设置单个文件大小
spring.servlet.multipart.max-file-size=200MB
# 设置上传文件总大小
spring.servlet.multipart.max-request-size=500MB

#root 日志级别以WARN级别输出
logging.level.root=WARN
#springframework.web日志以DEBUG级别输出
logging.level.org.springframework.web=DEBUG
# 日志存放路径
logging.file=log/download.log
#配置控制台日志显示格式
logging.pattern.console=%d{yyyy/MM/dd-HH:mm:ss} [%thread] %-5level %logger- %msg%n
#配置文件中日志显示格式
logging.pattern.file=%d{yyyy/MM/dd-HH:mm:ss}  [%thread] %-5level %logger- %msg%n
```

DownloadController.java
```java
@Controller
public class DownloadController {

    @Value("${downloadPlace}")
    private String downloadPlace;

    @RequestMapping("/")
    public String index(){
        return "index";
    }

    @RequestMapping("/upload")
    public String fileUpload(@RequestParam("file") MultipartFile file , HttpServletRequest request) throws IOException {

        //获取文件名 : file.getOriginalFilename();
        String uploadFileName = file.getOriginalFilename();
        System.out.println(uploadFileName);

        //如果文件名为空，直接回到首页！
        if ("".equals(uploadFileName)){
            return "index";
        }

        //上传路径保存设置
        String path = downloadPlace;
        //如果路径不存在，创建一个
        File realPath = new File(path);
        if (!realPath.exists()){
            realPath.mkdir();
        }

        InputStream is = file.getInputStream(); //文件输入流
        OutputStream os = new FileOutputStream(new File(realPath,uploadFileName)); //文件输出流

        //读取写出
        int len=0;
        byte[] buffer = new byte[1024];
        while ((len=is.read(buffer))!=-1){
            os.write(buffer,0,len);
            os.flush();
        }
        os.close();
        is.close();
        return "index";
    }

}
```

# mybatis-generator

# markdown editor github

