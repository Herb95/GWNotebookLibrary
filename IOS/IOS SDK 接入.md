QQ文档:![](file:///C:\Users\Administrator\AppData\Roaming\Tencent\QQ\Temp\[5UQ[BL(6~BS2JV6W}N6[%S.png)https://wiki.connect.qq.com/sdk%e4%b8%8b%e8%bd%bd  
微信稳定:![](file:///C:\Users\Administrator\AppData\Roaming\Tencent\QQ\Temp\[5UQ[BL(6~BS2JV6W}N6[%S.png)https://developers.weixin.qq.com/doc/oplatform/Mobile_App/WeChat_Login/Development_Guide.html  
记得 这两种腾讯的登录方式 都取玩家的 unionid 这个值当作唯一标识  


Info.plist

```
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>weixin</string>
        <string>weixinULAPI</string>
        <string>weixinURLParamsAPI</string>
        <string>tim</string>
        <string>mqq</string>
        <string>mqqapi</string>
        <string>mqqbrowser</string>
        <string>mttbrowser</string>
        <string>mqqOpensdkSSoLogin</string>
        <string>mqqopensdkapiV2</string>
        <string>mqqopensdkapiV4</string>
        <string>mqzone</string>
        <string>mqzoneopensdk</string>
        <string>mqzoneopensdkapi</string>
        <string>mqzoneopensdkapi19</string>
        <string>mqzoneopensdkapiV2</string>
        <string>mqqapiwallet</string>
        <string>mqqopensdkfriend</string>
        <string>mqqopensdkavatar</string>
        <string>mqqopensdkminiapp</string>
        <string>mqqopensdkdataline</string>
        <string>mqqgamebindinggroup</string>
        <string>mqqopensdkgrouptribeshare</string>
        <string>tencentapi.qq.reqContent</string>
        <string>tencentapi.qzone.reqContent</string>
        <string>mqqthirdappgroup</string>
        <string>mqqopensdklaunchminiapp</string>
        <string>mqqopensdkproxylogin</string>
        <string>mqqopensdknopasteboard</string>
        <string>mqqopensdknopasteboardios16</string>
        <string>mqqopensdkcheckauth</string>
        <string>mqqguild</string>
    </array>
```