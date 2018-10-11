---
title: JWT
date: 2018-10-11 14:59:09
tags: [JWT]
categories: [General]
---
JWT: JSON Web Token 是目前最流行的跨域认证解决方案。

## 一、JWT的数据结构

JWT 的三个部分依次如下。
- Header(头部)
- Payload(负载)
- Signature(签名)

写成一行，就是下面的样子。
```
Header.Payload.Signature
```
下面依次介绍这三个部分。

### 1.1 Header

Header 部分是一个 JSON 对象，描述 JWT 的元数据，通常是下面的样子。
```
{
    "alg": "HS256",
    "typ": "JWT"
}
```
### 1.2 Payload
Payload 部分也是一个 JSON 对象，用来存放实际需要传递的数据。
- iss (issuer)：签发人
- exp (expiration time)：过期时间
- sub (subject)：主题
- aud (audience)：受众
- nbf (Not Before)：生效时间
- iat (Issued At)：签发时间
- jti (JWT ID)：编号

除了官方字段，你还可以在这个部分定义私有字段，下面就是一个例子。
```
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```
注意，JWT 默认是不加密的，任何人都可以读到，所以不要把秘密信息放在这个部分。

### 1.3 Signature
ignature 部分是对前两部分的签名，防止数据篡改。
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```
算出签名以后，把 Header、Payload、Signature 三个部分拼成一个字符串，每个部分之间用"点"（.）分隔，就可以返回给用户。

## 二、JWT 的使用方式
客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。

此后，客户端每次与服务器通信，都要带上这个 JWT。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息Authorization字段里面。
```
Authorization: Bearer <token>
```
另一种做法是，跨域的时候，JWT 就放在 POST 请求的数据体里面。

\----------------------------------------------------
|  感觉实际使用还是得存库或者redis中呀                  |
\----------------------------------------------------
