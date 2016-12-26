---
layout: post
title: 解析URL上面的参数
author: shefh
category: blog
published: true
---

 iOS下解析url上面的参数，废话不多说，直接代码吧：

 {% highlight swift linenos %}

 extension NSURL {
    public  func queryParameters()->[String:String] {
        guard let queString = self.query else {
            return [String:String]();
        }
        return queString.queryParameters()
    }
}

extension String {
    public  func queryParameters()->[String:String] {        
        var parameters = [String:String]()
        self.components(separatedBy: "&").forEach {
            let keyAndValue = $0.components(separatedBy: "=")
            if keyAndValue.count == 2 {
                let key = keyAndValue[0]
                let value = keyAndValue[1].replacingOccurrences(of: "+", with: " ").removingPercentEncoding
                    ?? keyAndValue[1]
                parameters[key] = value
            }
        }
        return parameters
    }
}
{% endhighlight %}