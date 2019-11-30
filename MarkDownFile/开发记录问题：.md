# 开发记录问题：



## 微信小程序中的问题：

    可以内置手机号验证码

```java
1.问题：session_key失效
    通过前台传入的iv,code,encryptedData，nick后台通过接受这些参数
    code: "033gH7422h46pV0nDA222hCa422gH74d"
    encryptedData: "WzOuQVxdCRM6VzQ8Oxdtp5e+x1D7yo4e8TwAxFkA4wDSrK89SY0+fugD8QF3I3Q7Fw61moaqCO6h1hvQofs/+5f+mZcojg7TcTni7au4M8dCWDAEgg/Xk6jHejs2Pu4CGC3dct/lxXN8FqDC04zE4AXBNImw6q9RTsH7p5H4+JJn3nVZBTm+jrgcb4ZNQ6EtzUS/c3Bqp97lerGxp0CbW8m40TZu18BIBnKn77GCNo8WsL8fgFmxUmUVDxWbW7wRXCLDkjOmaGrYcExnZSmVAmh5rpl3S84L9IcxJyaBl/SYadqa+ttSXMWaWtmSGLl1JUrh4qn9uU4fb4s6OBMuZLN/M7x+7u2i6l5GHaOF+jvCYbduibtjbhop4OOk+mlTEy2XgMuVdJVorTK5fcDvkyA9wJqWQTWL3Xm3uvgK1JwDkjGT/SIOydwjWwKmRQgrduxQ/bruzVhDNaunSdsZv2HYd4OIc0wZUxTXzPuUQLrHdegMPCP5Dsm4wR5DH1bixEgkcBgBV21PGVD/n5VWBA=="
    iv: "MIU0nuFdCk/dqeTjQrQUbQ=="
    nick: "Kay三石"
    拼接出xcxLoginUrl = "https://api.weixin.qq.com/sns/jscode2session?appid=%s&secret=%s&js_code=%s&grant_type=authorization_code";
    通过微信返回的json数据置换出session_key，
    通过session_key和iv,code,encryptedData,通过加密/解密算法解密出
    unionid，和openid,通过唯一的unionId获取到userid的用户信息
    总的来说，就是不能点击按钮后，才调用wx.login，必须先wx.login后，才能点击按钮，这时候的session_key才是正确的
    使用的时候，确保微信小程序在同一个开发者账号平台下，不然获取不到unionid,unionID是微微信唯一的身份标识
    WeChatSecret weChatSecret = weChatConfig.getSecrets().get(client.toUpperCase());
        String loginUrl = String.format(xcxLoginUrl, weChatSecret.getAppId(), weChatSecret.getAppSecret(), request.getCode());
        String result = restTemplate.getForObject(loginUrl, String.class);
        WeXCXLoginCheckResponse checkResponse = new WeXCXLoginCheckResponse();
        String encryptedData = request.getEncryptData();
        JSONObject jsonSessionKey = new JSONObject(result);
        String iv = request.getIv();
        String sessionKey = jsonSessionKey.getString("session_key");
        checkResponse.setSession_key(sessionKey);
        String decryptData = null;
        try {
        	//解密出unionID和OpenId
            decryptData = AESUtil.decrypt(encryptedData, sessionKey, iv, "UTF-8");
        } catch (Exception e) {
            e.printStackTrace();
        }
        if (StringUtil.isEmptyOrBlank(decryptData))
            throw new BusinessException(ErrorCode.AUTHORIZE_FAILED);
        JSONObject jsonData = new JSONObject(decryptData);
        String unionId = jsonData.getString("unionId");
        String openId = jsonData.getString("openId");
        String userId = this.findUserIdByTypeAndSocialId(unionId, SocialLoginType.WE_CHAT.toString().toUpperCase());
2.辨别不同的小程序的使用：
    通过header进行辨别是否是同一个小程序
3.app分享这个小程序连接到微信
	1.必须把这个app和小程序绑定到一个开放平台账号下才可以进行使用
4.微信请求出现request begin 确认请求的ip是否是公网ip,如果是内网ip必须让手机连上内网否则不能请求到,
如果请求的是test环境必须让手机和test环境在同一网络下。
const env = 'test';
const api1Enviroment = {
  dev: 'http://dev.api2.learninggenie.cn/api/v1/',
  local: 'http://localhost:8080/api/v1/',
  test: 'http://192.168.1.109:8080/api/v1/',
  stage: 'http://stage.api2.learninggenie.cn/api/v1/',
  prod: 'https://api2.plg100.com/api/v1/'
};
```

## 微信小程序语音播放：

```html
 <view wx:if='{{childNotes.length || initShow}}'>
    <view wx:for="{{childNotes}}" bindtap='viewReport' data-note="{{note}}" wx:for-item="note" class='note clear-float' wx:for-index="item_id" wx:key="item_id">
      <view wx:if="{{note.defaultType}}">
        <view class='note-icon clear-float'>
          <image class='note-icon-png pull-left marginleft-30' src='{{note.icon}}'></image>
          <view class='pull-left marginleft-18 note-icon-font'>{{note.typeName}}</view>
          <text class='pull-left marginleft-18'>{{note.sendTime}}</text>
        </view>
      </view>
      <view wx:if="{{!note.defaultType}}" hover-class='note-report-hover'>
        <view class='note-icon clear-float'>
          <view wx:if="{{note.type == 'CHILD_REPORT'}}">
            <view class='clear-float' style='height:60rpx'>
              <image class='note-icon-png pull-left marginleft-30' src='{{note.icon}}'></image>
              <view class='pull-left marginleft-18 note-icon-font'>{{note.typeName}}</view>
              <text class='pull-left marginleft-18'>{{note.sendTime}}</text>
              <image class='pull-right note-arrow marginright-28' src="../../images/PLG_Right_arrow.png"></image>
            </view>
          </view>

          <view wx:if="{{note.type == 'IMPROVE_SCHEME'}}">
            <view class='clear-float' style='height:60rpx'>
              <image class='note-icon-png pull-left marginleft-30' src='{{note.icon}}'></image>
              <view class='pull-left marginleft-18 note-icon-font'>{{note.typeName}}</view>
              <text class='pull-left marginleft-18'>{{note.sendTime}}</text>
              <image class='pull-right note-arrow marginright-28' src="../../images/PLG_Right_arrow.png"></image>
            </view>
          </view>
        </view>
      </view>

      <view class="note-content" data-note="{{note}}">
        <!--default type show  -->
        <view wx:if="{{note.defaultType}}">
          <view class='note-payload'>
            <text wx:if="{{note.payload}}">{{note.payload}}</text>
          </view>
          <view class="overflowhidden">
            <view wx:for="{{note.media}}" wx:for-item="media" wx:key="this" >
              <view class='note-media note-media-2 swiperVideo' wx:if="{{media.fileType == 'mp4'}}">
                <cover-view class="videoMask" bindtap="swiperVideo" data-wxmedia='{{note.media}}' data-index="{{index}}"></cover-view>
                  <video data-wxmedia='{{note.media}}' data-index="{{index}}" show-fullscreen-btn="{{showFullscreen}}" poster='{{media.snapshot_url}}' objectFit="contain" src="{{media.public_url}}" direction="0">
                  </video>
                </view>
              <!-- <view   class='note-media'> -->
              <!-- <image wx:else bindtap="swiperVideo" data-index="{{index}}" data-wxmedia='{{note.media}}' data-mediaUrl='{{note.mediaUrl}}' data-Url='{{media.public_url}}' mode='aspectFill' src="{{media.public_url}}" class='note-media-2'></image> -->
               <!-- 语音 --->
              <cover-view wx:if="{{media.fileType =='aac'}}" wx:key="this" data-indexid="{{item_id}}" data-audiomedia="{{note.media}}" data-mediaid="{{media.id}}" class="audioclass"  bindtap="audioplay"  data-audio="{{media.public_url}}" >
                <!--默认状态--->
                <cover-image  class="audioImg"  style=""   src="{{isactive==item_id?'../../images/audioImgRun.gif':'../../images/audio.png'}}"  transform="{{transform}}"></cover-image>
                <!-- <cover-view class="audioText">{{isactive==item_id?audiotime:''}} </cover-view>  -->
                <cover-view class="audioText">{{note.voiceTime!=null? note.voiceTime:'0'}}s </cover-view> 
              </cover-view>
              <image wx:else bindtap="viewImage" data-index="{{index}}" data-wxmedia='{{note.media}}' data-mediaUrl='{{note.mediaUrl}}' data-Url='{{media.public_url}}' mode='aspectFill' src="{{media.public_url}}" class='note-media-2'></image>
              <!-- </view>  -->
            </view>
          </view>
          <view>
          </view>
          <view wx:if="{{note.domains.length}}" class='clear-float'>
            <view class='note-domain' wx:for="{{note.domains}}" wx:for-item="domain" wx:key="domain">
              {{domain.abbreviation}}: {{domain.name}}
            </view>
          </view>
          <!-- <view class='note-send-time'>
            <text>{{note.sendTime}}</text>
          </view> -->
        </view>
     <!---不使用gif图片，因为gif图片在使用的时候ios不支持，使用图片循环播放---->
    <!-- 语音 --->
              <view  wx:if="{{media.fileType =='aac'}}" class="audioView">
              <cover-view wx:if="{{media.fileType =='aac'}}" wx:key="this" data-indexid="{{item_id}}" data-audiomedia="{{note.media}}" data-mediaid="{{media.id}}" class="audioclass"  bindtap="audioplay"  data-audio="{{media.public_url}}" >
                <!--默认状态--->
               <!--默认状态--->
                <!-- <cover-image  class="audioImg"  style=""   src="{{isactive==item_id?'../../images/audioImgRun.gif':'../../images/audio.png'}}"  transform="{{transform}}"></cover-image> -->
                <cover-image class="audioImg"  style="{{isactive==item_id?'display:none':''}}"   src="../../images/audio.png"></cover-image>
               
                <cover-view wx:if="{{isactive==item_id}}">
                 <cover-image wx:if="{{index_image==0}}" class="audioImg" style="" src="../../images/audio.png"></cover-image>
                <cover-image wx:if="{{index_image==1}}"  class="audioImg"  style=""   src="{{'../../images/audio1.png'}}"  ></cover-image>
                <cover-image wx:if="{{index_image==2}}" class="audioImg" style="" src="../../images/audio2.png"></cover-image>
                <cover-image wx:if="{{index_image==3}}" class="audioImg" style="" src="../../images/audio.png"></cover-image>
              <cover-view class="audioText">{{note.voiceTime!=null? note.voiceTime:'0'}}s </cover-view>    
</cover-view>
                <!-- <cover-view class="audioText">{{isactive==item_id?audiotime:''}} </cover-view>  -->
                <cover-view class="audioText">{{note.voiceTime!=null? note.voiceTime:'0'}}s </cover-view> 
              </cover-view>
              </view>    
```

js中使用：



```javascript
const inneraudioContext =wx.createInnerAudioContext(); // 创建音频播放
//音频播放
  audioplay: function (e) {
    let that = e.this;
    var src = e.currentTarget.dataset.audio;
    var mediaid = e.currentTarget.dataset.mediaid;  // 音频的ID
    var indexid = e.currentTarget.dataset.indexid  // 列表项ID
    var media = e.target.dataset.audiomedia;
    console.log(indexid)

    inneraudioContext.autoplay = true;
    inneraudioContext.src = src;
    var res = wx.getSystemInfoSync()
    if (res.platform == 'ios') {
      console.log('ios')
      inneraudioContext.obeyMuteSwitch = false;
      wx.playBackgroundAudio({
        dataUrl: src,
      })
      wx.onBackgroundAudioPlay((res) => {
        console.log('ios音频播放')
        this.setData({
          isactive: indexid,
          // 通过判断是不是当前的ID 如果是则后对比为true
        })
        speaking.call(this)
      })
      wx.onBackgroundAudioStop((res) => {
        console.log("ios音频停止了")
        clearInterval(this.timer)
        this.setData({
          isactive: indexid + "stop",
          // index_image:1
          // audiotime:''
        })
      })
    } else {
      setTimeout(() => {
        inneraudioContext.play();
        inneraudioContext.currentTime
        inneraudioContext.onTimeUpdate(() => {
          console.log(inneraudioContext.duration)   //总时长
          // this.setData({
          //   // audiotime: inneraudioContext.duration.toFixed(0)+'s',
          //   isactive: indexid
          // })
          console.log('开始播放！代码')
        })

      }, 500)
      inneraudioContext.onPlay((res) => {
        console.log('android音频播放')
        this.setData({
          isactive: indexid,
          // 通过判断是不是当前的ID 如果是则后对比为true
        })
        speaking.call(this)
      })

      inneraudioContext.onPause((res) => {
        console.log("pause")
        this.setData({
          isactive: indexid + "pause"
        })
        clearInterval(this.timer)
      })
      inneraudioContext.onEnded((res) => {
        console.log("android音频停止了")
        clearInterval(this.timer)
        this.setData({
          isactive: indexid + "stop",
          // index_image:1
          // audiotime:''
        })
      })
      inneraudioContext.onError((res) => {
        console.log(res.message)
        console.log(res.errCoder)
      })
      clearInterval(this.timer)
    }

  },
 function speaking() {
  var _this = this
  //话筒帧动画
  var i = 1;
  this.timer = setInterval(function () {

    i = i % 4;
    _this.setData({
      index_image: i
    })
    i++;

  }, 500);
}

function speaking() {
  var _this = this
  //话筒帧动画
  var i = 1;
  this.timer = setInterval(function () {

    i = i % 4;
    _this.setData({
      index_image: i
    })
    i++;

  }, 500);
}


```



## 短信邀请：

只需要手机号码就可以：

```java
// 查询该班级已绑定未激活的手机号
        List<InvitationModel> childInvitationModel = invitationDao.getInvitationPhoneNumbersByGroupId(groupId);
        List<String> keys = new ArrayList<>();
        childInvitationModel.forEach(ci -> {
            String childId = ci.getChildId();
            String phoneNumber = ci.getUserEmail();
            keys.add(childId + phoneNumber + groupId);
        });
        // 校验发送次数，如果超出则提示
        if (shieldProvider.isOperationExcessive(ip, ShieldLimitType.PLG_INVITE_PARENT_SMS, 500)) {
            throw new BusinessException(ErrorCode.IP_SEND_SMS_EXCESSIVE, MSG.t("IP_SEND_SMS_EXCESSIVE"));
        }
        List<String> newKeys = new ArrayList<>();
        for (int i = 0; i < keys.size(); i++) {
            if (!shieldProvider.isOperationExcessive(keys.get(i), ShieldLimitType.PLG_INVITE_PARENT_SMS, 1)) {
                newKeys.add(keys.get(i));
            }
        }
        if (newKeys.isEmpty()) {
            throw new BusinessException(ErrorCode.INVITE_PARENT_SMS_EXCESSIVE, MSG.t("INVITE_PARENT_SMS_EXCESSIVE"));
        }
        // 发送短信
        AgencyEntity agency = agencyDao.getByGroupId(groupId);
        String agencyName = "";
        String defaultAgencyId = "";
        if (agency != null) {
            agencyName = agency.getName();
            defaultAgencyId = agency.getId();
        }
        for (InvitationModel invitationModel : childInvitationModel) {
            String childId = invitationModel.getChildId();
            String phoneNumber = invitationModel.getUserEmail();
            String childName = invitationModel.getChildName();
            String contact = childId + phoneNumber + groupId;
            if (!newKeys.contains(contact)) continue;
            // 手机号发送验证码次数+1(用于限制手机号单日最大发送短信数量)
            shieldProvider.userOperationINCR(contact, ShieldLimitType.PLG_INVITE_PARENT_SMS);
            // IP发送验证码次数+1(用于限制IP单日最大发送短信数量)
            shieldProvider.userOperationINCR(ip, ShieldLimitType.PLG_INVITE_PARENT_SMS);
            ShortMessage message = new ShortMessage();
            message.setContent();
            message.setReceiver(phoneNumber);
            AgencyMetaDataEntity agencytype = agencyDao.getMeta(defaultAgencyId, AgencyMetaKey.WXMP_USERNAME.toString());
            // 短信类型:sendMsg方法会根据短信类型选择 不同的短信模板
            if (agencytype != null && !StringUtil.isEmptyOrBlank(agencytype.getMetaValue())) {
                message.setSignName(SmsUtil.findSingNameBySmsType(ShortMessageType.PLG_INVITE_PARENT_HCY));
                message.setType(ShortMessageType.PLG_INVITE_PARENT_HCY);
            } else {
                message.setSignName(SmsUtil.findSingNameBySmsType(ShortMessageType.PLG_INVITE_PARENT));
                message.setType(ShortMessageType.PLG_INVITE_PARENT);
            }

            try {
                shortMessageProvider.sendMsg(message);
            } catch (Exception e) {
                e.printStackTrace();
                throw new BusinessException(ErrorCode.SMS_SEND_FAILURE, MSG.t("SMS_SEND_FAILURE"));
            }
        }
```

