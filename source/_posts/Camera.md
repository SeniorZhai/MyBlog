title: Camera
date: 2014-08-06 13:47:24
categories: Android
tags: [Android]
---
1. 自定义Camera需要添加CAMERA权限
```xml
<uses-permission android:name="android.permission.CAMERA"/>
```
2. 通过Camera静态open方法访问摄像头
```java
Camera camera = Camera.open();
```
使用完成后调用`release`方法来释放它
```java
camera.release();
```
3. 摄像头属性
通过Camera对象的`getParameters`方法可以得到`Camera.Parameters`对象来对摄像头进行设置
Anroid 2.2引入`getFocalLength`和`get[Horizontal/Vertical]`方法分别可以得到焦距和相关水平和垂直视角
Android 2.3引入`getFocusDistances`方法，用于估算镜头和当前被对焦的物体之间的距离，该方法并不返回值，而是填充一个与近、远和最佳距离对应的浮点数组，对焦中最清晰的物体位于最佳位置
```java
float[] focusDistances = new float[3];

paramerters.getFocusDistance(focusDistance);

float near = focusDistances[Camera.Parameters.FOCUS_DISTANCE_NEAR_INDEX];
float far = focusDistance[Camera.Parameters.FOCUS_DISTANCE_FAR_INDEX];
float optimal = focusDistance[Camera.Parameters.FOCUS_DISTANCE_OPTIMAL_INDEX];
```
通过使用set*方法来修改Parameters参数
- [get/set]SceneMode 使用一个`SCENE_MODE_*`的常量设置所拍摄场景的类型。
- [get/set]FlashMode 使用一个`FLASH_MODE_*`常量设置闪光模式（通常为“打开”、“关闭”、“红眼消除”、“闪光灯”模式）
- [get/set]WhiteBalance 使用一个`WHITE_BALANCE_*`常量设置白平衡校正来校正场景。在设置白平衡之前，可以使用`getSupportedWhiteBalance`方法来确认哪些设置可用
- [get/set]AutoWhiteBalanceLock Android 4.0引入，使用自动白平衡算法，会暂停颜色校正算法。使用isAutoWhiteBalancLockSupported方法确认设备是否支持这种功能
- [get/set]ColorEffect 使用EEFECT_*常量舍hi特殊颜色效果，使用getColorEffect可以找到可用的颜色效果
- [get/set]FocusMode 使用FOCUS_MODE_*常量设置摄像头尝试对焦的方式（连续自动对焦在Android 4.0中引入）使用getSupportedFocusModes方法可以找出可用的模式。
- [get/set]Antibanding 使用ANTIBANDING_*常量设置降低条带效果的屏幕刷新频率。
===
- JPEG和缩略图质量 使用setJpegQuality和setJpegThumbnailQuality方法，并传入0到100之间的证书设置图像质量
- 图像、预览和缩略图大小 分别使用setPictureSize、setPreviewSize和setJpegThumbnailSize方法制定图像、预览和缩略图的高度和宽度。使用getSupportedPictureSize、getSupportedPreviewSizes和getSupportededJpegThumbnailSize方法来确定有效值，返回Camera.Size对象的列表。
- 图像和预览像素格式 使用PixelFormat类的静态常量调用`setPictyreFormat`和`setPreviewFormat`设置图像的格式。在使用getSupportedPictureFormats和getSupportedPreviewFormats方法返回支持的格式的一个列表。
- 预览帧速率 `setPreviewFpsRange`方法取代了弃用的`setPreviewFpsRangeFrameRate`（Android 2.3）指定预览的首选帧率范围。使用`getSupportedPreviewFpsRange`方法可以找出支持的最低和最高帧率。
4. 控制自动对焦、对焦区域和测光区域
```java
Camera.Parameters parameters = camera.getParameters();
if(parameters.getSupportedFocusModes().contains(Camera.Parameters.FOCUS_MODE_CONTINUOUS_PICTURE)) {
	parameters.setFocusMode(Camera.Parameters.FOCUS_MODE_CONTINUOUS_PICTURE);
	camera.autoFocus(new Auto FocusCallback(){
		public void onAutoFocus(boolean success,Camera camera){
			// 
		}
	})；
}
```
Android 4.0引入两个对焦的API，用于在对焦图像或者确定场景的白平衡和亮度时指定对焦区域和侧光区域。
使用`getMaxNumFocusAreas`方法来确定设备是否支持该功能，返回值为检测到的最大对焦区域，如果为0，表示设备不支持定义对焦区域。
定义对焦区域使用`setFocusAreas`方法，传入一个Camera.Area对象的了表。
使用`setMeteringAreas`以同样的方式设置侧光区域。
5. 使用摄像头预览
不显示一个预览，食物无法使用Camera对象拍摄照片的。
```java
public class CameraActivity extends Activity implements SufaceHolder.Callback {
	private static final String TAG = "CameraActivity";

	private Camera camera;

	@Override
	public void onCreate(Bundle saveInstanceState) {
		super.onCreate(saveInstanceState);
		setContentView(R.layout.main);

		SufaceView surface = (SurfaceView)findViewById(R.id.surfaceView);
		SufaceHolder holder = surface.getHolder();
		holder.addCallback(this);
		holder.setType(SurfaceHolder.SUREFACE_TYPE_PUSH_BUFFERS);
		holder.setFixed(400,300);
	}
	
	public void surfaceCreated(SurfaceHolder holder){
		try{
			camera.setPreviewDisplay(holder);
			camera.startPreview();
		} catch (IOException e) {
			//
		}
	}

	public void surfaceDestroyed(SurfaceHolder holder){
		camera.stopPreview();
	}

	public void surfaceChanged(SurfaceHolder holder,int format,int width,int height) {

	}

	@Override
	protected void onPaues(){
		super.onPause();
		camera.release();
	}

	@Override
	protected void onResume(){
		super.onResume();
		camera = Camera.open();
	}
}
```
调用camera的`setPreviewCallback`方法传入`PreviewCallback`，监听每个预览帧
```java
camera.setPreviewCallback(new PreviewCallback(){
	public void onPreviewFrame(byte[] data,Camera camera) {
		int quality = 60;

		Size[] previewSize = camera.getParameters().getPreviewSize();
		YuvImage iamge = new YuvImage(data,ImageFormat.NV21,previewSize.width,previewSize.height,null);
		ByteArrayOutStream outputStream = new ByteArrayOutStream();
		image.compressToJpeg(new Rect(0,0,previewSize.width,previewSize.height),quality,outputStream);
	}
});
```