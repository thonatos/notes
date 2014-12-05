> * title: 模拟器中运行编译好的Android
> * date: 2012-01-09 21:27:15

编译android4.0成功以后，肯定要测试的，下面就要想办法运行了，也许你跟我一样，既安装了sdk又下载了源码，然后输入emulator命令时候就会提示错误了。



首先你需要设置一下emulator工具的目录之类的，这个不细说了，

要在.bashrc中新增环境变量，如下

	ANDROID_PRODUCT_OUT=~/android/out/target/product/generic
	ANDROID_PRODUCT_OUT_BIN=~/android/out/host/linux-x86/bin
	
这里是设置你的输出文件的位置和bin工具目录，不用多解释吧？
然后在命令行输入：

	export PATH=${PATH}:${ANDROID_PRODUCT_OUT_BIN}:${ANDROID_PRODUCT_OUT};

上面是导入了相关的配置，然后使之生效。

	source ~/.bashrc

接着切换到输出的system文件夹

	cd ~/android/out/target/product/generic

然后来创建模拟器

	emulator -system system.img -data userdata.img -ramdisk ramdisk.img

如果你运气够好的话，也许现在已经在运行了，不过我运气明显不够好。

提示一：

	emulator: ERROR: You did not specify a virtual device name, and the system
	directory could not be found.
	If you are an Android SDK user, please use '@<name>' or '-avd <name>'
	to start a given virtual device (see -help-avd for details).
	Otherwise, follow the instructions in -help-disk-images to start the emulator

既然人家提示了，那就按照步骤走吧，输入命令：

	emulator -help-avd

接着提示如下：

	use '-avd <name>' to start the emulator program with a given Android
	Virtual Device (a.k.a. AVD), where <name> must correspond to the name
	of one of the existing AVDs available on your host machine.
	See -help-virtual-device to learn how to create/list/manage AVDs.
	As a special convenience, using '@<name>' is equivalent to using
	'-avd <name>'.
	
跟着提示继续走，输入命令：

	emulator -help-virtual-device

又是提示了：

	An Android Virtual Device (AVD) models a single virtual
	device running the Android platform that has, at least, its own
	kernel, system image and data partition.
	Only one emulator process can run a given AVD at a time, but
	you can create several AVDs and run them concurrently.
	You can invoke a given AVD at startup using either '-avd <name>'
	or '@<name>', both forms being equivalent. For example, to launch
	the AVD named 'foo', type:
	emulator @foo
	The 'android' helper tool can be used to manage virtual devices.
	For example:
	android create avd -n <name> -t 1 # creates a new virtual device.
	android list avd # list all virtual devices available.
	Try 'android --help' for more commands.
	Each AVD really corresponds to a content directory which stores
	persistent and writable disk images as well as configuration files.
	Each AVD must be created against an existing SDK platform or add-on.
	For more information on this topic, see -help-sdk-images.
	SPECIAL NOTE: in the case where you are *not* using the emulator
	with the Android SDK, but with the Android build system, you will
	need to define the ANDROID_PRODUCT_OUT variable in your environment.
	See -help-build-images for the details.
	
说实在的，这提示看着很郁闷，让你跳来跳去，就是不能解决问题。

直接创建avd吧，先看下说明：

	Usage:
	android [global options] create avd [action options]
	Global options:
	-h --help : Help on a specific command.
	-v --verbose : Verbose mode, shows errors, warnings and all messages.
	-s --silent : Silent mode, shows errors only.
	Action "create avd":
	Creates a new Android Virtual Device.
	Options:
	-c --sdcard : Path to a shared SD card image, or size of a new sdcard for
	the new AVD.
	-n --name : Name of the new AVD. [required]
	-a --snapshot: Place a snapshots file in the AVD, to enable persistence.
	-p --path : Directory where the new AVD will be created.
	-f --force : Forces creation (overwrites an existing AVD)
	-s --skin : Skin for the new AVD.
	-t --target : Target ID of the new AVD. [required]
	-b --abi : The ABI to use for the AVD. The default is to auto-select the
	ABI if the platform has only one ABI for its system images.

看明白了吧？

	android create avd -n test2 -f -p /home/thonatos/test -t 1
	
然后会有提示：

	Auto-selecting single ABI armeabi-v7a
	Android 4.0.3 is a basic Android platform.
	Do you wish to create a custom hardware profile [no]no
	Created AVD 'test2' based on Android 4.0.3, ARM (armeabi-v7a) processor,
	with the following hardware config:
	hw.lcd.density=240
	vm.heapSize=48
	hw.ramSize=512

创建好了，不过你的模拟器估计一运行就出错吧。（我故意滴。。。别怪我。。）

切换到刚才创建模拟器的目录，然后看看有什么东东，打开config.ini

	hw.lcd.density=240
	skin.name=WVGA800
	skin.path=platforms/android-15/skins/WVGA800
	hw.cpu.arch=arm
	abi.type=armeabi-v7a
	hw.cpu.model=cortex-a8
	vm.heapSize=48
	hw.ramSize=512
	image.sysdir.1=system-images/android-15/armeabi-v7a/
	
知道该做什么了吧？改吧，把相关的目录改成你自己的，然后再执行一下

	emulator @test2

怎么样！是不是已经运行啦？呵呵，恭喜哈!

本说明到此为止，同学们，好好学习哈，其实很简单，主要就是一种思路而已，加油吧