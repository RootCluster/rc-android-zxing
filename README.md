# rc-android-zxing

该分支仅作为源码阅读学习，此分支上做任何修改

## import project

官方项目  
![zxing](images/zxing-project.png)

准备工作
* 下载官方项目[zxing](https://github.com/zxing/zxing)
* 使用 AS 创建一个新Project

> 编译环境
>* android studio：3.4
>* gradle：5.1.1
>* sdk：28

导入步骤
* 导入module  
    ![import_module](images/import_module.png)
* 选择module  
    ![select_import_module](images/select_import_module.png)
* 移除最小及目标版本设置  
    ![remove_min_target_version](images/remove_min_target_version.png)
* 添加项目核心依赖  
    ![import_dependencies](images/import_dependencies.png)
* 删除 `app` module（可选，该分支已删除）

## show project
![zxing](images/zxing.gif)

## solve problem
<img src="images/project_problem.png" width="800" hegiht="313" align=center />

运行错误日志  
![zxing_error_log](images/zxing_error_log.png)

### 解决方法  
* 打开应用设置，手动设置应用相机权限
* 代码中加入权限校验
```java
// CaptureActivity.jaca
// line 266 && line 443
mHolder = surfaceHolder;
if (Build.VERSION.SDK_INT > Build.VERSION_CODES.LOLLIPOP_MR1) {
    if (ContextCompat.checkSelfPermission(CaptureActivity.this,
           android.Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED) {
        // 先判断有没有权限 ，没有就在这里进行权限的申请
        ActivityCompat.requestPermissions(CaptureActivity.this,
            new String[]{Manifest.permission.CAMERA}, CAMERA_OK);
        } else {
             // 说明已经获取到摄像头权限了
             initCamera(surfaceHolder);
         }
    } else {
        initCamera(surfaceHolder);
}

// line 803
@Override
public void onRequestPermissionsResult(int requestCode
        , @NonNull String[] permissions, @NonNull int[] grantResults) {
    // If request is cancelled, the result arrays are empty.
    if (requestCode == CAMERA_OK) {
        if (grantResults.length > 0
                && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
            // permission was granted, yay! Do the
            // contacts-related task you need to do.
            initCamera(mHolder);
        }
    }
}
```

### 其他问题
* 同样我们在`EncodeActivity.java`文件中加入读取内存卡的权限`READ_EXTERNAL_STORAGE`