sendMsg方法：

```Java
@Override
    public Boolean sendMsg(ShortMessage message) throws ClientException {
        SendSmsRequest request = new SendSmsRequest();
        request.setPhoneNumbers(message.getReceiver());
        // SignName在阿里云短信管理页面可查
        request.setSignName(message.getSignName());
        request.setTemplateCode(SmsUtil.findTemplateCodeBySmsType(message.getType()));
        request.setTemplateParam(message.getContent().replaceAll("Mr.", ""));
        // 选填-上行短信扩展码(无特殊需求用户请忽略此字段)
        // request.setSmsUpExtendCode("90997");
        // 可选:outId为提供给业务方扩展字段,最终在短信回执消息中将此值带回给调用者
        // request.setOutId("yourOutId");
        // hint 此处可能会抛出异常，注意catch
        SendSmsResponse sendSmsResponse = null;
      	//阿里云提供的接口
        sendSmsResponse = acsClient.getAcsResponse(request);
        // 非法手机号
        if (Objects.equals("isv.MOBILE_NUMBER_ILLEGAL", sendSmsResponse.getCode())) {
            throw new BusinessException(ErrorCode.PHONE_NUMBER_ILLEGAL, MSG.t("PHONE_NUMBER_ILLEGAL"));
        }
        return Objects.equals("OK", sendSmsResponse.getCode());
    }
```

## wkhtml转pdf



```java
public void dashboardPdf(final String userId, final ExportDashboardPdfRequest request) {
        String frameworkId = request.getFrameworkId();
        UserEntity user = userProvider.checkUser(userId);
        DashboardDataRequest dataRequest = this.processPdfData(request, user);
        String htmlPath = ResourceUtil.createHtmlFile(dataRequest, "dashboard/observation_and_rating+measures.html");
        File htmlFile = new File(htmlPath);
        String key = "html/" + UUID.randomUUID() + ".html";
        fileSystem.upload(pdfBucket, key, htmlFile);
        String htmlUrl = fileSystem.getPublicUrl(pdfBucket, key);
        htmlUrl = htmlUrl.replace(s3Root, pdfEndpoint);
        List<String> htmlUrls = new ArrayList<>();
        htmlUrls.add(htmlUrl);
        //处理文件名字
        String fileName = "Dashboard";
        if (domainNames.get(frameworkId) == null) {
            DomainEntity domain = domainDao.getDomain(frameworkId);
            domainNames.put(frameworkId, domain.getName());
        }
        String domainName = domainNames.get(frameworkId);
        fileName = fileName + "_" + domainName + ".pdf";

        //保存上传的htmlUrl
        PdfConvertJobEntity pdfConvertJobEntity = new PdfConvertJobEntity();
        pdfConvertJobEntity.setUrl(org.apache.commons.lang3.StringUtils.join(htmlUrls, ","));
        pdfConvertJobEntity.setStatus(PdfConvertStatus.CREATED.toString());
        pdfConvertJobEntity.setCreatedBy(userId);
        pdfConvertJobEntity.setCreatedAtUtc(TimeUtil.getUtcNow());
        pdfConvertJobEntity.setType(PdfType.DASHBOARD_FRAMEWORK_PDF.toString());
        pdfConvertJobEntity.setPdfName(fileName);
        pdfConvertJobEntity.setData(domainName);
        reportDao.createPdfConvertJob(pdfConvertJobEntity);
    }
```

```java
public static String createHtmlFile(Object data, String templateName) {
        String jsonData = JsonUtil.toJson(data);
        String template = ResourceUtil.getResourceAsString(templateName);
        String htmlStr = template.replace("@{data}", jsonData);

        String htmlFilePath = FileUtil.randomTempFilePath(".html");
        FileUtil.createStringFile(htmlFilePath, htmlStr);
        return htmlFilePath;
    }
```

使用：

```html
<script type="text/javascript">
    var data = @{data};
    通过这个data进行获取数据
```
### dashboardpdf

