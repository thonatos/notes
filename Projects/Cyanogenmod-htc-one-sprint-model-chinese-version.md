> * title: "CyanogenMod HTC One (Sprint model) Chinese Version"
> * date: 2014-07-23 18:13:59


此前一直在使用 CyanogenMod 官方Rom，但由于美版的配置和中国大陆网络的不同，往往无法直接使用，会出现无法读取手机卡，无法上网等问题，当然，通过自己的修改是可以使他正常的，但是作为一个懒人，真的没那么好的心情一个个去修改文件，添加配置，尤其是经常更新的时候，一次次的修改真的太痛苦了，所以Copy了一份CyanogenMod Device Tree中的android_device_htc_m7spr并默认修改了system.prop与xml中的一些未汉化的部分，用于直接编译，这样生成的版本基本可以通过“首选网络”与“RUIM”选项之后便能够正常使用了。

由于硬件相同，CyanogenMod 建议不分离一个新的版本，故而作为非官方版本存在了github上，有兴趣的同学可以看看。

#### Github

* [https://github.com/thonatos/android_device_htc_m7sprcn](https://github.com/thonatos/android_device_htc_m7sprcn "android_device_htc_m7sprcn")
* [https://github.com/thonatos/android_device_htc_m7-common](https://github.com/thonatos/android_device_htc_m7-common "android_device_htc_m7-common")

#### Change
	
	### Changed Logs

	+2014.07.19

	ADD	:	packages/apps/GrimRock-Theme-White

	MOD	:	vendor/htc/m7spr/m7spr-vendor-blobs.mk
		# Custom Theme
		PRODUCT_PACKAGES += \
		    GrimRock-Theme-White

	+2014.07.18

	MOD	:	device/htc/m7spr/system.prop

		rild.libargs=-d /dev/smd0
		rild.libpath=/system/lib/libril-qc-qmi-1.so
		persist.radio.snapshot_enabled=1
		persist.radio.snapshot_timer=2
		ro.telephony.default_network=10
		telephony.lteOnCdmaDevice=1
		persist.radio.apm_sim_not_pwdn=1
		ro.cdma.home.operator.numeric=46003
		ro.cdma.home.operator.alpha=ChinaTelecom
		gsm.sim.operator.alpha=ChinaTelecom
		gsm.sim.operator.numeric=46003
		gsm.sim.operator.iso-country=cn
		gsm.operator.alpha=ChinaTelecom
		gsm.operator.numeric=46003
		gsm.operator.iso-country=cn
		persist.radio.use_nv_for_ehrpd=true
		persist.radio.mode_pref_nv10=1
		ril.subscription.types=NV,RUIM
		persist.radio.always_send_plmn=true

	ADD	:	device/htc/m7spr/overlay/packages/apps/Settings/res/values-zh-rCN/cm_strings.xml
		%% Clock_Style
			<string name="status_bar_clock_title">时钟样式</string>
			<string name="status_bar_clock_style_default">默认</string>
			<string name="status_bar_clock_style_center">居中</string>
			<string name="status_bar_clock_style_hidden">隐藏</string>
		%% Seting String
			<string name="mod_version">编译版本</string>
			<string name="cmupdate_settings_title">版本更新</string>


由于是个人使用，部分字符串我修改了，保留了CM的标志阿，只是有点“强迫症”，看着字符串一样长心里不舒服...

#### ScreenShot

在我的微博上可以看到编译版本的截图，[http://weibo.com/thonatos](http://weibo.com/thonatos) 。