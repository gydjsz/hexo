---
title: spring boot实现url下载数据到浏览器
tags:
- spring boot
---

```java
	@RequestMapping("/download")
    public void download(HttpServletRequest request, HttpServletResponse response) throws IOException {
        String urlStr = request.getParameter("url");
        URL url = new URL(urlStr);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestProperty("User-Agent", "Mozilla/4.0 (compatible; MSIE 5.0; Windows NT; DigExt)");
        InputStream inputStream = conn.getInputStream();
        response.addHeader("Content-Disposition", "attachment;filename="+ urlStr.substring(urlStr.lastIndexOf("/") + 1));
        byte[] buffer = new byte[1024 * 10];
        int len = 0;
        OutputStream outputStream = response.getOutputStream();
        while((len = inputStream.read(buffer)) != -1) {
            outputStream.write(buffer, 0, len);
        }
        outputStream.close();
        outputStream.flush();
        inputStream.close();
        response.flushBuffer();
    }
```
