---
layout: post
title: URL Encoding 的坑
---

## URL Encoding
URLs can only be sent over the Internet using the [ASCII character-set](http://www.w3schools.com/charsets/ref_html_ascii.asp).

Since URLs often contain characters outside the ASCII set, the URL has to be converted into a valid ASCII format.

URL encoding replaces unsafe ASCII characters with a "%" followed by two hexadecimal digits.

URLs cannot contain spaces. URL encoding normally replaces a space with a plus (+) sign or with %20.


掉坑是因为App内动态控制 url ,服务端返回的 url 带中文字符, NSString类提供了 `stringByAddingPercentEscapesUsingEncoding:` 方法.该方法能编码非 url 的字符,但对于类似 `/` `&` 的 reserved characters 没有做处理,也就是没有严格遵守 url 应该编码成 ASCII 字符的规范.
网上给的通用String的Category:

    - (NSString *)urlencode {
        NSMutableString *output = [NSMutableString string];
        const unsigned char *source = (const unsigned char *)[self UTF8String];
        int sourceLen = strlen((const char *)source);
        for (int i = 0; i < sourceLen; ++i) {
            const unsigned char thisChar = source[i];
            if (thisChar == ' '){
                [output appendString:@"+"];
            } else if (thisChar == '.' || thisChar == '-' ||
                       thisChar == '_' || thisChar == '~' ||
                       (thisChar >= 'a' && thisChar <= 'z') ||
                       (thisChar >= 'A' && thisChar <= 'Z') ||
                       (thisChar >= '0' && thisChar <= '9')) {
                [output appendFormat:@"%c", thisChar];
            } else {
                [output appendFormat:@"%%%02X", thisChar];
            }
      }
      return output;
    }
