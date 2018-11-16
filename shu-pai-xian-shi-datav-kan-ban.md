# 树莓派显示datav看板

* 不用时候隐藏鼠标

```bash
# sudo apt-get install unclutter​​
```

*  创建启动文件

```bash
# vi /home/pi/.config/autostart/datav.desktop
```

*  内容下载链接：[http://xin-min.oss-cn-beijing.aliyuncs.com/ERPManage/datav.desktop](http://xin-min.oss-cn-beijing.aliyuncs.com/ERPManage/datav.desktop)

```text
[Desktop Entry]
Type = Application
Comment = Set datav full screen
NoDisplay = true 
Exec = chromium-browser --disable-popup-blocking --no-first-run --disable-desktop-notifications --kiosk 'https://datav.aliyun.com/share/935802f54ce6d8fd7b18661875bf5c5a?spm=datav.10712494.0.0.5c534a9aK653JX&whatever=true'
```

*  Crel+F4退出
*  设置显示屏长亮，请参考：[https://blog.csdn.net/u011720560/article/details/78288128](https://blog.csdn.net/u011720560/article/details/78288128)
*  系统时间矫正，请参考：[https://blog.csdn.net/github\_38111866/article/details/76057237](https://blog.csdn.net/github_38111866/article/details/76057237)



