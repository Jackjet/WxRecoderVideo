# [WxRecoderVideo](https://github.com/maimingliang/WxRecoderVideo)

## 简介
基于[VCamera](https://www.vitamio.org/Download/)，仿微信录制短视频

![这里写图片描述](https://github.com/maimingliang/WxRecoderVideo/blob/master/recoder.gif)

## 使用

1)  在build.gradle，添加wechatRecoderVideoLibrary module 。


### 配置manifest

```xml
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

* 配置拍摄视频缓存路径
```xml
<application
    ...
       // 设置拍摄视频缓存路径
        File dcim = Environment
                .getExternalStoragePublicDirectory(Environment.DIRECTORY_DCIM);
        if (DeviceUtils.isZte()) {
            if (dcim.exists()) {
                VCamera.setVideoCachePath(dcim + "/recoder/");
            } else {
                VCamera.setVideoCachePath(dcim.getPath().replace("/sdcard/",
                        "/sdcard-ext/")
                        + "/recoder/");
            }
        } else {
            VCamera.setVideoCachePath(dcim + "/WeChatJuns/");
        }

//		VCamera.setVideoCachePath(FileUtils.getRecorderPath());
        // 开启log输出,ffmpeg输出到logcat
        VCamera.setDebugMode(true);
        // 初始化拍摄SDK，必须
        VCamera.initialize(this);
```

* 注册activity

```xml
<application
    ...
   <activity android:name="com.maiml.wechatrecodervideolibrary.recoder.WechatRecoderActivity"
            android:configChanges="orientation|keyboardHidden|screenSize"
            android:hardwareAccelerated="true"
            android:launchMode="singleTop"
            android:screenOrientation="portrait"
            android:theme="@style/CameraTheme"
            />
</application
```


* 调用 WechatRecoderActivity
```code
   WechatRecoderActivity.launchActivity(MainActivity.this,REQ_CODE);
```
*在 onActivityResult Method 接收结果
```code
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if(RESULT_OK == resultCode){

            if(requestCode == REQ_CODE){
                String videoPath = data.getStringExtra(WechatRecoderActivity.VIDEO_PATH);

                play(videoPath);
            }

        }
    }
```

##参数配置

###自定义dialog
>拍摄完成需要对视频进行转码，转码过程中弹出的dialog。

让你的Activity implements OnDialogListener 例如：

```code

 public class MainActivity extends AppCompatActivity implements OnDialogListener{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
		WechatRecoderActivity.launchActivity(MainActivity.this,REQ_CODE);
     }

    /**
     * 处理自定义Dialog 的显示
     * @param context 自定义dialog 依赖的Context，注意：自定义dialog的Context 需要使用这个
     *
     */
    @Override
    public void onShowDialog(Context context) {

    }
    /**
     * 处理自定义Dialog 的隐藏
     * @param context 自定义dialog 依赖的Context，注意：自定义dialog的Context 需要使用这个
     *
     */
    @Override
    public void onHideDialog(Context context) {

    }
}

```

###配置参数

|name|format|description|
|:---:|:---:|:---:|
| recoderTimeMax| integer |录制的最长时间
| recoderTimeMin| integer |录制的最短时间
| titleBarCancelTextColor| integer |titleBar取消字体的颜色
| pressBtnColor| integer|按住拍字体的颜色
| pressBtnBg| integer|圆环的颜色
| lowMinTimeProgressColor| integer|Progress小于录制最短时间的颜色
| progressColor| integer|Progress大于录制最短时间的颜色

注意：颜色值均为 十六进制值，例如：0xFFFC2828

![这里写图片描述](https://github.com/maimingliang/WxRecoderVideo/blob/master/img_des1.png)

![这里写图片描述](https://github.com/maimingliang/WxRecoderVideo/blob/master/img_des2.png)


##自定义参数

```code

	 RecoderAttrs attrs = new RecoderAttrs.Builder()
                            .pressBtnColorBg(0xff00ff00)
                            .titleBarCancelTextColor(0xff00ff00)
                            .pressBtnTextColor(0xff00ff00)
                            .build();
        WechatRecoderActivity.launchActivity(MainActivity.this,attrs,REQ_CODE);

```

##Thanks

[VCamera](http://wscdn.miaopai.com/download/VCameraRecorder3.1.pdf)