```html
<!DOCTYPE html>
<html lang="en">

	<head>
		<meta charset="UTF-8">
		<title>pdf</title>
		<!-- <script src="../libs/echarts/echarts-en.common.min.js"></script> -->
		<script src="https://cdn.bootcss.com/echarts/4.2.1-rc1/echarts-en.common.js"></script>
		<style>
			.header,
			.line-wrap {
				position: relative
			}

			.menu,
			.menu-item {}

			.main-col,
			.main-row {
				-webkit-box-sizing: border-box
			}

			.main-item,
			.main-login-family {
				-webkit-box-shadow: 0 2px 9px 1px rgba(219, 224, 223, 1)
			}

			.fontw600,
			.main-item-head {
				font-weight: 600
			}

			a,
			abbr,
			acronym,
			address,
			applet,
			article,
			aside,
			audio,
			big,
			blockquote,
			body,
			canvas,
			caption,
			cite,
			code,
			dd,
			del,
			details,
			dfn,
			div,
			dl,
			dt,
			em,
			fieldset,
			figcaption,
			figure,
			font,
			footer,
			form,
			h1,
			h2,
			h3,
			h4,
			h5,
			h6,
			header,
			hgroup,
			html,
			iframe,
			ins,
			kbd,
			label,
			legend,
			li,
			mark,
			menu,
			nav,
			object,
			ol,
			p,
			pre,
			q,
			s,
			samp,
			section,
			small,
			span,
			strike,
			strong,
			sub,
			summary,
			sup,
			table,
			tbody,
			td,
			tfoot,
			th,
			thead,
			time,
			tr,
			tt,
			ul,
			var,
			video {
				border: 0;
				margin: 0;
				outline: 0;
				padding: 0;
				vertical-align: baseline
			}

			:focus {
				outline: 0
			}

			body {
				background: #fff;
				color: #58666E;
				font-family: "Microsoft Yahei", "Source Sans Pro", "Helvetica Neue", Helvetica, Arial, sans-serif, YaHei;
				font-size: 20px
			}

			.padding-lr {
				padding-left: 25px;
				padding-right: 25px
			}

			.padding-lr-5 {
				padding-left: 5px;
				padding-right: 5px
			}

			.padding-l {
				padding-left: 15px
			}

			.padding-r {
				padding-right: 15px
			}

			.margin-b {
				margin-bottom: 10px
			}

			.font-size-37 {
				font-size: 37px
			}

			.font-size-30 {
				font-size: 30px
			}

			.font-size-27 {
				font-size: 27px
			}

			.font-size-16 {
				font-size: 16px
			}

			.font-size-13 {
				font-size: 13px
			}

			.color-gray {
				color: #9A9EA0
			}

			.border-right {
				border-right: 1px solid #ECEDF1
			}

			.border-left {
				border-left: 1px solid #ECEDF1
			}

			.border-top {
				border-top: 1px solid #ECEDF1
			}

			.fl {
				float: left
			}

			.fr {
				float: right
			}

			.full {
				width: 100%
			}

			.text-right {
				text-align: right
			}

			.text-left {
				text-align: left
			}

			.main {
				width: 1000px;
				margin: 0 auto;
				background: #f2f2f2;
			}

			.arc-wrap,
			.header,
			.main-item,
			.menu {
				background: #fff;
			}

			.header {
				text-align: center;
				height: 100px
			}

			.header h1 {
				font-size: 25px;
				height: 100px;
				padding: 15px 0;
				box-sizing: border-box;
			}

			.header h1 p {
				line-height: 35px
			}

			.line-wrap img {
				width: 100%;
				position: absolute;
				bottom: 0;
				left: 0;
				right: 0
			}

			.menu {
				height: 120px;
				padding-top: 10px;
				box-sizing: border-box;
			}

			.menu-item {
				display: inline-block;
				font-size: 18px;
				height: 100%;
				-ms-flex-direction: column;
				padding-right: 55px;
				box-sizing: border-box;
			}

			.main-item-login,
			.main-login-half {
				display: -webkit-box;
				display: -ms-flexbox
			}

			.menu-item-head {
				color: #76AB3C;
				width: 20%;
				height: 50px;
			}

			.menu-item-content {
				width: 80%;
				height: 50px;
			}

			.arc-wrap {
				height: 19px
			}

			.dashboard-date {
				font-size: 20px;
				height: 45px;
				line-height: 45px;
				text-align: center;
				overflow: hidden;
			}

			.dashboard-main {
				margin-bottom: 15px
			}

			.main-row {
				margin-left: -6px;
				margin-right: -6px;
				box-sizing: border-box;
				overflow: hidden;
				padding-bottom: 20px;
			}

			.main-col {
				float: left;
				box-sizing: border-box;
				padding-left: 6px;
				padding-right: 6px;
				width: 50%;
				margin-top: 10px
			}
			.main-col-right{
				float: right;
				box-sizing: border-box;
				padding-left: 6px;
				padding-right: 6px;
				width: 50%;
				margin-top: 10px
			}
			.main-item {
				box-shadow: 0 2px 9px 1px rgba(219, 224, 223, 1);
				border: 1px solid rgba(233, 236, 235, 1);
				height: 265px
			}

			.main-item-login {
				display: flex;
				-webkit-box-orient: horizontal;
				-webkit-box-direction: normal;
				-ms-flex-direction: row;
				flex-direction: row;
				height: 150px
			}

			.main-chat-r,
			.main-login-half,
			.main-notice-item {
				-webkit-box-orient: vertical;
				-webkit-box-direction: normal
			}

			.main-login-half {
				display: flex;
				-ms-flex-direction: column;
				flex-direction: column;
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1;
				color: #fff
			}

			.main-login-family {
				background: rgba(0, 149, 193, 1);
				box-shadow: 0 2px 9px 1px rgba(219, 224, 223, 1)
			}

			.main-login-child {
				background: rgba(132, 190, 67, 1);
				-webkit-box-shadow: 0 2px 9px 1px rgba(219, 224, 223, 1);
				box-shadow: 0 2px 9px 1px rgba(219, 224, 223, 1)
			}

			.main-item-head {
				font-size: 16px;
				padding: 13px 15px;
				height: 44px;
				-webkit-box-sizing: border-box;
				box-sizing: border-box
			}

			.main-item-login .main-item-content {
				height: 90px;
				display: -webkit-box;
				display: -ms-flexbox;
				display: flex;
				-webkit-box-pack: center;
				-ms-flex-pack: center;
				justify-content: center;
				-webkit-box-align: center;
				-ms-flex-align: center;
				align-items: center;
				font-size: 37px
			}

			.main-item-notice {
				height: 265px
			}
			.text-left-left {
				float: left;
			}
			.main-item-notice .main-item-content {
				display: -webkit-box;
				display: -ms-flexbox;
				display: flex;
				height: 90px;
				margin-top: 45px;
			}

			.main-notice-item {
				display: -webkit-box;
				display: -ms-flexbox;
				display: flex;
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1;
				-ms-flex-direction: column;
				flex-direction: column;
				-webkit-box-align: center;
				-ms-flex-align: center;
				align-items: center;
				-webkit-box-pack: center;
				-ms-flex-pack: center;
				justify-content: center
			}

			.main-notice-item .main-notice-num {
				font-size: 37px
			}

			.main-notice-item .main-notice-txt {
				font-size: 13px
			}

			.main-item-chat .main-item-content {
				padding: 0 20px;
				display: -webkit-box;
				display: -ms-flexbox;
				display: flex;
				-webkit-box-pack: center;
				-ms-flex-pack: center;
				justify-content: center;
				-webkit-box-align: center;
				-ms-flex-align: center;
				align-items: center;
				text-align: center
			}

			.main-chat-r,
			.main-chat-rb {
				display: -webkit-box;
				display: -ms-flexbox
			}

			.main-chat-l {
				width: 264px
			}

			.main-chat-r {
				width: 163px;
				display: flex;
				-ms-flex-direction: column;
				flex-direction: column;
				-webkit-box-pack: justify;
				-ms-flex-pack: justify;
				justify-content: space-between;
				height: 210px
			}

			.main-chat-rt {
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1
			}

			.main-chat-rb {
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1;
				display: flex;
				-webkit-box-orient: vertical;
				-webkit-box-direction: normal;
				-ms-flex-direction: column;
				flex-direction: column;
				-webkit-box-align: center;
				-ms-flex-align: center;
				align-items: center;
				-webkit-box-pack: center;
				-ms-flex-pack: center;
				justify-content: center
			}

			.main-item-chart,
			.main-item-chart .main-item-content {
				display: -webkit-box;
				display: -ms-flexbox;
				-webkit-box-orient: vertical;
				-webkit-box-direction: normal
			}

			.main-item-chart {
				display: flex;
				-ms-flex-direction: column;
				flex-direction: column
			}

			.main-item-chart .main-item-content {
				display: flex;
				-ms-flex-direction: column;
				flex-direction: column;
				padding: 0 20px
			}

			.main-item-chart .main-chart-t {
				display: -webkit-box;
				display: -ms-flexbox;
				display: flex;
				text-align: center;
				height: 60px
			}

			.main-item-chart .main-chart-t-item {
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1
			}

			.main-item-chart .main-chart-b {
				display: -webkit-box;
				display: -ms-flexbox;
				display: flex
			}

			.main-item-chart .main-chart-b-item {
				display: -webkit-box;
				display: -ms-flexbox;
				display: flex;
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1;
				text-align: center;
				-webkit-box-pack: center;
				-ms-flex-pack: center;
				justify-content: center;
				-webkit-box-orient: vertical;
				-webkit-box-direction: normal;
				-ms-flex-direction: column;
				flex-direction: column
			}

			.main-item-chart .dashboard-pie {
				width: 150px !important;
				flex: none
			}

			.main-item-chart .main-chart-b-item:first-child {
				width: 130px;
				-webkit-box-flex: 0;
				-ms-flex: none;
				flex: none
			}

			.main-item-inkind .main-chart-b-item:first-child {
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1
			}

			.main-chart-scalewrap {
				font-size: 13px;
				margin-bottom: 5px
			}

			.main-chart-scalewrap p {
				margin-bottom: 2px
			}

			.chart-item-circle {
				width: 10px;
				height: 10px;
				border-radius: 50%;
				display: inline-block;
				margin-right: 5px
			}

			.main-item-event .main-progress,
			.main-item-inkind .main-inkind-t {
				display: -webkit-box;
				display: -ms-flexbox;
				-webkit-box-direction: normal
			}
			.main-item-inkind{
				height:390px
			}
			.chart-item-circle-green {
				background: rgba(162, 207, 112, 1)
			}

			.chart-item-circle-blue {
				background: rgba(0, 149, 193, 1)
			}

			.chart-item-circle-yellow {
				background: rgba(255, 205, 81, 1)
			}

			.main-item-inkind .main-inkind-t {
				display: flex;
				-webkit-box-orient: horizontal;
				-ms-flex-direction: row;
				flex-direction: row;
				-webkit-box-align: center;
				-ms-flex-align: center;
				align-items: center;
				height: 50px;
				margin-bottom: 20px
			}

			.main-inkind-t .font-size-13 {
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1
			}

			.main-inkind-t .font-size-37 {
				-webkit-box-flex: 3;
				-ms-flex: 3;
				flex: 3;
				padding-left: 70px
			}

			.main-item-inkind .main-chart-b {
				margin: 0 auto;
				width: 95%
			}

			.main-item-event .main-progress {
				display: flex;
				-webkit-box-orient: vertical;
				-ms-flex-direction: column;
				flex-direction: column;
				padding: 0 5px;
				-webkit-box-flex: 0;
				-ms-flex: none;
				flex: none;
				width: 150px
			}

			.main-progress .main-progress-t {
				display: -webkit-box;
				display: -ms-flexbox;
				display: flex;
				height: 30px
			}

			.main-progress-t .font-size-13 {
				text-align: left
			}

			.main-progress-b .main-progress-wrap {
				height: 6px;
				line-height: 0;
				background: #F1F1F1;
				border-radius: 3px;
				margin-top: 8px;
				margin-bottom: 4px
			}

			.main-progress-b .main-progress-yes {
				background: #0095C0;
				display: inline-block;
				height: 100%;
				border-top-left-radius: 3px;
				border-bottom-left-radius: 3px
			}

			.main-progress-b .main-progress-no {
				background: #FFCD51;
				display: inline-block;
				height: 100%;
				border-top-right-radius: 3px;
				border-bottom-right-radius: 3px
			}

			.main-progress .main-progress-b {
				-webkit-box-flex: 1;
				-ms-flex: 1;
				flex: 1
			}

			.main-progress .main-rate-wrap {
				font-size: 12px
			}
		</style>
	</head>
	<!--  wkhtmltopdf -s A4 -T 0 -B 0 -L 0 -R 0 --image-dpi 150 --javascript-delay 2000 dashboardPdf.html ***.pdf -->
	<body>
		<div class="main">
			<header class="header padding-lr">
				<h1>
					<p id="agencyName">{m936}</p>
					<p>{m936}</p>
				</h1>
				<div class="line-wrap padding-lr">
					<img src="https://s3.amazonaws.com/com.learning-genie.cdn/images/dashboard-line.png" alt="">
				</div>
			</header>
			<menu class="menu padding-lr" id="menu">
				<!-- <div class="menu-item">
            <div class="menu-item-head">Agency</div>
            <div class="menu-item-content">Learning Genie Academy</div>
        </div>
        <div class="menu-item">
            <div class="menu-item-head">Center</div>
            <div class="menu-item-content">Happy School</div>
        </div>
        <div class="menu-item">
            <div class="menu-item-head">Class</div>
            <div class="menu-item-content">Happy Class</div>
        </div> -->
			</menu>
			<section class="arc-wrap line-wrap">
				<img src="https://s3.amazonaws.com/com.learning-genie.cdn/images/dashboard-arc.png" alt="">
			</section>
			<section class="dashboard-date" id="menuDate">
				<!-- <p>08/07/2018-09/07/2018</p> -->
			</section>
			<main class="dashboard-main">
				<div class="main-row padding-lr" style="margin-bottom: 30px;">
					<!-- register -->
					<!--<div class="main-col" id="registWrap">-->
					<!--<div class="main-item-login">-->
					<!--<div class="main-login-half main-login-family">-->
					<!--<div class="main-item-head">{m940}</div>-->
					<!--<div class="main-item-content" id="totalFamily">0</div>-->
					<!--</div>-->
					<!--<div class="main-login-half main-login-child" id="registHalfWrap" style="margin-left:10px;display: block;width: 229px;">-->
					<!--<div class="main-item-head">{m941}</div>-->
					<!--<div class="main-item-content" id="totalChild">0</div>-->
					<!--</div>-->
					<!--</div>-->
					<!--</div>-->
					<!--<div class="main-col" id="registChildWrap" style="display: none">-->
					<!--<div class="main-item-login">-->
					<!--<div class="main-login-half main-login-child">-->
					<!--<div class="main-item-head">{m941}</div>-->
					<!--<div class="main-item-content" id="totalChild1">0</div>-->
					<!--</div>-->
					<!--</div>-->
					<!--</div>-->
					<!-- active -->
					<div class="main-col" id="activeWrap">
						<div class="main-item">
							<div class="main-item-head" id="activeTxt">Daily Active Parent App Users</div>
							<div class="dashboard-bar" id="actUser" style="width: 100%;height: 220px;"></div>
						</div>
					</div>
					<!-- notification -->
					<div class="main-col-right" id="noticeWrap">
						<div class="main-item main-item-notice">
							<div class="main-item-head">{m942}</div>
							<div class="main-item-content">
								<div class="main-notice-item border-right" style="width: 233px;">
									<div class="main-notice-num" id="noticeNum">0</div>
									<div class="main-notice-txt">{m943}</div>
								</div>
								<div class="main-notice-item" style="width: 233px;">
									<div class="main-notice-num"><span id="noticeRate">0</span>%</div>
									<div class="main-notice-txt">{m944}</div>
								</div>
							</div>
						</div>
					</div>
					<!-- two way -->
					<div class="main-col" id="chatWrap">
						<div class="main-item main-item-chat">
							<div class="main-item-head">{m948}</div>
							<div class="main-item-content">
								<div class="main-chat-l">
									<div class="font-size-37" id="chatMsg">0</div>
									<div class="font-size-13">{m949}</div>
									<div class="font-size-13 color-gray">{m950}</div>
								</div>
								<div class="main-chat-r">
									<div class="main-chat-rt" style="margin-top:15px;">
										<div class="font-size-27" id="chatTranFamily">0</div>
										<div class="font-size-13" style="margin-top:8px;">{m951}</div>
									</div>
									<div class="main-chat-rb border-top">
										<div class="font-size-27" id="chatTranMsg">0</div>
										<div class="font-size-13" style="margin-top:8px;">{m952}</div>
									</div>
								</div>
							</div>
						</div>
					</div>
					<!-- event -->
					<div class="main-col-right" id="eventWrap">
						<div class="main-item main-item-chart main-item-event">
							<div class="main-item-head">{m953}</div>
							<div class="main-item-content">
								<div class="main-chart-t margin-b">
									<div class="main-chart-t-item padding-lr-5" style="width: 105px;">
										<div class="font-size-30" id="eventTotal">0</div>
										<div class="font-size-13">{m954}</div>
									</div>
									<div class="main-chart-t-item border-right border-left main-progress" style="width: 210px;">
										<div class="main-progress-t" style="margin-bottom: 10px;">
											<div class="font-size-13" style="width: 120px;">{m956}</div>
											<div class="font-size-30" style="width: 60px;text-align: right;"><span id="eventAvgRate">0</span>%</div>
										</div>
										<div class="main-progress-b" style="height: 32px;">
											<div class="main-progress-wrap">
												<span class="main-progress-yes fl" id="resYesRateBar" style="width: 90%"></span>
												<span class="main-progress-no fl" id="resNoRateBar" style="width: 10%"></span>
											</div>
											<div class="main-rate-wrap">
												<span class="main-rate-yes fl">{m957} <span style="color:#0095C0;"><b id="resYesRateNum">0</b><b>%</b></span></span>
												<span class="main-rate-no fr">{m958} <span style="color:#FFCD51;"><b id="resNoRateNum">0</b><b>%</b></span></span>
											</div>
										</div>
									</div>
									<div class="main-chart-t-item" style="width: 126px;">
										<div class="font-size-30"><span id="eventAvgParRate">0</span>%</div>
										<div class="font-size-13">{m959}</div>
									</div>
								</div>
								<div class="main-chart-b">
									<div class="main-chart-b-item main-chart-b-first">
										<div class="font-size-27" id="eventTolNum">0</div>
										<div class="font-size-13">{m960}</div>
									</div>
									<div class="dashboard-pie main-chart-b-item" id="eventPie" style="width: 100%;height: 130px;"></div>
									<div class="main-chart-b-item" style="width: 147px;margin-top:15px;">
										<div class="main-chart-scalewrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-green"></i>{m961}:</p>
											<span class="fontw600 fl padding-l" id="eventFathNum">0</span>
											<span class="fontw600 fr padding-r"><span id="eventFathRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-blue"></i>{m962}:</p>
											<span class="fontw600 fl padding-l" id="eventMothNum">0</span>
											<span class="fontw600 fr padding-r"><span id="eventMothRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap" id="eventOthWrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-yellow"></i>{m963}:</p>
											<span class="fontw600 fl padding-l" id="eventOthNum">0</span>
											<span class="fontw600 fr padding-r"><span id="eventOthRate">0</span>%</span>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
					<!-- media book-->
					<div class="main-col">
						<div class="main-item main-item-chart">
							<div class="main-item-head">{m968}</div>
							<div class="main-item-content">
								<div class="main-chart-t margin-b">
									<div class="main-chart-t-item" style="width: 140px;">
										<div class="font-size-30" id="bookTotal">0</div>
										<div class="font-size-13">{m969}</div>
									</div>
									<div class="main-chart-t-item border-right border-left" style="width: 140px;">
										<div class="font-size-30" id="bookTolWord">0</div>
										<div class="font-size-13">{m970}</div>
									</div>
									<div class="main-chart-t-item" style="width: 140px;">
										<div class="font-size-30" id="bookAvgRead">0</div>
										<div class="font-size-13">{m971}</div>
									</div>
								</div>
								<div class="main-chart-b">
									<div class="main-chart-b-item main-chart-b-first">
										<div><span class="font-size-27" id="bookTolHour">0</span>&nbsp;hrs</div>
										<div class="font-size-13">{m972}</div>
									</div>
									<div class="dashboard-pie main-chart-b-item" id="bookPie" style="width: 100%;height: 130px;"></div>
									<div class="main-chart-b-item" style="width: 147px;margin-top:10px;">
										<div class="main-chart-scalewrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-green"></i>{m961}:</p>
											<span class="fl padding-l"><span class="fontw600" id="bookFathNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="bookFathRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-blue"></i>{m962}:</p>
											<span class="fl padding-l"><span class="fontw600" id="bookMothNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="bookMothRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap" id="bookOthWrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-yellow"></i>{m963}:</p>
											<span class="fl padding-l"><span class="fontw600" id="bookOthNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="bookOthRate">0</span>%</span>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
					<!-- in-kind -->
					<div class="main-col-right" id="inKindWrap" style="display:block">
						<div class="main-item main-item-chart main-item-inkind">
							<div class="main-item-head">{m964}</div>
							<div class="main-item-content">
								<div class="main-chart-t margin-b">
									<div class="main-chart-t-item" style="width: 140px;">
										<div class="font-size-30" id="kindTolEntries">0</div>
										<div class="font-size-13">{m976}</div>
									</div>
									<div class="main-chart-t-item border-right border-left" style="width: 140px;">
										<div><span class="font-size-30" id="kindTolTimes">0</span>&nbsp;hrs</div>
										<div class="font-size-13">{m977}</div>
									</div>
									<div class="main-chart-t-item" style="width: 140px;">
										<div class="font-size-30" id="kindTolVal">$<span>0</span></div>
										<div class="font-size-13">{m978}</div>
									</div>
								</div> 
								<!-- <div class="main-inkind-t">
                            <div class="font-size-13">{m965}:</div>
                            <div class="font-size-37" id="kindTolVal">$<span>0</span></div>
                        </div> -->
								<div class="main-chart-b">
									<div class="dashboard-pie main-chart-b-item" id="inKindPieOfSource" style="width: 100%;height: 130px;"></div>
									<div class="main-chart-b-item">
										<div class="main-chart-scalewrap">
											<p class="text-left full">{m979}</p>
										</div>
										<div class="main-chart-scalewrap">
											<p class="text-left-left"><i class="chart-item-circle chart-item-circle-green"></i>{m966}:</p>
											<span class="fl"><span class="fontw600"id="kindHomeNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="kindHomeRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap">
											<p class="text-left-left"><i class="chart-item-circle chart-item-circle-blue"></i>{m967}:</p>
											<span class="fl"><span class="fontw600"  id="kindVolunteerNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="kindVolunteerRate">0</span>%</span>
										</div>
									</div>
								</div>
								<div class="main-chart-b">
									<div class="dashboard-pie main-chart-b-item" id="inKindPieOfActivity" style="width: 100%;height: 130px;"></div>
									<div class="main-chart-b-item">
										<div class="main-chart-scalewrap">
											<p class="text-left full">{m980}</p>
										</div>
										<div class="main-chart-scalewrap">
											<p class="text-left-left"><i class="chart-item-circle chart-item-circle-green"></i>{m981}:</p>
											<span class="fl"><span class="fontw600"  id="kindFFNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="kindFFRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap">
											<p class="text-left-left "><i class="chart-item-circle chart-item-circle-blue"></i>{m982}:</p>
											<span class=" fl"><span class="fontw600"  id="kindMFNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="kindMFRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap">
											<p class="text-left-left "><i class="chart-item-circle chart-item-circle-yellow"></i>{m983}Other:</p>
											<span class="fl"><span class="fontw600" id="kindOtherNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="kindOtherRate">0</span>%</span>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>

					<!-- learning media -->
					<div class="main-col">
						<div class="main-item main-item-chart">
							<div class="main-item-head">{m973}</div>
							<div class="main-item-content">
								<div class="main-chart-t margin-b">
									<div class="main-chart-t-item">
										<div class="font-size-30" id="mediaTotal">0</div>
										<div class="font-size-13">{m974}</div>
									</div>
									<div class="main-chart-t-item border-left">
										<div class="font-size-30" id="mediaTolView">0</div>
										<div class="font-size-13">{m975}</div>
									</div>
								</div>
								<div class="main-chart-b">
									<div class="main-chart-b-item main-chart-b-first">
										<div><span class="font-size-27" id="mediaHour">13</span>&nbsp;hr</div>
										<div class="font-size-13">{m976}</div>
									</div>
									<div class="dashboard-pie main-chart-b-item" id="mediaPie" style="width: 100%;height: 130px;"></div>
									<div class="main-chart-b-item" style="width: 147px;margin-top:10px;">
										<div class="main-chart-scalewrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-green"></i>{m961}:</p>
											<span class="fl padding-l"><span class="fontw600" id="mediaFathNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="mediaFathRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-blue"></i>{m962}:</p>
											<span class="fl padding-l"><span class="fontw600" id="mediaMothNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="mediaMothRate">0</span>%</span>
										</div>
										<div class="main-chart-scalewrap" id="mediaOthWrap">
											<p class="text-left full"><i class="chart-item-circle chart-item-circle-yellow"></i>{m963}:</p>
											<span class="fl padding-l"><span class="fontw600" id="mediaOthNum">0</span>&nbsp;hrs</span>
											<span class="fontw600 fr padding-r"><span id="mediaOthRate">0</span>%</span>
										</div>
									</div>
								</div>
							</div>
						</div>
					</div>
				</div>
			</main>
		</div>
		<script>
			var json_data={
	"agencyName": "Moonlight (Test)",
	"centerName": "所有学校",
	"centerCount": 26,
	"className": "所有班级",
	"classCount": 89,
	"fromData": "10/20/2019",
	"toData": "11/19/2019",
	"noticeOpen": true,
	"chatOpen": true,
	"eventOpen": true,
	"inKindOpen": true,
	"registerStats": {
		"totalFamily": 58,
		"totalChild": 159
	},
	"activeStats": {
		"type": "DAY",
		"loginCounts": [{
			"date": "Jan",
			"count": 50
		}, {
			"date": "Feb",
			"count": 100
		}, {
			"date": "Mar",
			"count": 500
		}, {
			"date": "Apr",
			"count": 1000
		}, {
			"date": "May",
			"count": 10000
		}, {
			"date": "Jun",
			"count": 550
		}, {
			"date": "Jul",
			"count": 1000
		}, {
			"date": "Aug",
			"count": 500
		}, {
			"date": "Sept",
			"count": 3700
		}]
	},
	"notificationStats": {
		"totalNotification": 50,
		"averageOpenRate": 5.0
	},
	"chatStats": {
		"totalMessage": 78,
		"totalTranslatedFamily": 2,
		"totalTranslatedMessage": 39
	},
	"eventStats": {
		"totalEvent": 5,
		"avgResponseRate": 10.0,
		"responseAttendRate": 100.0,
		"responseNotAttendRate": 0.0,
		"avgAttendRate": 5.0,
		"totalAttendNum": 20,
		"fatherAttendNum": 20,
		"fatherAttendRate": 6.0,
		"motherAttendNum": 10,
		"motherAttendRate": 13.0,
		"otherAttendNum": 20,
		"otherAttendRate": 24.0
	},
	"inKindStats": {
		"totalEntries":100,
		"totalTime":20,
		"totalValue": 393,
		"atHomeValue": 363,
		"atHomeRate": 92.4,
		"inSchoolValue": 30,
		"inSchoolRate": 7.6,
		"fatherFigure":25,
		"fatherFigureRate":5.7,
		"matherFigure":55,
		"matherFigureRate":4.3,
		"kindOtherNum":10,
		"kindOtherRate":3.5
	},
	"bookStats": {
		"totalBook": 100,
		"totalWord": 100,
		"avgReadLevel": 5.0,
		"totalHour": 6.0,
		"fatherNum": 7.0,
		"fatherRate": 5.0,
		"motherNum": 5.0,
		"motherRate": 2.0,
		"otherNum": 5.0,
		"otherRate": 3.0
	},
	"mediaStats": {
		"totalVideo": 10,
		"totalView": 20,
		"totalHour": 5.0,
		"fatherNum": 6.0,
		"fatherRate": 6.0,
		"motherNum": 7.0,
		"motherRate": 7.0,
		"otherNum": 5.0,
		"otherRate": 6.0
	},
	"language": "US"
}
			var jsonData = json_data
			console.log(jsonData)

			// 初始化
			init()

			function init() {
				// 设置机构、学校、班级
				setMenu()
				// 设置起止日期
				setMenuDate()
				// 日活/月活/年活
				setActive()
				// book
				setBook()
				// learning Media
				setMedia()
				// 学校通知
				// jsonData.noticeOpen = true;

				setVal('agencyName', jsonData.agencyName)
				if (jsonData.noticeOpen) {
					// document.getElementById('registHalfWrap').style.display = 'block'
					document.getElementById('noticeWrap').setAttribute('style', 'display:block')
					// document.getElementById('registChildWrap').setAttribute('style','display:none')
					setVal('noticeNum', numFormat(jsonData.notificationStats.totalNotification))
					setVal('noticeRate', jsonData.notificationStats.averageOpenRate)
					// 注册统计
					// setVal('totalFamily',numFormat(jsonData.registerStats.totalFamily))
					// setVal('totalChild',numFormat(jsonData.registerStats.totalChild))
				} else {
					// document.getElementById('registHalfWrap').style.display = 'none'
					document.getElementById('noticeWrap').setAttribute('style', 'display:none')
					// document.getElementById('registChildWrap').setAttribute('style','display:block')
					// 注册统计
					// setVal('totalFamily',numFormat(jsonData.registerStats.totalFamily))
					// setVal('totalChild1',numFormat(jsonData.registerStats.totalChild))
				}
				// 双向沟通
				if (jsonData.chatOpen) {
					setVal('chatMsg', numFormat(jsonData.chatStats.totalMessage))
					setVal('chatTranFamily', numFormat(jsonData.chatStats.totalTranslatedFamily))
					setVal('chatTranMsg', numFormat(jsonData.chatStats.totalTranslatedMessage))
				} else {
					document.getElementById('chatWrap').setAttribute('style', 'display:none')
				}
				// event
				if (jsonData.eventOpen) {
					setEvent()
				} else {
					document.getElementById('eventWrap').setAttribute('style', 'display:none')
				}
				// in-kinde
				if (jsonData.inKindOpen) {
					setInKind()
				} else {
					document.getElementById('inKindWrap').setAttribute('style', 'display:none')
				}
				var language = jsonData.language;
				// (language.indexOf("es") != -1) {
				//$('#menu .menu-item').css("padding-right",'18px');
				//$('#menu .menu-item').css("margin-left",'0px');
				// var elements = document.getElementsByClassName('menu-item');
				// for(var i in elements) {
				//     elements[i].css("padding-right",'18px');
				// }
				//}
			}

			function setMenu() {
				var el = document.getElementById('menu');
				var data = jsonData;
				var centerCount = data.centerCount;
				var classCount = data.classCount;
				var centerCountStr = "";
				var classCountStr = "";
				if (centerCount > 1) {
					centerCountStr = "(" + centerCount + ")";
				}
				if (classCount > 1) {
					classCountStr = "(" + classCount + ")";
				}
				// var str='<div class="menu-item">'+
				// '<div class="menu-item-head">Agency</div>'+
				// ' <div class="menu-item-content">'+data.agencyName+'</div>'+
				// '</div>'+
				var str = '<div style="overflow: hidden;width:100%;"><div class="menu-item fl" style="width:70%;">' +
					'<div class="menu-item-head fl">{m938}</div>' +
					'<div class="menu-item-content fl">' + data.centerName + centerCountStr + '</div>' +
					'</div>' +
					'<div class="menu-item fr" style="width:30%;padding-right:0;">' +
					'<div class="menu-item-head fl" style="color:#0095C1;text-align:left;width:auto">{m940}</div>' +
					'<div class="menu-item-content fl" style="text-align:center;width:auto;margin-left:15px;">' + data.registerStats.totalFamily +
					'</div>' +
					'</div></div>' +
					'<div style="overflow: hidden;width:100%;"><div class="menu-item fl" style="width:70%;">' +
					'<div class="menu-item-head fl">{m939}</div>' +
					'<div class="menu-item-content fl">' + data.className + classCountStr + '</div>' +
					'</div>' +
					'<div class="menu-item fr" style="padding-right:0;width:30%;">' +
					'<div class="menu-item-head fl" style="color:#0095C1;text-align:left;width:auto">{m941}</div>' +
					'<div class="menu-item-content fl" style="text-align:center;width:auto;margin-left:15px;">' + data.registerStats.totalChild +
					'</div>' +
					'</div></div>'
				if (data.centerName == "" && data.agencyName == "") {
					str = '<div style="overflow: hidden;width:100%;"><div class="menu-item fl" style="width:70%;">' +
						'<div class="menu-item-head fl">{m938}</div>' +
						'<div class="menu-item-content fl">' + data.centerName + centerCountStr + '</div>' +
						'</div>' +
						'<div class="menu-item fr" style="width:30%;padding-right:0;">' +
						'<div class="menu-item-head fl" style="color:#0095C1;text-align:left;width:auto">{m940}</div>' +
						'<div class="menu-item-content fl" style="text-align:center;width:auto;margin-left:15px;">' + data.registerStats.totalFamily +
						'</div>' +
						'</div></div>' +
						'<div style="overflow: hidden;width:100%;"><div class="menu-item fl" style="width:70%;">' +
						'<div class="menu-item-head fl">{m939}</div>' +
						'<div class="menu-item-content fl">' + data.className + classCountStr + '</div>' +
						'</div>' +
						'<div class="menu-item fr" style="padding-right:0;width:30%;">' +
						'<div class="menu-item-head fl" style="color:#0095C1;text-align:left;width:auto">{m941}</div>' +
						'<div class="menu-item-content fl" style="text-align:center;width:auto;margin-left:15px;">' + data.registerStats.totalChild +
						'</div>' +
						'</div></div>'
				}
				el.innerHTML = str
			}

			function setMenuDate() {
				var el = document.getElementById('menuDate');
				var data = jsonData;
				var str = '<p>' + jsonData.fromData + '-' + jsonData.toData + '</p>'
				el.innerHTML = str;
			}

			function setVal(id, val) {
				var el = document.getElementById(id);
				el.innerHTML = val;
			}
			// 日活
			function setActive() {
				var data = jsonData.activeStats;
				var activeTxt = '';
				switch (data.type) {
					case 'YEAR':
						activeTxt = '{m945}';
						break;
					case 'MONTH':
						activeTxt = '{m946}';
						break;
					default:
						activeTxt = '{m947}';
						break;
				}
				setVal('activeTxt', activeTxt);
				var isShowy = true;
				var actUserData = data.loginCounts;
				var dataX = getDataX(actUserData);
				var dataY = getDataY(actUserData);
				var dataShadow = getDataShadow(dataY);
				var maxY = Math.max.apply(null, dataY);
				isShowy = maxY > 0 ? true : false;
				var actUserOption = {
					animation: false,
					grid: {
						top: '30',
						left: '50',
						right: '30',
						bottom: '30'
					},
					// tooltip: {},
					xAxis: {
						data: dataX,
						axisLabel: {},
						axisTick: {
							show: false
						},
						axisLine: {
							show: false
						},
						z: 10
					},
					yAxis: {
						axisLine: {
							show: false
						},
						axisTick: {
							show: false
						},
						axisLabel: {
							show: isShowy,
							textStyle: {
								color: '#58666E'
							}
						},
						splitLine: {
							lineStyle: {
								color: '#f1f1f1'
							}
						}
					},
					series: [{ // For shadow
							type: 'bar',
							itemStyle: {
								normal: {
									// color: 'rgba(241,241,241,1)'
									color: 'none'
								}
							},
							barGap: '-100%',
							barCategoryGap: '40%',
							data: dataShadow,
							animation: false,
							silent: true
						},
						{
							type: 'bar',
							itemStyle: {
								normal: {
									color: '#0095C1'
								},
								emphasis: {
									color: '#05a3d2'
								}
							},
							data: dataY
						}
					]
				}
				echarts.init(document.getElementById('actUser')).setOption(actUserOption);
			}

			function getDataX(actUserData) {
				var arr = [];
				for (var index = 0; index < actUserData.length; index++) {
					var el = actUserData[index];
					arr.push(el.date);
				}
				return arr;
			}

			function getDataY(actUserData) {
				var arr = [];
				for (var index = 0; index < actUserData.length; index++) {
					var el = actUserData[index];
					arr.push(el.count);
				}
				return arr;
			}

			function getDataShadow(dataY) {
				var num = dataY;
				var max = Math.max.apply(null, num);
				// var yMax = computedMax(max);
				var yMax = Math.ceil(max / 5) * 5
				yMax = yMax < 5 ? 5 : yMax
				var arr = [];
				for (var i = 0; i < num.length; i++) {
					arr.push(yMax)
				}
				return arr;
			}

			function setEvent() {
				var data = jsonData.eventStats;
				setVal('eventTotal', numFormat(data.totalEvent));
				setVal('eventAvgRate', data.avgResponseRate);
				setVal('resYesRateNum', data.responseAttendRate);
				setVal('resNoRateNum', data.responseNotAttendRate);
				setVal('eventAvgParRate', data.avgAttendRate);
				setVal('eventTolNum', numFormat(data.totalAttendNum));

				setVal('eventFathNum', numFormat(data.fatherAttendNum));
				setVal('eventFathRate', data.fatherAttendRate);
				setVal('eventMothNum', numFormat(data.motherAttendNum));
				setVal('eventMothRate', data.motherAttendRate);

				if (data.otherAttendNum == '0') {
					// document.getElementById('eventOthWrap').setAttribute('style','display:none')
				} else {
					setVal('eventOthNum', numFormat(data.otherAttendNum));
					setVal('eventOthRate', data.otherAttendRate);
				}
				document.getElementById('resYesRateBar').style.width = data.responseAttendRate + '%';
				document.getElementById('resNoRateBar').style.width = data.responseNotAttendRate + '%';
				var eventData = {
					noData: false,
					data: [{
							value: data.fatherAttendNum,
							name: 'father'
						},
						{
							value: data.motherAttendNum,
							name: 'mother'
						},
						{
							value: data.otherAttendNum,
							name: 'other'
						}
					]
				}
				if (data.fatherAttendNum == '0' && data.motherAttendNum == '0' && data.otherAttendNum == '0') {
					eventData.noData = true
				}
				echarts.init(document.getElementById('eventPie')).setOption(pieOption(eventData));
			}

			function setInKind() {
				var data = jsonData.inKindStats;
				setVal('kindTolEntries',numFormat(data.totalEntries));
				setVal('kindTolTimes', numFormat(data.totalTime));
				setVal('kindTolVal', '$' + numFormat(data.totalValue));
				setVal('kindHomeNum', numFormat(data.atHomeValue));
				setVal('kindHomeRate', data.atHomeRate);
				setVal('kindVolunteerNum', numFormat(data.inSchoolValue));
				setVal('kindVolunteerRate', data.inSchoolRate);
				setVal('kindFFNum',numFormat(data.fatherFigure))  //faster figure
				setVal('kindFFRate',data.fatherFigureRate)
				setVal('kindMFNum',numFormat(data.matherFigure)) //mother figure
				setVal('kindMFRate',data.matherFigureRate)
				if (data.otherNum == '0') {
					
				} else {
					setVal('kindOtherNum',numFormat(data.kindOtherNum))
				}
				
				if (data.otherRate == '0') {
					
				} else {
					setVal('kindOtherRate',data.kindOtherRate)
				}
				var chartData = {
					noData: false,
					data: [{
							value: data.atHomeValue,
							name: 'Home'
						},
						{
							value: data.inSchoolValue,
							name: 'School'
						}
					]
				}
				var chartDataTwo={
					noData:false,
					data:[{
							value:data.fatherFigure,
							name:'Father'
						},
						{
							value:data.matherFigure,
							name:'Mother'
						},
						{
							value:data.kindOtherNum,
							name:'Other'
						}
					]
				}
				if (data.atHomeValue == '0' && data.inSchoolValue == '0') {
					chartData.noData = true
				}
				echarts.init(document.getElementById('inKindPieOfSource')).setOption(pieOption(chartData));
				echarts.init(document.getElementById("inKindPieOfActivity")).setOption(pieOption(chartDataTwo))
			}

			function setBook() {
				var data = jsonData.bookStats;
				setVal('bookTotal', numFormat(data.totalBook));
				setVal('bookTolWord', numFormat(data.totalWord));
				setVal('bookAvgRead', numFormat(data.avgReadLevel));

				setVal('bookTolHour', numFormat(data.totalHour));

				setVal('bookFathNum', numFormat(data.fatherNum));
				setVal('bookFathRate', data.fatherRate);
				setVal('bookMothNum', numFormat(data.motherNum));
				setVal('bookMothRate', data.motherRate);

				if (data.otherNum == '0') {
					// document.getElementById('bookOthWrap').setAttribute('style','display:none')
				} else {
					setVal('bookOthNum', numFormat(data.otherNum));
				}

				if (data.otherRate == '0') {
					// document.getElementById('bookOthWrap').setAttribute('style','display:none')
				} else {
					setVal('bookOthRate', data.otherRate);
				}
				var chartData = {
					noData: false,
					data: [{
							value: data.fatherNum,
							name: 'father'
						},
						{
							value: data.motherNum,
							name: 'mother'
						},
						{
							value: data.otherNum,
							name: 'other'
						}
					]
				}
				if (data.fatherNum == '0' && data.motherNum == '0' && data.otherNum == '0') {
					chartData.noData = true
				}
				echarts.init(document.getElementById('bookPie')).setOption(pieOption(chartData));
			}

			function setMedia() {
				var data = jsonData.mediaStats;
				setVal('mediaTotal', numFormat(data.totalVideo));
				setVal('mediaTolView', numFormat(data.totalView));

				setVal('mediaHour', numFormat(data.totalHour));

				setVal('mediaFathNum', numFormat(data.fatherNum));
				setVal('mediaFathRate', data.fatherRate);
				setVal('mediaMothNum', numFormat(data.motherNum));
				setVal('mediaMothRate', data.motherRate);

				if (data.otherNum == '0') {
					// document.getElementById('mediaOthWrap').setAttribute('style','display:none')
				} else {
					setVal('mediaOthNum', numFormat(data.otherNum));
				}

				if (data.otherRate == '0') {
					// document.getElementById('mediaOthWrap').setAttribute('style','display:none')
				} else {
					setVal('mediaOthRate', data.otherRate);
				}

				var chartData = {
					noData: false,
					data: [{
							value: data.fatherNum,
							name: 'father'
						},
						{
							value: data.motherNum,
							name: 'mother'
						},
						{
							value: data.otherNum,
							name: 'other'
						}
					]
				}
				if (data.fatherNum == '0' && data.motherNum == '0' && data.otherNum == '0') {
					chartData.noData = true
				}
				echarts.init(document.getElementById('mediaPie')).setOption(pieOption(chartData));
			}

			function computedMax(num) {
				var temp = num;
				var arr = String(num).split('');
				var scale = Math.pow(10, arr.length - 1);
				temp = Math.ceil(temp / scale) * scale;
				if (temp == '0') {
					temp = 10
				}
				return temp
			}

			function pieOption(chartData) {
				var noData = chartData.noData;
				var data = chartData.data;
				var pieOption = {
					animation: false,
					tooltip: {},
					series: [{
							// name: this.origin.name,
							type: 'pie',
							radius: ['50%', '80%'],
							color: noData ? ['#F1F1F1'] : ['#A2CF70', '#0095C1', '#FFCD51'],
							hoverAnimation: !noData,
							avoidLabelOverlap: false,
							label: {
								normal: {
									show: false,
									position: 'center'
								},
								emphasis: {
									show: false
								}
							},
							// emphasis: this.emphasis,
							silent: true,
							labelLine: {
								normal: {
									show: false
								}
							},
							data: noData ? [{
								value: 0
							}] : data
						},
						{
							// name: '',
							type: 'pie',
							radius: ['40%', '45%'],
							color: '#F1F1F1',
							avoidLabelOverlap: false,
							hoverAnimation: false,
							label: {
								normal: {
									show: false,
									position: 'center'
								},
								emphasis: {
									show: false
								}
							},
							silent: true,
							labelLine: {
								normal: {
									show: false
								}
							},
							tooltip: { // 禁止鼠标悬停显示提示框
								show: false
							},
							data: [{
								value: 1
							}]
						}
					]
				}
				return pieOption
			}

			function numFormat(value) {
				if (!value) return '0'
				var intPart = Number(value); // 获取整数部分
				var intPartFormat = intPart.toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,') // 将整数部分逢三一断
				return intPartFormat
			}
		</script>
	</body>

</html>

```

