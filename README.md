# 相册选择库
相册选择和相机拍照，裁剪，图片旋转功能，支持动画弹窗，裁剪设置例如状态栏颜色，缩放比例，裁剪框横竖颜色，横竖线数量设置等

[![](https://jitpack.io/v/AnglePengCoding/android_picture.svg)](https://jitpack.io/#AnglePengCoding/android_picture)

<h3>Add the dependency</h3>

```java

  implementation 'com.github.AnglePengCoding:android_picture:Tag'

```

<h3>项目示范流程</h3>
[![](https://github.com/AnglePengCoding/android_picture/blob/main/GIF/gif1.gif)

<div align=center><img width="250" height="350" src="https://github.com/AnglePengCoding/android_picture/blob/main/GIF/gif1.gif"/></div>

<h3>如何使用？</h3>

```java

          findViewById<Button>(R.id.mBtTake).setOnClickListener {
            PictureChooseDialog.build(this) {
                setFileTextSize(18f)//设置dialog“相册”按钮字体大小
                setFileTextColor(Color.parseColor("#FF3700B3"))//设置dialog“相册”按钮字体颜色
                setCameraTextSize(15f)//设置dialog“相机”按钮字体大小
                setCameraTextColor(R.color.black)//设置dialog“相机”按钮字体颜色
                setAnimationDuration(2000)//设置dialog动画时长 可不设置
                pictureDialogAnimation(PictureDialogAnimation.TranslateFromBottom)//设置dialog弹窗动画  可不设置
                setCameraDialogVisibility(true)//设置dialog 相机按钮隐藏  根据业务需求
                setFileDialogVisibility(true)//设置dialog 相册按钮隐藏  根据业务需求
                setUCropToolbarColor(R.color.teal_200)//设置裁剪ToolbarColor   可不设置
                setUCropStatusBarColor(R.color.teal_200)//设置裁剪状态栏颜色   可不设置
                setMaxScaleMultiplier(2f)//裁剪最大缩放比例  可不设置
                setImageToCropBoundsAnimDuration(1000)//设置图片在切换比例时的动画  可不设置
                setShowCropFrame(true)//设置是否展示矩形裁剪框  可不设置
                setCropGridStrokeWidth(R.color.teal_200)//设置裁剪框横竖线的颜色 可不设置
                setCropGridColumnCount(1)//设置裁剪竖线的数量 可不设置
                setCropGridRowCount(2)//设置裁剪横线的数量 可不设置
                show() //必设置
            }
        }

        
```


<h3> onActivityResult 方法</h3>

```java


    @Deprecated("Deprecated in Java")
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        if (resultCode == RESULT_OK) {
            when (requestCode) {
                PictureUtils.GET_IMAGE_FROM_PHONE -> { //选择相册之后的处理
                    data?.data?.let { PictureUtils.initUCrop(this, it) }
                }

                PictureUtils.GET_IMAGE_BY_CAMERA -> { //选择相机之后的处理
                    PictureUtils.initUCrop(this, PictureUtils.imageUriFromCamera)
                }

                UCrop.REQUEST_CROP -> {
                    val resultUri = UCrop.getOutput(data!!)
                    val my_avatar =
                        File(PictureUtils.getImageAbsolutePath(this, resultUri).toString())
                    findViewById<ImageView>(R.id.mIv).post {
                        findViewById<ImageView>(R.id.mIv).setImageURI(resultUri)
                    }

//                    val partList: MutableList<MultipartBody.Part> = ArrayList()
//                    val requestBody =
//                        RequestBody.create(MediaType.parse("multipart/form-data"), my_avatar)
//                    val imageBodyPart =
//                        MultipartBody.Part.createFormData("files", my_avatar.name, requestBody)
//                    partList.add(imageBodyPart)
                }
            }
        }
        super.onActivityResult(requestCode, resultCode, data)
    }

```



