---
title: OAuth2登录基本流程
date: 2025-2-11 9:25
description: 记录OAuth2登录实现方式
image: "../public/assets/images/wechat_oauth.png"
category: 教程
tags:
  - OAuth2
  - 登录
  - Web
published: true
sitemap: true
---

## OAuth2 登录介绍

OAuth 是一个关于授权（authorization）的开放网络标准，在全世界得到广泛应用，目前的版本是 2.0 版。
一般 OAuth2，能在某个平台通过其他平台或方式登录，比如在自己开发的 web 应用中，接入 QQ，微信，GitHub 等第三方登录。OAuth2 能让首次使用的用户可以不注册网站账号，使用自己熟悉的平台账号登录，可以简化认证，增强用户体验。

## OAuth2 授权原理

借用微信的 OAuth2 授权登录图
![OAuth2授权](../public/assets/images/wechat_oauth.png)

## OAuth2 授权流程

这里以微信为例,其他平台的 OAuth2 认证大体相同

### 第一步：申请开放平台 AppID 和 AppSecret

选择你需要的 OAuth 授权平台，如微信，就在搜索引擎搜索 “微信开放平台” ，根据其中引导获取 AppID 和 AppSecret。

不少平台的 AppID 和 AppSecret 只有企业级应用才能申请。

![获取API1](../public/assets/images/wechat_oauth_1.png)

![获取API2](../public/assets/images/wechat_oauth_2.png)

提交后要等待一段时间审核，审核通过后可以拿到 AppID 和 AppSecret。

### 第二步：请求 code

使用我们上一步获取的`AppID`和`AppSecret`，发送 get 请求到微信的授权 url 中

示例：`https://open.weixin.qq.com/connect/qrconnect?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect`

该页面会有一个登录二维码，如果用户扫码确认这次认证，会返回一个重定向到你申请的应用网址。

示例：`redirect_uri?code=CODE&state=STATE`

### 第三步：通过 code 获取 access_token

使用上一步重定向时接收的`code`以及获取的`AppID`和`AppSecret`，，发送到请求微信 token 的 url 中

示例：`https://api.weixin.qq.com/sns/oauth2/access_token?appid=APPID&secret=SECRET&code=CODE&grant_type=authorization_code`

请求成功会返回类似如下的数据：

```JSON
{
"access_token":"ACCESS_TOKEN",
"expires_in":7200,
"refresh_token":"REFRESH_TOKEN",
"openid":"OPENID",
"scope":"SCOPE",
"unionid": "o6_bmasdasdsad6_2sgVt7hMZOPfL"
}
```

获取到了`access_token`，就等同于我们已经成功通过了微信的 OAuth2 登录验证，我们可以使用该`access_token`请求微信提供的 api，获取用户信息。

> [网站应用微信登录开发指南 ](https://developers.weixin.qq.com/doc/oplatform/Website_App/WeChat_Login/Wechat_Login.html)  
> [理解 OAuth2](https://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)