## echarts使用：

<https://www.echartsjs.com/en/index.html>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <!-- 引入 ECharts 文件 -->
    <!--常用版：echarts/dist/echarts.common.js，体积适中，包含常见的图表和组件，所包含内容参见：echarts/echarts.common.js。-->
    <script src="https://cdn.bootcss.com/echarts/4.2.1-rc1/echarts-en.common.js"></script>
</head>
<div id="main" style="width: 600px;height: 400px; float: left;"></div>
<div id="pie" style="width:600px;height: 300px; float: right;"></div>
<div id="pie1" style="width: 600px;height: 400px;"></div>

<div id="dendrogram" style="width: 600px;height: 800px; float: right;"></div>

<script type="text/javascript">
    // 初始化eacharts实例 使用 eacharts.init()方法
    var myChart=echarts.init(document.getElementById('main'))
    // 设置loading
    myChart.showLoading();
    // 指定图标的配置项
    var options={
        title:{
            text :'Echarts 实例',
            link:'http://echartsjs.com/zh/option.html'
        },
        tooltip:{

        },
        //图例组件展现了不同系列的标记(symbol)，颜色和名字。可以通过点击图例控制哪些系列不显示
        legend:{
            data:['销量']
        },
        // 声明一个 X 轴，类目轴（category）。默认情况下，类目轴对应到 dataset 第一列。
        xAxis:{
            data:['衬衣','裤子',"雪纺衫","裤子","高跟鞋","袜子"]
        },
        // // 声明一个 Y 轴，数值轴。
        yAxis:{},
        // // 声明多个 bar 系列，默认情况下，每个系列会自动对应到 dataset 的每一列。
        series:{
            name:'销量',
            type:'bar',
            data:[5,6,30,50,40,30]
        }

    }
    myChart.hideLoading(); // 显示loading后必须隐藏loading
    //使用制定的配置项和数据显示图标
    myChart.setOption(options)

    ///
    echarts.init(document.getElementById('pie')).setOption({
        series:{
            type:'pie',
            data:[
                {name:'A',value:1212},
                {name:'B',value:1233},
                {name:'C',value:3232}
            ]
        }
    })
    ///绘制南丁格尔图
    echarts.init(document.getElementById('pie1')).setOption({
        backgroundColor: '#2c343c', // 全局设计背景色
        series:[
            {
                name:'访问来源',
                type:'pie',
                radius:'55%',
                roseType: 'angle', // 显示为南丁格尔图
                data:[
                    {value:235,name:'联盟广告'},
                    {value:235, name:'视频广告'},
                    {value:310, name:'邮件营销'},
                    {value:335, name:'直接访问'},
                    {value:400, name:'搜索引擎'}
                ],
                // 普通样式
                itemStyle: {
                    // 阴影的大小
                    shadowBlur: 200,
                    // 阴影水平方向上的偏移
                    shadowOffsetX: 0,
                    // 阴影垂直方向上的偏移
                    shadowOffsetY: 0,
                    // 阴影颜色
                    shadowColor: 'rgba(0, 0, 0, 0.5)',
                    // //以下写法是旧版本的
                    // // 鼠标hover时高亮样式
                    // emphasis: {
                    //     shadowBlur: 200,
                    //     shadowColor: 'rgba(0, 0, 0, 0.7)',
                    //     color: 'blue',
                    //     label:{
                    //         show:true,
                    //         formatter:'this is lable'
                    //     }
                    // },
                    normal: {
                        shadowBlur: 200,
                        shadowColor: 'rgba(0, 0, 0, 0.5)'
                    },
                },
                // 高亮样式。(推荐这样书写)
                emphasis: {
                    itemStyle: {
                        // 高亮时点的颜色。
                        color: 'blue'
                    },
                    label: {
                        show: true,
                        // 高亮时标签的文字。
                        formatter: 'This is a emphasis label.'
                    }
                },
                label: {
                    normal: {
                        textStyle: {
                            color: 'rgba(255, 255, 255, 0.3)'
                        }
                    }
                },
                labelLine: {
                    normal: {
                        lineStyle: {
                            color: 'rgba(255, 255, 255, 0.3)'
                        }
                    }
                },
            }
        ]
    })
    /// 绘制基于数据，在配置项中指定映射到图形
    //指定 dataset 的列（column）还是行（row）映射为图形系列（series）。这件事可以使用 series.seriesLayoutBy 属性来配置。默认是按照列（column）来映射。
    //指定维度映射的规则：如何从 dataset 的维度（一个“维度”的意思是一行/列）映射到坐标轴（如 X、Y 轴）、提示框（tooltip）、标签（label）、图形元素大小颜色等（visualMap）。
    //这件事可以使用 series.encode 属性，以及 visualMap 组件（如果有需要映射颜色大小等视觉维度的话）来配置。上面的例子中，没有给出这种映射配置，那么 ECharts 就按最常见的理解进行默认映射：X 坐标轴声明为类目轴，默认情况下会自动对应到 dataset.source 中的第一列；三个柱图系列，一一对应到 dataset.source 中后面每一列
    echarts.init(document.getElementById('dendrogram')).setOption({
        // title : {
        //     text: '制定',
        //     x: 'center'
        // },
        backgroundColor: '#FFDAB9', // 全局设计背景色
        legend: {},
        tooltip: {},
        dataset: {
            source: [
                ['product', '2012', '2013', '2014', '2015'],
                ['Matcha Latte', 41.1, 30.4, 65.1, 53.3],
                ['Milk Tea', 86.5, 92.1, 85.7, 83.1],
                ['Cheese Cocoa', 24.1, 67.2, 79.5, 86.4]
            ]
        },
        xAxis: [
            {type: 'category', gridIndex: 0},
            {type: 'category', gridIndex: 1}
        ],
        yAxis: [
            {gridIndex: 0},
            {gridIndex: 1}
        ],
        // 直角坐标系内绘图网格，单个 grid 内最多可以放置上下两个 X 轴，左右两个 Y 轴。可以在网格上绘制折线图，柱状图，散点图（气泡图）
        grid: [
            {bottom: '55%'},
            {top: '55%'}
        ],
        series: [
          // 这几个系列会在第一个直角坐标系中，每个系列对应到 dataset 的每一行。
          //'column': 默认值。系列被安放到 dataset 的列上面。
          // row 系列被安放到 dataset 的行上面。
            {type: 'bar', seriesLayoutBy: 'row'},
            {type: 'bar', seriesLayoutBy: 'row'},
            {type: 'bar', seriesLayoutBy: 'row'},
            // 这几个系列会在第二个直角坐标系中，每个系列对应到 dataset 的每一列。
            {type: 'bar', xAxisIndex: 1, yAxisIndex: 1},
            {type: 'bar', xAxisIndex: 1, yAxisIndex: 1},
            {type: 'bar', xAxisIndex: 1, yAxisIndex: 1},
            {type: 'bar', xAxisIndex: 1, yAxisIndex: 1}
        ]

    })
