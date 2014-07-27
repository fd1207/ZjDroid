ZjDroid
=======
<BR>
Android app dynamic reverse tool based on Xposed framework.<BR>
<BR>
前提条件：<BR>
1、已Root手机一部<BR>
2、[安装Xposed installer](http://dl.xposed.info/latest.apk),并从Xposed installer安装Xposed Framework<BR>

### 一、ZjDroid工具介绍
<BR>
ZjDroid是基于Xposed Framewrok的动态逆向分析模块，逆向分析者可以通过ZjDroid完成以下工作：<BR>
1、DEX文件的内存dump<BR>
2、基于Dalvik关键指针的内存BackSmali，有效破解主流加固方案<BR>
3、敏感API的动态监控<BR>
4、指定内存区域数据dump<BR>
5、获取应用加载DEX信息。<BR>
6、获取指定DEX文件加载类信息。<BR>
7、dump Dalvik java堆信息。<BR>
8、在目标进程动态运行lua脚本。<BR>
<BR>
<BR>
### 二、ZjDroid相关命令
<BR>
1、获取APK当前加载DEX文件信息：<BR>
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_dexinfo"}'<BR>
说用说明<BR>
> 	pid 调用时把pid换成进程的id<BR>
>  	查看结果:<BR>
> > 	从Android的LogCat中查看结果,得到当前加载的dex的信息如:
> 
> > 	The DexFile Infomation ->
> 
> > 	07-27 02:29:52.728: D/zjdroid-shell-com.evernote(5365): filepath:/data/app/com.evernote-2.apk mCookie:1770063976
> 
> > 	End DexFile Infomation

<BR>
2、获取指定DEX文件包含可加载类名：<BR>
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_class","dexpath":"*****"}'<BR>
说用说明<BR>
> 	pid 调用时把pid换成进程的id
> 
> > 	dexpath 从上dex文件地址,如/data/app/com.evernote-2.apk
> 
> >  查看结果:
> 
> > 	从Android的LogCat中查看结果,得到当前加载的class信息.

<BR>
4、根据Dalvik相关内存指针动态反编译指定DEX，并以文件形式保存。<BR>
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"backsmali","dexpath":"*****"}'<BR>
<BR>
该方式可以脱壳目前大部分流行的加固防护。(由于手机性能问题，运行较忙)<BR>
例外情况：<BR>
> 由于ApkProtect特定防修改检测，需要做如下修改即可解固该保护：
> 
> （1）在设备上创建特定目录（如/data/local）并 chmod 为777
> 
> （2）复制zjdroid.apk到该目录，并修改文件名为zjdroid.jar
> 
>  (3) 修改/data/data/de.robv.android.xposed.installer/conf/modules.list 模块代码文件修改为"zjdroid.jar"
> 重启设备即可。

<BR>
5、Dump指定DEX内存中的数据并保存到文件（数据为odex格式，可在pc上反编译）。<BR>
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_dexfile","dexpath":"*****"}'<BR>
<BR>
<BR>
6、Dump指定内存空间区域数据到文件<BR>
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_mem","startaddr":1234567,"length":123}'<BR>
说用说明<BR>
> 	startaddr 注意此值是10进制

7、Dump Dalvik堆栈信息到文件，文件可以通过java heap分析工具分析处理。<BR>
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"dump_heap"}'<BR>
<BR>
8、运行时动态调用Lua脚本,[luajava相关使用方法：](http://www.keplerproject.org/luajava/)<BR>
该功能可以通过Lua脚本动态调用java代码。<BR>
使用场景：<BR>
可以动态调用解密函数，完成解密。<BR>
可以动态触发特定逻辑。<BR>
am broadcast -a com.zjdroid.invoke --ei target pid --es cmd '{"action":"invoke","filepath":"****"}'<BR>

9、敏感API调用监控<BR>
<BR>
<BR>
### 三、相关命令执行结果查看：
<BR>
1、命令执行结果：<BR>
adb shell logcat -s zjdroid-shell-{package name}<BR>
<BR>
2、敏感API调用监控输出结果：<BR>
adb shell logcat -s zjdroid-apimonitor-{package name}<BR>
