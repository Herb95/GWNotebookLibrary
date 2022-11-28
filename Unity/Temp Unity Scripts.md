
### GUI Script

```C# 

    private bool isActiveGUIUI = false;
    private bool isActiveGUIUI2 = false;
    private Rect _winRect = new Rect(150, 150, 300, 300);
    private void OnGUI()
    {
        if (GUI.Button(new Rect(100,0,100,100),"是否开启测试弹窗"))
        {
            isActiveGUIUI = true;
        }
        if (GUI.Button(new Rect(Screen.width-100,0,100,100),"我是测试按钮"))
        {
            isActiveGUIUI2 = true;
        }
        
        if (isActiveGUIUI)
        {
            _winRect = GUILayout.Window(0, new Rect(150, 150, 300, 300), OpenLog, "我是测试");
        }
        if (isActiveGUIUI2)
        {
            _winRect = GUILayout.Window(1, new Rect(Screen.width-300, 150, 300, 300), OpenLog, "我是测试2");
        }
    }

    private void OpenLog(int winId)
    {
        if (winId == 0)
        {
            if (GUILayout.Button("我是打印按钮"))
            {
                Log.Warn("我是测试,我是测试我是测试我是测试我是测试");
            }
            if (GUILayout.Button("关闭窗口"))
            {
                isActiveGUIUI = false;
            }
        }
        else
        {
            
        }
    }
```