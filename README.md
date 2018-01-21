PicAtSize
===========================================

This version was compiled using Titanium SDK 7.0.1.GA

PicAtSize is an Android module, fork of skypanther/picatsize, that lets you take a "_PIC_&#65279;ture _AT_ a _SIZE_" you specify. By taking a  photo smaller than the camera's native size, you create smaller files which are faster to upload, faster to process (resize, crop, rotate), and which create less memory-related issues. It was the memory issue that inspired this module.

Natively, Android supports the `camera.getParameters.setPictureSize()` method. But Titanium's Ti.Media module does not expose this functionality. This module makes that available for you to use.

Additionally this fork exposes a setZoom(level) and setZoomPercentage(1-100) methods, to allow control of the camera's Zoom capability. 

A few notes:

* With this module, you must use a camera overlay or you'll get the native camera which has no size-specifying support.
* You will get a photo at the closest, next larger size supported by the device's camera. It may not, and probably won't match the exact size you specify. (Reportedly, some Android devices will not take any photo if you were to specify a size that the camera doesn't support.)




## INSTALL THE MODULE

### Use GitTio

Use GitTio, as it will download, unzip, and install the module for you.

```shell
gittio install cl.creador.picatsize
```

### Manually

Download the zip from the dist folder, unzip, place in your project's modules folder.

Register in the tiapp.xml:

```xml
<modules>
	<module platform="android">cl.creador.picatsize</module>
</modules>
```



## USING THE MODULE IN CODE

See the included sample app (in the example directory).

```javascript
// instantiate and specify your target size
var pascam = require('cl.creador.picatsize');
var targetWidth = 1024;
var targetHeight = 800;

// create the necessary overlay
var overlay = Ti.UI.createView();

var clickBt = Ti.UI.createButton({
	title: "Take Photo",
	bottom: 10,
	right: 20
});
overlay.add(clickBt);
clickBt.addEventListener('click', function() {
	pascam.takePicture();
});
var closeBt = Ti.UI.createButton({
	title: "Cancel",
	bottom: 10,
	left: 20
});
overlay.add(closeBt);
closeBt.addEventListener('click', function() {
	pascam.hideCamera();
});
//zoom capability
var slider = Ti.UI.createSlider({
	bottom: 10
	min: 0,
	max: 100
});
overlay.add(slider);
slider.addEventListener('change', function(e) {
	pascam.setZoomPercentage(e.value);
});

// show the camerapascam.showCamera({
	overlay: overlay, // setting the overlay makes the module start PASCameraActivity
	autohide: false, // autohide must be false for takePicture() to work
	success: function (e) {
		Ti.API.info("bytes: " + e.media.length);
		Ti.API.info("height: " + e.media.height);
		Ti.API.info("width: " + e.media.width);
		pascam.hideCamera();
	},
	error: function (e) {
		Ti.API.info(JSON.stringify(e));
	},
	saveToPhotoGallery: false,
	mediaTypes: [pascam.MEDIA_TYPE_PHOTO],
	targetWidth: targetWidth,
	targetHeight: targetHeight
});

```

# License

Apache License, Version 2.0
