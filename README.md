# EZCam [ ![Download](https://api.bintray.com/packages/omaflak/maven/ezcam/images/download.svg) ](https://bintray.com/omaflak/maven/ezcam/_latestVersion)

EZCam is an Android library that simplifies the use of Camera 2 API for a basic usage.

# Dependencie

Add the following line in your gradle dependencies :

```
compile 'me.aflak.libraries:ezcam:X.X'
```

Or if you use Maven :

```
<dependency>
  <groupId>me.aflak.libraries</groupId>
  <artifactId>ezcam</artifactId>
  <version>X.X</version>
  <type>pom</type>
</dependency>
```

# TextureView

**Important : The TextureView must be in a FrameLayout.**

```xml
<FrameLayout
android:layout_width="match_parent"
android:layout_height="match_parent">
<TextureView
    android:id="@+id/textureView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
</FrameLayout>
```

# Initialize

```java
EZCam cam = new EZCam(Context);
String id = cam.getCamerasList().get(CameraCharacteristics.LENS_FACING_BACK); // should check if LENS_FACING_BACK exist before calling get()
cam.selectCamera(id);
```

# Callback

```java
cam.setCameraCallback(new EZCamCallback() {
	@Override
	public void onCameraReady() {
		// triggered after cam.open(...)
		// you can set capture settings for example:
		cam.setCaptureSetting(CaptureRequest.COLOR_CORRECTION_ABERRATION_MODE, CameraMetadata.COLOR_CORRECTION_ABERRATION_MODE_HIGH_QUALITY);
		cam.setCaptureSetting(CaptureRequest.CONTROL_EFFECT_MODE, CameraMetadata.CONTROL_EFFECT_MODE_NEGATIVE);

		// then start the preview
		cam.startPreview();
	}

	@Override
	public void onPicture(Image image) {
		File file = new File(getFilesDir(), "image.jpg"); // internal storage
		File file = new File(getExternalFilesDir(null), "image.jpg") // external storage, need permissions
		EZCam.saveImage(image, file);
	}

	@Override
	public void onError(String message) {
		// all errors will be passed through this methods
	}

	@Override
	public void onCameraDisconnected() {
		// camera disconnected
	}
});
```

# Open Camera

```java
cam.open(CameraDevice.TEMPLATE_PREVIEW, textureView); // needs Manifest.permission.CAMERA
```
	
# Take picture

```java
cam.takePicture();
```
	
# Close camera

```java
@Override
protected void onDestroy() {
cam.close();
	super.onDestroy();
}
```

# TODO

- Recording videos
- Apply custom filters

See [MainActivity.java](https://github.com/omaflak/Android-Camera2-Library/blob/master/app/src/main/java/me/aflak/libraries/MainActivity.java)