</script>
</html>
```



## 记录一个Array转list异常：

![img](C:\Users\kayneth\AppData\Local\Temp\企业微信截图_15731952468763.png)

Exception in thread "main" java.lang.UnsupportedOperationException

1）常常使用Arrays.asLisvt()后调用add，remove这些method时出现java.lang.UnsupportedOperationException异常。这是由于：
Arrays.asLisvt() 返回java.util.Arrays$ArrayList， 而不是ArrayList。Arrays$ArrayList和ArrayList都是继承AbstractList，remove，add等method在AbstractList中是默认throw UnsupportedOperationException而且不作任何操作。ArrayList override这些method来对list进行操作，但是Arrays$ArrayList没有override remove(int)，add(int)等，所以throw UnsupportedOperationException。
解决方法是使用Iterator，或者转换为ArrayList

List list = Arrays.asList(a[]);
List arrayList = new ArrayList(list);

**解决方法先看下源码：**

```java
/* 
* @param <T> the class of the objects in the array
 * @param a the array by which the list will be backed
 * @return a list view of the specified array
 */
@SafeVarargs
@SuppressWarnings("varargs")
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}

/**
 * @serial include
 */
private static class ArrayList<E> extends AbstractList<E>
    implements RandomAccess, java.io.Serializable
{
    private static final long serialVersionUID = -2764017481108945198L;
    private final E[] a;

    ArrayList(E[] array) {
        a = Objects.requireNonNull(array);
    }

    @Override
    public int size() {
        return a.length;
    }

    @Override
    public Object[] toArray() {
        return a.clone();
    }

    @Override
    @SuppressWarnings("unchecked")
    public <T> T[] toArray(T[] a) {
        int size = size();
        if (a.length < size)
            return Arrays.copyOf(this.a, size,
                                 (Class<? extends T[]>) a.getClass());
        System.arraycopy(this.a, 0, a, 0, size);
        if (a.length > size)
            a[size] = null;
        return a;
    }

    @Override
    public E get(int index) {
        return a[index];
    }

    @Override
    public E set(int index, E element) {
        E oldValue = a[index];
        a[index] = element;
        return oldValue;
    }

    @Override
    public int indexOf(Object o) {
        E[] a = this.a;
        if (o == null) {
            for (int i = 0; i < a.length; i++)
                if (a[i] == null)
                    return i;
        } else {
            for (int i = 0; i < a.length; i++)
                if (o.equals(a[i]))
                    return i;
        }
        return -1;
    }

    @Override
    public boolean contains(Object o) {
        return indexOf(o) != -1;
    }

    @Override
    public Spliterator<E> spliterator() {
        return Spliterators.spliterator(a, Spliterator.ORDERED);
    }

    @Override
    public void forEach(Consumer<? super E> action) {
        Objects.requireNonNull(action);
        for (E e : a) {
            action.accept(e);
        }
    }

    @Override
    public void replaceAll(UnaryOperator<E> operator) {
        Objects.requireNonNull(operator);
        E[] a = this.a;
        for (int i = 0; i < a.length; i++) {
            a[i] = operator.apply(a[i]);
        }
    }

    @Override
    public void sort(Comparator<? super E> c) {
        Arrays.sort(a, c);
    }
}
```

在这个内部类中没有这个List接口中的remove,add,这些方法，但是使用List接收并不会出错，因为所有的在运行时都会向上转型。（转型后具有了其他的特性，编译不提示出错，运行时出错），运行时发现没有这些方法，出现异常。

```java
// array转list不支持remove add 这里的 list是Array中的内部类
List<String> lastChildIdsArray = Arrays.asList(lastChildIds);
// 需要使用 new 出一个新的Arraylist来接收新的的参数
List<String> lastChildIdslist = new ArrayList(lastChildIdsArray);
```

因为这里的返回的是一个arraylist

```java
public static <T> List<T> asList(T... a) {
        return new ArrayList<>(a);
    }
