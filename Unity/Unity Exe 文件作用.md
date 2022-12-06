
1. 一个后缀为 BurstDebugInformation_DoNotShip的文件夹，早期unity打包没有这个玩意，这个文件夹可以忽略和删除没有实际意义，内部是一个txt文本，主要是用来帮助查看调试当前打包的信息
2. XX_Data文件夹里面包含unity的一些默认资源和当前工程的一些必须资源
3. MonoBleedingEdge 文件夹包含运行应用程序所需的 C# 和 MonoDevelop 库。UnityPlayer.dll 包含引擎的核心，脚本库位于 MonoBleedingEdge 中。虽然运行游戏/应用程序需要其部分内容，但我们能够删除其中的“etc”文件夹，只保留 EmbedRuntime 文件夹。
4. 我们打包出来的可执行文件exe，启动项目双击这个就行
5. UnityCrashHandler64.exe 它是Unity的发布之后的崩溃处理程序，当你的程序崩溃的时候他会向Unity发送崩溃的日志，当然你是可以把他删除掉的，
6. UnityPlayer.dll unity的库文件，很重要不可删除
