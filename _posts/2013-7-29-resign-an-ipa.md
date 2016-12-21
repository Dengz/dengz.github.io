---
layout: post
title: RESIGN AN IPA
---

有时候需要对一个已经打好的ipa包进行重新打包, 比如一个使用开发者证书签名的ipa包,在经过测试以后,需要切换成发布证书重新签名打包, 然后再发布到app store上.当然, 还有个情况是需要改变ipa包里面的渠道值, 然后重新打包发布.(虽然这种情况下,除了app store以外的基本都是越狱渠道, 基本不需要关注签名问题)

在这些情况下, 如果有源码,或者是项目的archive文件, 那么我们可以直接用xcode来重新打包. 如果没有的话, 我们还可以使用以下命令来重新打包.

1.Unzip the IPA

```
unzip Application.ipa
```

2.Remove old CodeSignature

```
rm -r "Payload/Application.app/_CodeSignature" "Payload/Application.app/CodeResources" 2> /dev/null | true
```

3.Replace embedded mobile provisioning profile

```
cp "MyEnterprise.mobileprovision" "Payload/Application.app/embedded.mobileprovision"
```

4.Re-sign

```
/usr/bin/codesign -f -s "iPhone Distribution: Certificate Name" --resource-rules "Payload/Application.app/ResourceRules.plist" "Payload/Application.app"
```

5.Re-package

```
zip -qr "Application.resigned.ipa" Payload
```

话说回来, 这样不就是可以把别人的包给改成自己的包了么…