```

这个ArrayList<E> t是继承的AbstractList，AbstractList又实现了List接口：

```java
public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E> 
```

```java
//Arraylist类
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
```

所以这里是可以用来接收的。并且可以转换为一个Arraylist，那么当new 出一个Arraylist时怎样又能接收一个参数了呢（参数类型：和arraylist一个类型也就是List接口类型，或者list的子类）

```java
/**
     * The array buffer into which the elements of the ArrayList are stored.
     * The capacity of the ArrayList is the length of this array buffer. Any
     * empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     * will be expanded to DEFAULT_CAPACITY when the first element is added.
     */
    transient Object[] elementData; // non-private to simplify nested class access

    /**
     * The size of the ArrayList (the number of elements it contains).
     *
     * @serial
     */
    private int size;

    /**
     * Constructs an empty list with the specified initial capacity.
     *
     * @param  initialCapacity  the initial capacity of the list
     * @throws IllegalArgumentException if the specified initial capacity
     *         is negative
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }

    /**
     * Constructs an empty list with an initial capacity of ten.
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    /**
     * Constructs a list containing the elements of the specified
     * collection, in the order they are returned by the collection's
     * iterator.
     *
     * @param c the collection whose elements are to be placed into this list
     * @throws NullPointerException if the specified collection is null
     */
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```

实际上是一个Collection或其子类而List又是Collection的子类

```java
public interface List<E> extends Collection<E> {
```

所以是成立的。这样的话就是一个真正的list了，那么就具有了arraylist所有的特性了，所以操作就不会报错了。

# Sql优化操作：

## 左连接(left join on / left outer join on)

left join 是left outer join的简写，它的全称是左外连接，是外连接中的一种。

左(外)连接，左表(a_table)的记录将会全部表示出来，而右表(b_table)只会显示符合搜索条件的记录。右表记录不足的地方均为NULL

```sql
SELECT
	agencies.*,
FROM
	authentication_user au
LEFT JOIN agencies_agencyuser ag ON ag.UserId = au.Id
LEFT JOIN agencies_agency agencies ON agencies.Id = ag.AgencyId
WHERE
	au.Email = 'kay@plg-test.com'
//结果：

2019-08-06 11:22:26.497	1BCE1865-970C-4E60-9CE9-63D2D6941F01	0	北大西洋教育	0	9B075BFC-4475-45A5-B6B3-878C3BBED701	2019-08-06 11:22:26.497		
```



## 右连接(right join/right outer join)

right join是right outer join的简写，它的全称是右外连接，是外连接中的一种。

与左(外)连接相反，右(外)连接，左表(a_table)只会显示符合搜索条件的记录，而右表(b_table)的记录将会全部表示出来。左表记录不足的地方均为NULL

```sql
SELECT
	agencies.*
FROM
	authentication_user au
RIGHT JOIN agencies_agencyuser ag ON ag.UserId = au.Id
right JOIN agencies_agency agencies ON agencies.Id = ag.AgencyId
WHERE
	au.Email = 'kay@plg-test.com'
//结果：

2019-08-06 11:22:26.497	1BCE1865-970C-4E60-9CE9-63D2D6941F01	0	北大西洋教育	0	9B075BFC-4475-45A5-B6B3-878C3BBED701	2019-08-06 11:22:26.497	
```



## 内连接(inner join on)

说明：组合两个表中的记录，返回关联字段相符的记录，也就是返回两个表的交集（阴影）部分。

select * from a_table a inner join b_table bon a.a_id = b.b_id;

- JOIN: 如果表中有至少一个匹配，则返回行
- LEFT JOIN: 即使右表中没有匹配，也从左表返回所有的行
- RIGHT JOIN: 即使左表中没有匹配，也从右表返回所有的行
- FULL JOIN: 只要其中一个表中存在匹配，就返回行

```sql
//右连接
SELECT column_name(s)
FROM table_name1
RIGHT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
//左连接
SELECT column_name(s)
FROM table_name1
LEFT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
// 内连接
SELECT column_name(s)
FROM table_name1
INNER JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
//SELECT INTO 语句从一个表中选取数据，然后把数据插入另一个表中。
//SELECT INTO 语句常用于创建表的备份复件或者用于对记录进行存档。
SELECT Persons.LastName,Orders.OrderNo
INTO Persons_Order_Backup
FROM Persons
INNER JOIN Orders
ON Persons.Id_P=Orders.Id_P

```

### SQL SELECT DISTINCT 语句

在表中，可能会包含重复值。这并不成问题，不过，有时您也许希望仅仅列出不同（distinct）的值。

关键词 DISTINCT 用于返回唯一不同的值。

```
SELECT DISTINCT 列名称 FROM 表名称
```



## 优化

优化思路：

- •去掉不必要的关联
- •规避or：使用notexists（notexists的执行逻辑，什么情况下适用notexists）
- •规避distinct：使用exists（exists的执行逻辑是什么？什么情况下适合使用exists）
- •必要的索引，根据需要建立组合索引jia

•优化后的确认

- •结果一致性确认
- •查看解释计划
- •执行脚本，比较修改前后脚本第二次执行的时间
- •改写后执行时间有可能变长，原因：
- •跟哪些字段建立索引有关系
- •跟组合索引的字段顺序有关系
- •跟数据分布有关系，一般来讲，如果过滤条件的筛选小于30%，是不走索引的

### 使用绑定变量形式不使用拼接sql

```java
绑定变量形式：
PreparedStatement pstmt = con.prepareStatement("UPDATE employees SET salay = ? WHERE id = ?"); pstmt.setBigDecimal(1, 15.00);
pstmt.setInt(2, 110592); 
//result statmement:   UPDATE employees SET salay = 15.00 WHERE id = 110592 pstmt.executeQuery();
//其实使用 mybatis 我们在不知不觉中就使用量绑定变量的方式执行 sql。这涉及 mybatis 中 ‘#’ 和 ‘$ ‘的区别。mybatis 在对 sql 语句进行预编译之前，会对 sql 进行动态解析，解析为一个 BoundSql 对象，也是在此处对动态 SQL 进行处理的。在动态 SQL 解析阶段， #{ } 和 ${ } 会有不同的表现。比如同一条 sql
select * from dual where t = #{test}
// #{} 在动态解析的时候， 会解析成一个参数标记符。就是解析之后的语句是：
select * from dual where t = ?
//这就和 jdbc 绑定变量的方式一样了。而
select * from dual where t = ${test}
//${}在动态解析的时候，会将我们传入的参数当做String字符串填充到我们的语句中，就会变成下面的语句
select * from dual where t = '参数'
//预编译之前的 SQL 语句已经不包含变量了，完全已经是常量数据了。相当于我们普通没有变量的sql了。
//综上所得， ${ } 变量的替换阶段是在动态 SQL 解析阶段，而 #{ }变量的替换是在 DBMS 中。

```

**什么时候不用绑定变量：**

- a.如果你用数据仓库，一条大查询一跑几个小时，根本没必要做绑定变量，因为解析的消耗微乎其微。
- b. 变量对优化器产生执行计划有很重要的影响的时候：绑定变量被使用时，查询优化器会忽略其具体值，因此其预估的准确性远不如使用字面量值真实，尤其是在表存在数据倾斜(表上的数据非均匀分布)的列上会提供错误的执行计划。从而使得非高效的执行计划被使用。

### 使用join，in,还是exists

- in的使用场景：结果集很小，最好小于百条内

- join：多对一或者一对一时，可以使用

- exists：在表数据量较大，选择性较低，关联关系属于一对多时适合用该语法

  在使用时，select sname,from student where exists (select * from sc where sno=student.sno and cno='1'),这里使用了exists字句，若内层的查询结果为空，则外层的where字句返回真值，否则为假，这里引出子查询用了*因为exists的子查询只返回真与假给出列无实际意义。

###union与union all的两点区别是什么

UNION 操作符用于合并两个或多个 SELECT 语句的结果集。

请注意，UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。

默认地，UNION 操作符选取不同的值。如果允许重复的值，请使用 UNION ALL。

UNION ALL 命令和 UNION 命令几乎是等效的，不过 UNION ALL 命令会列出所有的值,并且允许重复

```sql
SELECT
	c.Id CenterId,
	c.Name CenterName,
	g.Id GroupId,
	g.Name GroupName,
	g.DomainId,
	d.Name DomainName,
	p.Alias GroupAlias,
	p.ToAtLocal GroupAliasToAtLocal,
	p.FromAtLocal GroupAliasFromAtLocal,
	e.Id EnrollmentId,
	e.FirstName,
	e.LastName,
	ep.Alias EnrollmentAlias,
	ep.FromAtLocal EnrollmentAliasFromAtLocal,
	ep.ToAtLocal EnrollmentAliasToAtLocal,
	c.CenterTimeZone,
	g.IsDeleted GroupIsDeleted,
	e.IsDeleted EnrollmentIsDeleted,
	ep.isActived ChildPeriodIsActived,
	g.IsInactive GroupIsInactive
FROM
	contents_center c
INNER JOIN agencies_agencycenter ac ON c.Id = ac.CenterId
LEFT JOIN contents_group g ON c.Id = g.CenterId
LEFT JOIN contents_enrollment e ON g.Id = e.GroupId
LEFT JOIN groups_period p ON g.PeriodId = p.Id
LEFT JOIN enrollment_period ep ON e.Id = ep.EnrollmentId
LEFT JOIN domains_domain d ON g.DomainId = d.Id
WHERE
	ac.AgencyId = '1BCE1865-970C-4E60-9CE9-63D2D6941F01'
AND (
	c.IsDeleted = 0
	OR c.IsDeleted IS NULL
)
AND (
	ep.isActived = 1
	OR ep.isActived IS NULL
)
ORDER BY
	CenterName,
	GroupName,
	FirstName,
	LastName,
	p.FromAtLocal DESC;
```



## IDEA 使用

在切换不同的分支代码后必须先把 lg-common进行 clean---》install 然后在把lg-api-service 进行 clean ---> install





# Spring类解读

### Interceptor类

SpringMVC 中的Interceptor 拦截请求是通过HandlerInterceptor 来实现的。在SpringMVC 中定义一个Interceptor 非常简单，主要有两种方式，第一种方式是要定义的Interceptor类要实现了Spring 的HandlerInterceptor 接口，或者是这个类继承实现了HandlerInterceptor 接口的类，比如Spring 已经提供的实现了HandlerInterceptor 接口的抽象类HandlerInterceptorAdapter ；第二种方式是实现Spring的WebRequestInterceptor接口，或者是继承实现了WebRequestInterceptor的类。

   （1 ）preHandle (HttpServletRequest request, HttpServletResponse response, Object handle) 方法，顾名思义，该方法将在请求处理之前进行调用。SpringMVC 中的Interceptor 是链式的调用的，在一个应用中或者说是在一个请求中可以同时存在多个Interceptor 。每个Interceptor 的调用会依据它的声明顺序依次执行，而且最先执行的都是Interceptor 中的preHandle 方法，所以可以在这个方法中进行一些前置初始化操作或者是对当前请求的一个预处理，也可以在这个方法中进行一些判断来决定请求是否要继续进行下去。该方法的返回值是布尔值Boolean类型的，当它返回为false 时，表示请求结束，后续的Interceptor 和Controller 都不会再执行；当返回值为true 时就会继续调用下一个Interceptor 的preHandle 方法，如果已经是最后一个Interceptor 的时候就会是调用当前请求的Controller 方法。

   （2 ）postHandle (HttpServletRequest request, HttpServletResponse response, Object handle, ModelAndView modelAndView) 方法，由preHandle 方法的解释我们知道这个方法包括后面要说到的afterCompletion 方法都只能是在当前所属的Interceptor 的preHandle 方法的返回值为true 时才能被调用。postHandle 方法，顾名思义就是在当前请求进行处理之后，也就是Controller 方法调用之后执行，但是它会在DispatcherServlet 进行视图返回渲染之前被调用，所以我们可以在这个方法中对Controller 处理之后的ModelAndView 对象进行操作。postHandle 方法被调用的方向跟preHandle 是相反的，也就是说先声明的Interceptor 的postHandle 方法反而会后执行，这和Struts2 里面的Interceptor 的执行过程有点类型。Struts2 里面的Interceptor 的执行过程也是链式的，只是在Struts2 里面需要手动调用ActionInvocation 的invoke 方法来触发对下一个Interceptor 或者是Action 的调用，然后每一个Interceptor 中在invoke 方法调用之前的内容都是按照声明顺序执行的，而invoke 方法之后的内容就是反向的。

   （3 ）afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handle, Exception ex) 方法，该方法也是需要当前对应的Interceptor 的preHandle 方法的返回值为true 时才会执行。顾名思义，该方法将在整个请求结束之后，也就是在DispatcherServlet 渲染了对应的视图之后执行。这个方法的主要作用是用于进行资源清理工作的。

拦截器是一个Spring的组件，归Spring管理，配置在Spring文件中，因此能使用Spring里的任何资源、对象，例如 Service对象、数据源、事务管理等，通过IoC注入到拦截器即可 而 Filter 是被 Server(like Tomcat) 调用 Filter 不能够使用 Spring 容器资源，Filter 是在 Servlet 规范中定义的，是 Servlet 容器支持的。Filter 定义在 web.xml 中，Filter在只在 Servlet 前后起作用。Filters 通常将 请求和响应（request/response） 当做黑盒子，Filter 通常不考虑servlet 的实现。

#### Interceptor使用

interceptor 的执行顺序大致为：

1. 请求到达 DispatcherServlet
2. DispatcherServlet 发送至 Interceptor ，执行 preHandle
3. 请求达到 Controller
4. 请求结束后，postHandle 执行

Spring 中主要通过 HandlerInterceptor 接口来实现请求的拦截，实现 HandlerInterceptor 接口需要实现下面三个方法：

- preHandle() – 在handler执行之前，返回 boolean 值，true 表示继续执行，false 为停止执行并返回。

- postHandle() – 在handler执行之后, 可以在返回之前对返回的结果进行修改

- afterCompletion() – 在请求完全结束后调用，可以用来统计请求耗时等等

  

```java
//登陆拦截器

public class LoginInterceptor implements HandlerInterceptor {

/**
 * 在控制器执行之前完成业务逻辑操作
 * 方法的返回值决定逻辑是否继续执行， true，表示继续执行， false, 表示不再继续执行。
 */
public boolean preHandle(HttpServletRequest request, 
				HttpServletResponse response, Object handler) throws Exception {
	
	// 判断当前用户是否已经登陆
	HttpSession session = request.getSession();
	User loginUser = (User)session.getAttribute("loginUser");
	
	if ( loginUser == null ) {
		String path = session.getServletContext().getContextPath();
		response.sendRedirect(path + "/login");
		return false;	
	} else {
		return true;
	}
}

/**
 * 在Controller方法调用之后执行，但是它会在DispatcherServlet进行视图返回渲染之前被调用
 */
public void postHandle(HttpServletRequest request, 
				HttpServletResponse response, Object handler,
		ModelAndView modelAndView) throws Exception {
	// TODO Auto-generated method stub
}

/**
 * 在完成视图渲染之后，执行此方法。
 */
public void afterCompletion(HttpServletRequest request, 
				HttpServletResponse response, Object handler, Exception ex)
		throws Exception {
}
}
```

日志拦截器

```java
/**
* 用于记录日志，在每次请求之前，打印请求的地址和参数，方便调试
 */

public class LogInterceptor implements HandlerInterceptor {

	/** 记录日志 */
	private static Logger logger = LogManager.getLogger(LogManager.ROOT_LOGGER_NAME);
	
	@Override
	public boolean preHandle(HttpServletRequest request，HttpServletResponse response, 
								Object arg2) throws Exception {
		StringBuilder sb = new StringBuilder();
		String uri = request.getRequestURI();
		sb.append("---------------> demo uri:").append(uri).append(" - ");

		Enumeration<String> enums2 = request.getParameterNames();
		while (enums2.hasMoreElements()) {
			String key = enums2.nextElement();
			sb.append("\"").append(key).append("\":").append(
					request.getParameter(key)).append(", ");
		}
		logger.info(sb.toString());
		return true;
	}	

	@Override
	public void postHandle(HttpServletRequest request，HttpServletResponse response, 
								Object arg2, ModelAndView arg3)throws Exception {
	
	}

