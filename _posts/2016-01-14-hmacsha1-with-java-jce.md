---
layout: post
title: "使用Java JCE实现hmacsha1摘要"
permalink: hmacsha1-with-java-jce
date: 2016-01-14 17:34:49
comments: false
description: "hmacsha1 with java jce"
keywords: ""
categories: tech
tags:
- java
- jce
- digest
- hmac
---

`hmacsha1`是一种安全摘要算法，不同于MD5和sha1这种摘要算法不同的是，它需要一个密钥参与进行摘要，所以增加了摘要的安全性，目前很多OpenAPI的调用过程中，伴随参数传递的sign值一般都是通过hmacsha1摘要算法利用用户的secure_key对参数体进行摘要。

<!--more-->
{% highlight java %}
package util;

import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;

public class SignTools {

  private static final String HMAC_SHA1_ALGORITHM = "HmacSHA1";

  private static final char[] DIGIT = { '0', '1', '2', '3', '4', '5', '6',
      '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f' };

  /**
   * 使用hmacsha1安全摘要算法生成摘要
   * @param data 需要摘要的字符串
   * @param key 摘要使用的密钥
   * @return 如果计算失败则返回null
   */
  public static String sign(String data, String key){

    String dgst = null;

    SecretKeySpec signingKey = 
      new SecretKeySpec(key.getBytes(),HMAC_SHA1_ALGORITHM);

    Mac mac = null;
    try {
      mac = Mac.getInstance(HMAC_SHA1_ALGORITHM);
      mac.init(signingKey);
      byte[] dg = mac.doFinal(data.getBytes());
      StringBuffer sb = new StringBuffer();//StringBuilder
      for (int i = 0; i &lt; dg.length; i++) { 
        sb.append(hexString(dg[i]));
      }
      dgst = sb.toString();
    } catch (NoSuchAlgorithmException e) {
      //ignore
    } catch (InvalidKeyException e) {
      e.printStackTrace();
    }
    return dgst;
  }

  private static String hexString(byte b) {
    char[] ob = new char[2];
    ob[0] = DIGIT[(b &gt;&gt;&gt; 4) &amp; 0X0F];
    ob[1] = DIGIT[b &amp; 0X0F];
    String s = String.valueOf(ob);
    return s;
  }

  public static void main(String[] args) throws Exception {
    String data = "562742098461008807900199";
    String key = "HZsn43%Y6Z5wyMqp@I&amp;g&amp;u0dl@Vd^oHS";
    System.out.println(SignTools.sign(data,key));
  }

}
{% endhighlight %}

https://gist.github.com/wubo19842008/8318372