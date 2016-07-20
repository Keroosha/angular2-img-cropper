# Changelog

### Release 0.5.0

 - introduced flag to hide the component file input in order to allow customization
 - added pinch/zoom feature for touch devices

###  Release 0.4.5:
 - introduced rounded cropper: cropperSettings.rounded = true. Making keep aspect = false will throw an error on rounded cropper. (Issue #14)
 - cropper takes into consideration source image data pixels not cropper image data. (Issue #17)
 - support for minSize now have the following option: minWithRelativeToResolution. When set to false it will keep min size relative to canvas size. (Issue #21)
 - allow user to customize look and feel of the cropper:  
        this.cropperSettings.cropperDrawSettings.strokeColor = 'rgba(255,255,255,1)';
        this.cropperSettings.cropperDrawSettings.strokeWidth = 2;

### Release 0.4.2:
Starting with: 0.4.2 ts files are no loger published (only js & d.ts).
Please change your system.config files to make use of the js files.
```
 'ng2-img-cropper' :           { main: 'index.js', defaultExtension: 'js' }
```



# ng2-img-cropper

This is an adapatation of Angular 1 image cropper from: https://github.com/AllanBishop/angular-img-cropper
An image cropping tool for AngularJS. Features a rectangular crop area. The crop area's aspect ratio can be enforced during dragging. 
The crop image can either be 1:1 or scaled to fit an area.

## Install from NPM

```
    npm i ng2-img-cropper --save
```

## Screenshot

![Screenshot](https://raw.githubusercontent.com/cstefanache/cstefanache.github.io/master/assets/img/cropper.png "Screenshot")

## Testing

```
    npm install
    npm run all
```

## Example usage

```typescript
import {Component} from 'angular2/core';
import {ImageCropperComponent, CropperSettings} from 'ng2-img-cropper';


@Component({
    selector: 'test-app',
    template: `<div>
        <img-cropper [image]="data" [settings]="cropperSettings"></img-cropper><br>
        <img [src]="data.image" [width]="cropperSettings.croppedWidth" [height]="cropperSettings.croppedHeight">
    </div>`,
    directives: [ImageCropperComponent]
})
export class AppComponent {
    data: any;
    cropperSettings: CropperSettings;

    constructor() {

        this.cropperSettings = new CropperSettings();
        this.cropperSettings.width = 100;
        this.cropperSettings.height = 100;
        this.cropperSettings.croppedWidth =100;
        this.cropperSettings.croppedHeight = 100;
        this.cropperSettings.canvasWidth = 400;
        this.cropperSettings.canvasHeight = 300;

        this.data = {};

    }
}

```

Checkout this [sample plunker](https://embed.plnkr.co/V91mKCNkBQZB5QO2MUP4/)


## Settings

* **canvasWidth**:*number* - Canvas DOM Element width
* **canvasHeight**:*number* - Canvas DOM Element height
* **width**:*number* - Crop Width
* **height**:*number* - Crop Height
* **minWidth**:*number* - Minimum crop Width
* **minHeight**:*number* - Minimum crop height
* **croppedWidth**:*number* - Resulting image width
* **croppedHeight**:*number* - Resulting image height
* **touchRadius**:*number* - (default: 20) Touch devices radius
* **minWithRelativeToResolution**:*boolean* - (default: true) By default the resulting image will be cropped from original image. If false, it will be cropped from canvas pixels
* **noFileInput**:*boolean* - (default: false) - hides the file input element from cropper canvas. 
* **cropperDrawSettings**:*CropperDrawSettings* - rendering options
    * **strokeWidth**:*number* - box/ellipsis stroke width
    * **strokeColor**:*string* - box/ellipsis stroke color

## Customizing Image cropper

Replacing component file input:

```html
<div class="file-upload">
    <span class="text">upload</span>
    <input id="custom-input" type="file" (change)="fileChangeListener($event)">
</div>
<img-cropper #cropper [image]="data" [settings]="cropperSettings"></img-cropper>
<br>
<span class="result rounded" *ngIf="data.image" >
    <img [src]="data.image" [width]="cropperSettings.croppedWidth" [height]="cropperSettings.croppedHeight">
</span>
```

```typescript

data:any;

@ViewChild('cropper', undefined) 
cropper:ImageCropperComponent;

constructor() {
    this.cropperSettings = new CropperSettings();
    this.cropperSettings.noFileInput = true;
    this.data = {};
}

fileChangeListener($event) {
    var image:any = new Image();
    var file:File = $event.target.files[0];
    var myReader:FileReader = new FileReader();
    var that = this;
    myReader.onloadend = function (loadEvent:any) {
        image.src = loadEvent.target.result;
        that.cropper.setImage(image);

    };

    myReader.readAsDataURL(file);
}


```