	@Override
	public void afterCompletion(HttpServletRequest request，HttpServletResponse response, 
								Object arg2, Exception arg3) throws Exception {
	
	}
}
```

授权访问Url拦截器

```java
	//此处采用继承抽象类的方式，不是实现Interceptor方式
public class AuthInterceptor extends HandlerInterceptorAdapter {
	@Autowired
	private PermissionService permissionService;
	
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
										Object handler) throws Exception {
		// 获取用户的请求地址
		String uri = request.getRequestURI();
		String path = request.getSession().getServletContext().getContextPath();
		
		// 判断当前路径是否需要进行权限验证。
		// 查询所有需要验证的路径集合
		List<Permission> permissions = permissionService.queryAll();
		Set<String> uriSet = new HashSet<String>();
		for ( Permission permission : permissions ) {
			if ( permission.getUrl() != null && !"".equals(permission.getUrl()) ) {
				uriSet.add(path + permission.getUrl());
			}
		}
		
		if ( uriSet.contains(uri) ) {
			// 权限验证
			// 判断当前用户是否拥有对应的权限
			Set<String> authUriSet = (Set<String>)request.getSession()
														.getAttribute("authUriSet");
			if ( authUriSet.contains(uri) ) {
				return true;
			} else {
				response.sendRedirect(path + "/error");
				return false;
			}
		} else {
			return true;
		}
	}
}
```

##### 在spring中注册使用

需要把上面的这些拦截器给注册到spring中使用,在springMVC.xml中配置如下：

```
<mvc:interceptors>
<mvc:interceptor>
    <mvc:mapping path="/**" />
    <bean class="site.gaoyisheng.filter.LoginHandlerInterceptor"></bean>
</mvc:interceptor>
</mvc:interceptor>
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**" />
        <mvc:exclude-mapping path="/login" />
        <mvc:exclude-mapping path="/bootstrap/**" />
        <mvc:exclude-mapping path="/css/**" />
        <mvc:exclude-mapping path="/fonts/**" />
        <mvc:exclude-mapping path="/img/**" />
        <mvc:exclude-mapping path="/jquery/**" />
        <mvc:exclude-mapping path="/layer/**" />
        <mvc:exclude-mapping path="/script/**" />
        <mvc:exclude-mapping path="/ztree/**" />
        //该拦截器不对静态资源进行拦截
        <bean class="com.scorpios.atcrowdfunding.web.LoginInterceptor"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

##### 在springboot中注册使用：

```java
/**
 * 扩展SpringMVC
 * SpringBoot2使用的Spring5，因此将WebMvcConfigurerAdapter改为WebMvcConfigurer
 * 使用WebMvcConfigurer扩展SpringMVC好处既保留了SpringBoot的自动配置，又能用到我们自己的配置
 */
 //@EnableWebMvc //如果我们需要全面接管SpringBoot中的SpringMVC配置则开启此注解，
                 //开启后，SpringMVC的自动配置将会失效。
 @Configuration
 public class WebConfig implements WebMvcConfigurer {
     @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        //设置对“/”的请求映射到index
         //如果没有数据返回到页面，没有必要用控制器方法对请求进行映射
        registry.addViewController("/").setViewName("index");
    }
   /**
     * 跨域支持
     * @param registry
     */
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowCredentials(true)
                .allowedMethods("GET", "POST", "DELETE", "PUT")
                .maxAge(3600 * 24);
    }

  @Override
   public void addResourceHandlers(ResourceHandlerRegistry registry) {
         // 默认访问"/"到"index.html"
         registry.addResourceHandler("/index.html")
                .addResourceLocations("/");
          // 资源处理 添加静态资源处理器 ,自定义静态资源映射目录
          registry.addResourceHandler("/**")
                  .addResourceLocations("classpath:/static/");
           // 配置映射资源，访问这个路径直接映射到文件 拦截器
    	registry.addResourceHandler("/image/**").addResourceLocations("file:E:/upload/image/");
      }
     //注册拦截器
     @Override
     public void addInterceptors(InterceptorRegistry registry) {
        //SpringMVC下，拦截器的注册需要排除对静态资源的拦截(*.css,*.js)
        //SpringBoot已经做好了静态资源的映射
         registry.addInterceptor(new LoginInterceptor()).addPathPatterns("/**");
     }
 }
```

### WebMvcConfigurer接口（spring 4.0时)

定义回调方法以自定义通过启用的Spring MVC的基于Java的配置

`@EnableWebMvc`-带注释的配置类可以实现此接口以使其被回调，并有机会自定义默认配置

主要方法：

```
void addFormatters(FormatterRegistry var1);

void configureMessageConverters(List<HttpMessageConverter<?>> var1);

void extendMessageConverters(List<HttpMessageConverter<?>> var1);

Validator getValidator();

void configureContentNegotiation(ContentNegotiationConfigurer var1);

void configureAsyncSupport(AsyncSupportConfigurer var1);

void configurePathMatch(PathMatchConfigurer var1);

void addArgumentResolvers(List<HandlerMethodArgumentResolver> var1);

void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> var1);

void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> var1);

void addInterceptors(InterceptorRegistry var1);

MessageCodesResolver getMessageCodesResolver();

void addViewControllers(ViewControllerRegistry var1);

void configureViewResolvers(ViewResolverRegistry var1);

void addResourceHandlers(ResourceHandlerRegistry var1);

void configureDefaultServletHandling(DefaultServletHandlerConfigurer var1);
```



#### 实现类：WebMvcConfigurerAdapter（5.0之后被弃用了，不推荐使用但是还可以用）

其实这个WebMvcConfigurerAdepter只是WebMvcConfigurer接口的一个实现类。是一个抽象类，里面是和父接口一样的方法。所以在spring5.0之后去除了WebMvcConfigurerAdapter这个类。当我们使用时直接实现WebMvcConfigurer接口即可，然后实现其中的方法。

Defines callback methods to customize the Java-based configuration for Spring MVC enabled via `@EnableWebMvc`.从5.0开始，WebMvcConfigurer具有默认方法（由Java 8基准实现），可以直接实现而无需此适配器

#### 实现类 WebMvcConfigurerComposite (5.0后弃用)

主要方法如下：

```java
 // 
   public void addWebMvcConfigurers(List<WebMvcConfigurer> configurers) {
          if (configurers != null) {
              this.delegates.addAll(configurers);
          }

      }
	// 注册自定义转化器  注册自定义的Formatter和Convert
    public void addFormatters(FormatterRegistry registry) {
        Iterator var2 = this.delegates.iterator();

        while(var2.hasNext()) {
            WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
            delegate.addFormatters(registry);
        }

    }
    //// 拦截器配置，添加springmvc拦截器
    public void addInterceptors(InterceptorRegistry registry) {
        Iterator var2 = this.delegates.iterator();

        while(var2.hasNext()) {
            WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
            delegate.addInterceptors(registry);
        }

    }
	//// 添加自定义视图控制器 ,视图跳转控制 而无需书写controller
    public void addViewControllers(ViewControllerRegistry registry) {
        Iterator var2 = this.delegates.iterator();

        while(var2.hasNext()) {
            WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
            delegate.addViewControllers(registry);
        }

    }
	// 该方法的参数ViewResolverRegistry 是一个注册器，用来注册你想自定义的视图解析器等。
    public void configureViewResolvers(ViewResolverRegistry registry) {
        Iterator var2 = this.delegates.iterator();

        while(var2.hasNext()) {
            WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
            delegate.configureViewResolvers(registry);
        }

    }
	// // 资源处理 添加静态资源处理器 ,自定义静态资源映射目录
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        Iterator var2 = this.delegates.iterator();

        while(var2.hasNext()) {
            WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
            delegate.addResourceHandlers(registry);
        }

    }

```

#### 使用WebMvcConfigurer：

```java
/**
 * Spring 5 (或者Spring Boot 2.x)版本配置Web应用程序示例
 */
@Configuration
public class MvcConfigure implements WebMvcConfigurer{
 	@Override
    public void addFormatters(FormatterRegistry formatterRegistry) {
        // 注册自定义转化器  注册自定义的Formatter和Convert
    }

    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> list) {
		// 配置消息转换器。重载会覆盖默认注册的HttpMessageConverter
    }

    @Override
    public void extendMessageConverters(List<HttpMessageConverter<?>> list) {
		// 配置消息转换器。仅添加一个自定义的HttpMessageConverter
    }

    @Override
    public Validator getValidator() {
    	// 
        return null;
    }

    @Override
    public void configureContentNegotiation(ContentNegotiationConfigurer contentNegotiationConfigurer) {
		//  内容协商配置
    }

    @Override
    public void configureAsyncSupport(AsyncSupportConfigurer asyncSupportConfigurer) {
		// 
    }

    @Override
    public void configurePathMatch(PathMatchConfigurer pathMatchConfigurer) {
    	// 配置路由请求规则
 		configurer.setUseSuffixPatternMatch(false);
    }

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> list) {
		// 添加自定义方法参数处理器
    }

    @Override
    public void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> list) {
		// 添加自定义返回结果处理器
    }

    @Override
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> list) {
		// 配置异常转换器
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
		// 拦截器配置，添加springmvc拦截器
      // 如果未配置了静态资源映射可以把排除拦截中的路径写静态资源路径用.excludePathPatterns（"/static/**")排除静态资源路径，
		registry.addInterceptor(new MyInterceptor()).addPathPatterns("/**").excludePathPatterns("/js/**","/css/**","/images/**","/index.html","/","/user/login","/static/**");
		// addPathPatterns("/**")对所有请求都拦截，但是排除了/user/login和/login请求的拦截
    }

    @Override
    public MessageCodesResolver getMessageCodesResolver() {
    	// 
        return null;
    }
	/**
	 * 以前要访问一个页面需要先创建个Controller控制类，再写方法跳转到页面
     * 在这里配置后就不需要那么麻烦了，直接访问http://localhost:8080/toLogin就跳转到login.jsp页面了
     **/
    @Override
    public void addViewControllers(ViewControllerRegistry viewControllerRegistry) {
		// 添加自定义视图控制器 ,视图跳转控制 而无需书写controller
		viewControllerRegistry.addViewController("/toLogin").setViewName("login")
    }

    @Override
    public void configureViewResolvers(ViewResolverRegistry viewResolverRegistry) {
		// 从方法名称我们就能看出这个方法是用来配置视图解析器的，该方法的参数ViewResolverRegistry 是一个注册器，用来注册你想自定义的视图解析器等。
        	
    }

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
    	// 资源处理 添加静态资源处理器 ,自定义静态资源映射目录
      	// 自定义后在注册拦截器时不需要使用 排除路径的方法
		registry.addResourceHandler("/**")
                .addResourceLocations("classpath:/static/")
                .addResourceLocations("classpath:/public/")
                .addResourceLocations("classpath:/resources/");
         // 配置映射资源，访问这个路径直接映射到文件 拦截器
         registry.addResourceHandler("/image/**").addResourceLocations("file:E:/upload/image/");
    }

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer defaultServletHandlerConfigurer) {
		// 默认静态资源处理器 默认seevlet处理
    }
  
}
```

#### 使用 WebMvcConfigurationSupport

**WebMvcConfigurationSupport**是一个提供了以Java编程方式来配置Web应用程序的配置主类，

```
public class WebMvcConfigurationSupport implements ApplicationContextAware,ServletContextAware{}

```

```
1、springboot默认可以访问以下路径文件(见ResourceProperties)：
    classpath:/static
    classpath:/public
    classpath:/resources
    classpath:/META-INF/resources
   当使用了@EnableWebMvc时，默认的静态资源访问无效了因为默认情况下mvc使用的配置是WebMvcAutoConfiguration，加入该配置变成了WebMvcConfigurationSupport
2、@EnableWebMvc、WebMvcConfigurationSupport、WebMvcConfigurationAdapter
    @EnableWebMvc=WebMvcConfigurationSupport，使用了@EnableWebMvc注解等于扩展了WebMvcConfigurationSupport但是没有重写任何方法
    @EnableWebMvc+extends WebMvcConfigurationAdapter，在扩展的类中重写父类的方法即可，这种方式会屏蔽springboot的WebMvcAutoConfiguration中的设置
    @EnableWebMvc+extends WebMvcConfigurationSupport 只会使用@EnableWebMvc
    extends WebMvcConfigurationSupport，在扩展的类中重写父类的方法即可，这种方式会屏蔽springboot的@WebMvcAutoConfiguration中的设置
    extends WebMvcConfigurationAdapter，在扩展的类中重写父类的方法即可，这种方式依旧使用springboot的WebMvcAutoConfiguration中的设置
    在springboot2.x中，WebMvcConfigurationAdapter已经过时，通过实现接口WebMvcConfigurer可以替代原有规则
```

```java 
public class MyWebMvcConfiguer extends WebMvcConfigurationSupport {
    public MyWebMvcConfiguer() {
        super();
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) {
        super.setApplicationContext(applicationContext);
        //  设置ApplicationContext
    }

    @Override
    public void setServletContext(ServletContext servletContext) {
        super.setServletContext(servletContext);
        // 设置域
    }
	/**
     * 拦截器配置
     *
     * @param registry 注册类
     */
    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
    	registry.addInterceptor(new MyInterceptor()).addPathPatterns("/**").excludePathPatterns("/js/**","/css/**","/images/**","/index.html","/","/user/login","/static/**");
		// addPathPatterns("/**")对所有请求都拦截，但是排除了/user/login和/login请求的拦截
        super.addInterceptors(registry);
        
    }
    @Override
    public void configurePathMatch(PathMatchConfigurer configurer) {
        super.configurePathMatch(configurer);
        //配置路径匹配选项。
    }

    @Override
    protected void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
        super.configureContentNegotiation(configurer);
        // 配置内容协商
    }

    @Override
    protected void addViewControllers(ViewControllerRegistry registry) {
        super.addViewControllers(registry);
        // 添加视图控制器
    }

    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        super.addResourceHandlers(registry);
        // 添加用于处理静态资源的资源处理程序
    }

    @Override
    protected void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        super.configureDefaultServletHandling(configurer);
        // 配置“默认” Servlet处理
    }

    @Override
    protected void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
        super.addArgumentResolvers(argumentResolvers);
        // 除了默认注册的方法外，还添加要使用的方法
    }

    @Override
    protected void addReturnValueHandlers(List<HandlerMethodReturnValueHandler> returnValueHandlers) {
        super.addReturnValueHandlers(returnValueHandlers);
        // 除了默认注册的自定义内容外，还添加自定义内容
    }

    @Override
    protected void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        super.configureMessageConverters(converters);
        // 
    }

    @Override
    protected void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        super.extendMessageConverters(converters);
        // 以添加HttpMessageConverters 要与RequestMappingHandlerAdapter和 一起使用的自定义ExceptionHandlerExceptionResolver
    }

    @Override
    protected void addFormatters(FormatterRegistry registry) {
    	// 注册自定义转化器  注册自定义的Formatter和Convert
        super.addFormatters(registry);
    }

    @Override
    public void configureAsyncSupport(AsyncSupportConfigurer configurer) {
        super.configureAsyncSupport(configurer);
        // 配置异步请求处理选项
    }


    @Override
    protected void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> exceptionResolvers) {
        super.configureHandlerExceptionResolvers(exceptionResolvers);
        // 配置HandlerExceptionResolvers要使用的列表
    }

    @Override
    protected void configureViewResolvers(ViewResolverRegistry registry) {
        super.configureViewResolvers(registry);
        // 配置视图分辨率
    }
}
```

### WebApplicationInitializer 代替web.xml的配置

在Servlet 3.0+环境中实现的接口，以便以ServletContext`编程方式进行配置 -与传统的web.xml基于方法的方法相反（或可能与之结合）。会自动检测SPI的实现SpringServletContainerInitializer，它本身会被任何Servlet 3.0容器自动引导

大部分构建Web应用程序的Spring用户都需要注册Spring的DispatcherServlet。作为参考，通常在WEB-INF / web.xml中执行以下操作：

```xml
<servlet> 
   <servlet-name> dispatcher </ servlet-name> 
   <servlet-class> 
     org.springframework.web.servlet.DispatcherServlet 
   </ servlet-class> 
   <init-param> 
     <param-name> contextConfigLocation </ param-名称> 
     <param-value> /WEB-INF/spring/dispatcher-config.xml </ param-value> 
   </ init-param> 
   <load-on-startup> 1 </ load-on-startup> 
 </ servlet > 

 <servlet-mapping> 
   <servlet-name>dispatcher</ servlet-name> 
   <url-pattern> / </ url-pattern> 
 </ servlet-mapping>
