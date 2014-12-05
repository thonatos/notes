> * title: CyanogenMod SMSC
> * date: 2012-01-07 16:10:30

**CyanogenMod使用中很多人遇到GSM网络短信无法发送的问题，这个是因为SMSC设置有问题。**

**中文对照：**

SMS（短消息服务）是指定由ETSI（标准GSM 03.401和03.382）。它最多可包含160个字符，每个字符是根据7位GSM默认字母表书面。

短信还包含一些元数据，例如：

> *   有关发件人的信息（服务中心号码，发送号码）
> *   协议信息（协议标识符，数据编码方案）
> *   时间戳

有2种方式接收和发送短信消息的PDU（协议份比单 ​​位）和文本模式。在此页中，我们集中在PDU模式，这是什么的Android 2.3和更高的认识。旅行通过SMSC网关SMS消息发送和接收。一个急需配置SMSC的一个常见的症状是您的设备能够接收短信“，但不能发送（通常作为发送报告，但没有在接收端接收） 。

首先，你将需要运营商的SMSC网关号码。下面是美国主要运营商（运营商的谷歌）的数字：

> *   斯普林特：17044100000
> *   Verizon公司：316540951000
> *   AT＆T：13123149810
> *   的T - Mobile：12063130004

请注意数字包括“+”，为接下来的步骤的这个事项

转换为PDU格式
> 1.  拿你的，包括“+”号，并[去 http://www.twit88.com/home/utility/sms-pdu-encode-decode](http://www.twit88.com/home/utility/sms-pdu-encode-decode)
> 2.  接近页面的底部，有一个当场进入SMSC的，接收器和消息
>     1.  输入您的完整的短信服务中心号码（包括+）
>     2.  离开接收器和消息框空白
>     3.  选择字母7
>     4.  点击转换
> 3.  您将获得右侧在框中输出
> 4.  从第16位的第二行（上面列出的运营商例子）
> *   斯普林特：17044100000 = 07917140140000F0
> *   Verizon公司：316540951000 = 0791135604590100
> *   AT＆T公司：13123149810 = 0713121139418f0
> *   的T - Mobile：12063130004 = 07912160130300f4

您的SMSC号码现在是在PDU格式，现在来更新您的手机：

> 1.  打开拨号
> 2.  键入下列顺序'*#*# 4636 #*#*'
> 3.  开放手机信息
> 4.  向下滚动到SMSC
>     1.  可选：点击刷新查看当前使用的短信服务中心号码
> 5.  请输入您的PDU格式的短信服务中心号码
> 6.  新闻更新
一旦进入，可能需要长达10分钟的电话“握手”新网关。还建议重新启动电源周期的无线电。

假设一切正常，您现在应该能够正常发送和接收短信“。

注：MMS是在APN的控制Android的。如果彩信失败，请仔细检查您的APN 信息

**英文原文：**

SMS (Short Message Service) is specified by the ETSI (standards GSM 03.401 and 03.382 ). It can contain up to 160 characters, where each character is written according to the 7-bits GSM default alphabet.

SMS also contains some meta-data, e.g.

> *   Info about the senders ( Service center number, sender number)
> *   Protocol information (Protocol identifier, Data coding scheme)
> *   Timestamp

There are 2 ways to receive and send SMS messages a, PDU (protocol discription unit) and Text mode. In this page we focus on PDU mode, which is what Android 2.3 and higher recognize. The SMS messages travel through the SMSC gateway to send and receive. A common symptom of a badly configured SMSC is your device is able to receive SMS' but not send (usually reporting as sent, but without reception on the receiving end).

First, you will need your carrier's SMSC gateway number. Below are the numbers for major US carriers (Google for your carrier's):

> *   Sprint: +17044100000
> *   Verizon: +316540951000
> *   AT&amp;T: +13123149810
> *   T-Mobile: +12063130004

Notice the numbers include the +, this matters for the next steps

Converting to PDU format

> 1.  Take your number, including the + and go to [http://www.twit88.com/home/utility/sms-pdu-encode-decode](http://www.twit88.com/home/utility/sms-pdu-encode-decode)
> 2.  Towards the bottom of the page, there is a spot to enter SMSC, Receiver and Message
>     1.  Enter your full SMSC number (including the +)
>     2.  Leave Receiver and message box blank
>     3.  Select Alphabet 7
>     4.  Hit Convert
> 3.  You will get an output in the box on the right side
> 4.  Take the first 16 digits from the second line (examples for above carriers listed)
> *   Sprint: +17044100000 = 07917140140000F0
> *   Verizon: +316540951000 = 0791135604590100
> *   AT&amp;T: +13123149810 = 0713121139418f0
> *   T-Mobile: +12063130004 = 07912160130300f4
Your SMSC number is now in PDU format, now to update your phone:
> 1.  Open Dialer
> 2.  Type the following sequence '*#*#4636#*#*'
> 3.  Open Phone Information
> 4.  Scroll down to SMSC
>     1.  Optional: Hit Refresh to see current SMSC number used
> 5.  Enter in your PDU formatted SMSC number
> 6.  Press Update

Once entered, it can take up to 10 minutes for the phone to 'handshake' with the new gateway. A reboot is also suggested to power cycle the radio.

Assuming everything worked, you should now be able to send and receive SMS' properly.

Note: MMS is controlled by the APN in Android. If MMS is failing, double check your APN information