```

```java
// java代码代替上面操作：
public class MyWebAppInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
      XmlWebApplicationContext appContext = new XmlWebApplicationContext();
      appContext.setConfigLocation("/WEB-INF/spring/dispatcher-config.xml");

      ServletRegistration.Dynamic dispatcher =
        container.addServlet("dispatcher", new DispatcherServlet(appContext));
      dispatcher.setLoadOnStartup(1);
      dispatcher.addMapping("/");
    }

 }
```

`WebApplicationInitializer`非常适合与Spring的基于代码的 `@Configuration`类一起使用但是以下示例演示了重构，以使用Spring [`AnnotationConfigWebApplicationContext`](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/context/support/AnnotationConfigWebApplicationContext.html)代替`XmlWebApplicationContext`和用户定义的`@Configuration`类`AppConfig`， `DispatcherConfig`而不是Spring XML文件。此示例还超出了上面的示例，以演示“ root”应用程序上下文的典型配置和的注册`ContextLoaderListener`：

```java
public class MyWebAppInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
      // Create the 'root' Spring application context
      // 请看 AbstractAnnotationConfigDispatcherServletInitializer
      AnnotationConfigWebApplicationContext rootContext =
        new AnnotationConfigWebApplicationContext();
      rootContext.register(AppConfig.class);

      // Manage the lifecycle of the root application context
      container.addListener(new ContextLoaderListener(rootContext));
      // 加载配置文件
      ResourcePropertySource props;
		try {
			props = new ResourcePropertySource("classpath:service.properties");
		} catch (IOException ex) {
			log.error("could not load properties file to use for environment configuration", ex);
			throw new ServletException(ex);
		}
	  // Create the dispatcher servlet's Spring application context
      AnnotationConfigWebApplicationContext dispatcherContext =
        new AnnotationConfigWebApplicationContext();
      dispatcherContext.register(DispatcherConfig.class)
        
      // Register and map the dispatcher servlet
      ServletRegistration.Dynamic dispatcher =
        container.addServlet("dispatcher", new DispatcherServlet(rootContext));
      dispatcher.setLoadOnStartup(1);
      dispatcher.addMapping("/");
    }

 }
```

系统`WebApplicationInitializer`会*自动检测到实现* -因此您可以随意将其打包到应用程序中

```
AnnotationConfigWebApplicationContext rootContext =new AnnotationConfigWebApplicationContext();
```

**这里为何这样使用因为在AbstractAnnotationConfigDispatcherServletInitializer类中有这样的使用**：

```java
protected WebApplicationContext createRootApplicationContext() {
    Class<?>[] configClasses = this.getRootConfigClasses();
    if (!ObjectUtils.isEmpty(configClasses)) {
        AnnotationConfigWebApplicationContext rootAppContext = new AnnotationConfigWebApplicationContext();
        rootAppContext.register(configClasses);
        return rootAppContext;
    } else {
        return null;
    }
}

protected WebApplicationContext createServletApplicationContext() {
    AnnotationConfigWebApplicationContext servletAppContext = new AnnotationConfigWebApplicationContext();
    Class<?>[] configClasses = this.getServletConfigClasses();
    if (!ObjectUtils.isEmpty(configClasses)) {
        servletAppContext.register(configClasses);
    }

    return servletAppContext;
}

```

### Environment接口

#### API解读：

表示当前应用程序正在其中运行的环境的接口。 为应用程序环境的两个关键方面建模：配置文件和属性。 与属性访问有关的方法通过PropertyResolver超级接口公开。

用来表示整个应用运行时的环境，为了更形象地理解Environment，你可以把Spring应用的运行时简单地想象成两个部分：一个是Spring应用本身，一个是Spring应用所处的环境。
  Environment在容器中是一个抽象的集合，是指应用环境的2个方面：profiles和properties。

```java
package org.springframework.core.env;

public interface Environment extends PropertyResolver {
    String[] getActiveProfiles();

    String[] getDefaultProfiles();

    /** @deprecated */
    @Deprecated
    boolean acceptsProfiles(String... var1);

    boolean acceptsProfiles(Profiles var1);
}

```

#### 1. Profile

  profile配置是一个被命名的、bean定义的逻辑组，这些bean只有在给定的profile配置激活时才会注册到容器。不管是XML还是注解，Beans都有可能指派给profile配置。Environment环境对象的作用，对于profiles配置来说，它能决定当前激活的是哪个profile配置，和哪个profile是默认。

- 一个profile就是一组Bean定义的逻辑分组。
- 这个分组，也就 这个profile，被赋予一个命名，就是这个profile名字。
- 只有当一个profile处于active状态时，它对应的逻辑上组织在一起的这些Bean定义才会被注册到容器中。
- Bean添加到profile可以通过XML定义方式或才annotation注解方式。
- Environment对于profile所扮演的角色是用来指定哪些profile是当前活跃的缺省。

#### 2. Properties

  properties属性可能来源于properties文件、JVM properties、system环境变量、JNDI、servlet context parameters上下文参数、专门的properties对象，Maps等等。Environment对象的作用，对于properties来说，是提供给用户方便的服务接口、方便撰写配置、方便解析配置。

- 配置属性源。
- 从属性源中获取属性。
  容器（ApplicationContext）所管理的bean如果想直接使用Environment对象访问profile状态或者获取属性，可以有两种方式
  （1）实现EnvironmentAware接口。
  （2）@Inject或者@Autowired一个Environment对象。
  绝大数情况下，bean都不需要直接访问Environment对象，而是通过类似@Value注解的方式把属性值注入进来。

#### 使用：

**application.properties**

```properties
#DataSource
ds.driver.classname=com.microsoft.sqlserver.jdbc.SQLServerDriver
ds.url=
ds.username=
ds.password=
```

**加载配置的类：**

```java
@Configuration
@EnableWebMvc
@PropertySource("classpath:service.properties")
@EnableTransactionManagement
public class MVCConfig extends WebMvcConfigurerAdapter {
    @Autowired
    Environment env;
    @Bean
    public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer() {
        return new PropertySourcesPlaceholderConfigurer();
    }
}

```

在webAppinitializer注册：

```java
public class WebAppInitializer implements WebApplicationInitializer {
	private static final Logger log = LoggerFactory.getLogger(WebAppInitializer.class);

	@Override
	public void onStartup(ServletContext container) throws ServletException {
		log.info("starting web application initialization");

		// Create the 'root' Spring application context
		AnnotationConfigWebApplicationContext rootContext = new AnnotationConfigWebApplicationContext();
		rootContext.register(MVCConfig.class);

		// Manage the lifecycle of the root application context
		container.addListener(new ContextLoaderListener(rootContext));
		ResourcePropertySource props;
		try {
			props = new ResourcePropertySource("classpath:service.properties");
		} catch (IOException ex) {
			log.error("could not load properties file to use for environment configuration", ex);
			throw new ServletException(ex);
		}
		log.info("web application initialization properties:" + props.getSource());

		// Register and map the dispatcher servlet
		ServletRegistration.Dynamic dispatcher = container.addServlet(
				"dispatcher", new DispatcherServlet(rootContext));
		dispatcher.setLoadOnStartup(1);
		dispatcher.addMapping("/");

        // Register filter to allow cross-domain requests
        //http://stackoverflow.com/questions/21864147/access-control-allow-origin-error-mvc-spring-zepto-post
        FilterRegistration.Dynamic filterRegistration=container.addFilter("filterRegistration",new SimpleCORSFilter());
        filterRegistration.setAsyncSupported(true);
        filterRegistration.addMappingForUrlPatterns(EnumSet.allOf(DispatcherType.class), false, "/*");

    }
}

```



```java

package com.kayleoi.spring.data.config;

import com.jolbox.bonecp.BoneCPDataSource;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.env.Environment;

import javax.sql.DataSource;

/**
 * 配置DATASOURCE
 这里需要配置加载@PropertySource获取resource下面的资源
 */
@Configuration
@PropertySource("classpath:application.properties")
public class DatasourceConfig {
    /***
     * Spring应用的运行时简单地想象成两个部分：一个是Spring应用本身，一个是Spring应用所处的环境。
     * Environment在容器中是一个抽象的集合，是指应用环境的2个方面：profiles和properties 就像源碼中書寫的那樣
     * String[] getActiveProfiles(); 激活的
     * String[] getDefaultProfiles(); 默认的
     */
    @Autowired
    Environment env;

    @Bean(name = "sqlServerDataSource")
    public DataSource sqlServerDataSource() {
        // BoneCP是一个快速，开源的数据库连接池。帮你管理数据连接让你的应用程序能更快速地访问数据库。
        //比C3P0/DBCP连接池快25倍。
        BoneCPDataSource ds = new BoneCPDataSource();
        ds.setDriverClass(env.getProperty("ds.driver.classname"));
        ds.setJdbcUrl(env.getProperty("ds.url"));
        ds.setUsername(env.getProperty("ds.username"));
        ds.setPassword(env.getProperty("ds.password"));
        ds.setIdleConnectionTestPeriodInMinutes(240);
        ds.setIdleMaxAgeInMinutes(60);
        ds.setMaxConnectionsPerPartition(40);
        ds.setMinConnectionsPerPartition(5);
        ds.setPartitionCount(3);
        ds.setAcquireIncrement(10);
        ds.setStatementsCacheSize(100);
        return ds;
    }
}
```

### JDBCTemplate

`org.springframework.jdbc.core.JdbcTemplate`类是JDBC核心包中的中心类。它简化了JDBC的使用，并有助于避免常见的错误。 它执行核心JDBC工作流，留下应用程序代码来提供SQL并提取结果。 该类执行SQL查询或更新，在`ResultSet`类上启动迭代并捕获JDBC异常，并将它们转换为`org.springframework.dao`包中定义的通用更详细的异常层次结构。

#### 插入

```
String insertQuery = "insert into student (name, age) values (?, ?)";
jdbcTemplate.update( insertQuery, name, age)

```

#### 查询

```java
public List<Student> listStudents() {
        String SQL = "select * from Student";
        List<Student> students = jdbcTemplateObject.query(SQL, new StudentMapper());
        return students;
    }
    
public class StudentMapper implements RowMapper<Student> {
    public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
        Student student = new Student();
        student.setId(rs.getInt("id"));
        student.setName(rs.getString("name"));
        student.setAge(rs.getInt("age"));
        return student;
    }
}
```

##### 带参数查询

```java
public Student getStudent(Integer id) {
        String SQL = "select * from Student where id = ?";
        Student student = jdbcTemplateObject.queryForObject(SQL, new Object[] { id }, new StudentMapper());
        return student;
    }
public class StudentMapper implements RowMapper<Student> {
    public Student mapRow(ResultSet rs, int rowNum) throws SQLException {
        Student student = new Student();
        student.setId(rs.getInt("id")); // 列名
        student.setName(rs.getString("name"));
        student.setAge(rs.getInt("age"));
        return student;
    }
}
```

#### 更新

```java
String updateQuery = "update Student set age = ? where id = ?";
jdbcTemplateObject.update(updateQuery, age, id);


```

##### 批量更新

```java
String SQL = "update Student set age = ? where id = ?";
int[] updateCounts = jdbcTemplateObject.batchUpdate(SQL,
   new BatchPreparedStatementSetter() {
      public void setValues(PreparedStatement ps, int i) throws SQLException {
         ps.setInt(1, students.get(i).getAge());                        
         ps.setInt(2, students.get(i).getId());    
      }
      public int getBatchSize() {
         return students.size();
      }
   });
```



#### 删除

```java
String deleteQuery = "delete from Student where id = ?";
jdbcTemplateObject.update(deleteQuery, id);

```

#### RowMapper

JdbcTemplate类使用org.springframework.jdbc.core.RowMapper <T>接口在每行的基础上映射ResultSet的行。该接口的实现执行将每行映射到结果对象的实际工作。如果抛出SQLExceptions将被调用的JdbcTemplate捕获和处理。
接口的声明以下是org.springframework.jdbc.core.RowMapper<T>接口的声明 -
public interface RowMapper<T>
用法

- 步骤1 - 使用配置的数据源创建一个JdbcTemplate对象。
- 步骤2 - 创建一个实现RowMapper接口的StudentMapper对象。
- 步骤3 - 使用JdbcTemplate对象方法在使用StudentMapper对象时进行数据库操作





```java

public void insertMediaAll(MediaEntity media){
        jdbcTemplate.update(MediaMapper.insertMediaAll, media.getId(), media.getRelativePath(), media.getCreateAtUtc(),media.getSnapshotPath()
                ,media.getMimeType(),media.getWidth(),media.getHeight(),media.getSize(),media.getFileType(),media.getFileName(),media.getAnnexType(),media.getVoiceTime());
    }
//MediaMapper下的
    public static final String insertMediaAll = "insert into medias_media(Id,RelativePath,CreateAtUtc,SnapshotPath,MimeType,Width,Height,Size,FileType,FileName,AnnexType,VoiceTime) VALUES (?,?,?,?,?,?,?,?,?,?,?,?)";

// 结果集映射 查询的时候 query
 @Override
    public List<MediaResourceEntity> getMediaResourceByPlayIds(String playIds) {
        String sql = MediaMapper.SQL_SELECT_MEDIA_RESOURE_PLAYIDS.replace("@ids", playIds);
        return jdbcTemplate.query(sql, MediaMapper.MAPPER_MEDIAS_RESOURCE);
    }

//region mapper 映射的列
    public static final RowMapper<MediaResourceEntity> MAPPER_MEDIAS_RESOURCE = new RowMapper<MediaResourceEntity>() {
        @Override
        public MediaResourceEntity mapRow(ResultSet resultSet, int i) throws SQLException {
            MediaResourceEntity mediaResource = new MediaResourceEntity();
            mediaResource.setId(DBUtil.getString(resultSet, ID));
            mediaResource.setPlaylistId(DBUtil.getString(resultSet, PLAYLISTID));
            mediaResource.setCategory(DBUtil.getString(resultSet, CATEGORY));
            mediaResource.setLevel(DBUtil.getString(resultSet, LEVEL));
            mediaResource.setSource(DBUtil.getString(resultSet, SOURCE));
            mediaResource.setCreateAtUtc(DBUtil.getDate(resultSet, CREATE_AT_UTC));
            mediaResource.setDeleted(DBUtil.getBoolean(resultSet, IS_DELETED));
            mediaResource.setSortIndex(DBUtil.getInt(resultSet, SORTINDEX));
            mediaResource.setImg(DBUtil.getString(resultSet, IMG));
            mediaResource.setNeedPaid(DBUtil.getBoolean(resultSet, NEEDPAID));
            mediaResource.setPlayListName(DBUtil.getString(resultSet, PLAYLISTNAME));
            return mediaResource;
        }
    };
// queryforobject
 private static final String getSubTopicByNameSql = "select top 1 * from medias_subtopic where Name = ? and ParentId = ?";
    @Override
    public TopicEntity getSubTopicByNameAndParent(String name, String parentId) {
        try {
            return jdbcTemplate.queryForObject(getSubTopicByNameSql, new String[]{name, parentId}, new RowMapper<TopicEntity>() {
                @Override
                public TopicEntity mapRow(ResultSet rs, int rowNum) throws SQLException {
                    TopicEntity topic = new TopicEntity();
                    topic.setId(DBUtil.getString(rs, "Id"));
                    topic.setName(DBUtil.getString(rs, "Name"));
                    topic.setParentId(DBUtil.getString(rs, "ParentId"));
                    return topic;
                }
            });
        } catch (EmptyResultDataAccessException e) {
            return null;
        }
    }
```



