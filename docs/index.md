# InstaVu WebPlugin Usage

Using InstaVu web plugin you can display your product in 3D to your website. You need to load the javascript library to your website and initialize the viewport.

## Include
```html
<script type="text/javascript" src="https://instavu3d.com/webplugin/instavu.plugin.lib.js"></script>
```

## Get your scene id
**https://instavu3d.com/webplugin/info/{API_KEY}**

## Viewport

Models will display on the Viewport with the help of Renderer API. Contact administrator for your API key.

### Initialize Viewport
HTML:
```html
<div id="3d_container"></div>
```       
JS:
```js
var viewport = new InstaVUPlugin({
    container               : document.getElementById('3d_container'),
    apiKey                  : '{API_KEY}',      //{Required} Provide by administrator
    scene_id                : '{SCENE_ID}',     //{Required} Provide by administrator
});
```

### Events

- `scene_load:progress` This event will return you the progress of scene loading
- `scene_load:complete` Event triggered when the scene load on the viewport successfully

```js
viewport.addEventListener('scene_load:progress', function ( event ) {
    console.log(event.message); // The percentage of scene loading
});
viewport.addEventListener('scene_load:complete', function ( event ) {
    console.log(event.message);
});
```

##### Also you can use
```js
viewport.on('scene_load:progress', function ( event ) {
    console.log(event.message); // The percentage of scene loading
});
viewport.on('scene_load:complete', function ( event ) {
    console.log(event.message);
});
```

### Variable
Using this variable you can get available surfaces. This will help you to load the texture on particular surface.
```js
viewport.surfaces

// Data of viewport.surfaces
// [
// 	{
// 		"name": "surface1",
// 		"image": "surface1_thumbnail.jpg"
// 	},
// 	{
// 		"name": "surface2",
// 		"image": "surface2_thumbnail.jpg"
// 	},
// 	{
// 		"name": "surface3",
// 		"image": "surface3_thumbnail.jpg"
// 	}
// ]

// Surface Thumbnail URL
// http://instavu3d.com/webplugin/projects/assets/<scene_id>/<thumbnail>
```

### Methods
#### loadTexture
Load the texture to the surface.

Once the scene loaded successfully, you can load texture on the surface.

#### Parameters

-   `name` **[String][1]?** Name of the surface
-   `textureUrl` **[String][1]?** Texture url
-   `onSuccess` **[Function][2]?** called on completion with no argument.
-   `onError` **[Function][2]?** called on fail to load with one argument `(error)`. **[Error][3]?**
-   `onProgress` **[Function][2]?** called on progress loading with one argument `(progress)`. **[event][12]?**

```js
var url = '{TEXTURE URL}';
viewport.loadTexture('<name>',url,function(){
    //Call on success   
}, function(error){
    //Call on error
},function(progress){
    //Call on progress
});
```

Example:
```js
var url = '{TEXTURE URL}';
for (var i = 0; i < viewport.surfaces.length; i++){
    viewport.loadTexture(viewport.surfaces[i]['name'],url, function(){
        console.log('Texture loaded successfully');
    });
}
```


#### loadColor
Load the color to the scene.

Once the scene loaded successfully, you can load color on the scene.

##### Parameters

-   `name` **[String][1]?** Name of the surface (required)
-   `r` **[Number][8]?** Red color of RGB - Integer 0 to 255 (required)
-   `g` **[Number][8]?** Green color of RGB - Integer 0 to 255 (required)
-   `b` **[Number][8]?** Blue color of RGB - Integer 0 to 255 (required)

```js
var color = {
	r: 212,
	g: 112,
	b: 32
}
viewport.loadColor(name, color.r,color.g,color.b);
```

#### resize

Resize the viewport

##### Parameters

-   `width` **[Number][8]?** (Required)
-   `height` **[Number][8]?** (Required)

```js
viewport.resize(width,height);
```

#### resetPosition
Reset the scene at begin position.
```js
viewport.resetPosition();
```

#### toImage
Get base64 image of the canvas

Returns **[DataURI][10]?**

```js
viewport.toImage();
```

#### getImageBlob
Get Blob image of the canvas

Returns **[DataURI][11]?**

```js
viewport.getImageBlob();
```


# Example
```html
<!doctype html>
<html>
    <body>
        <div id="3d_container" width="800" height="600"></div>
        <script type="text/javascript" src="https://instavu3d.com/webplugin/instavu.plugin.lib.js"></script>
        <script type="text/javascript">
            var viewport = new InstaVUPlugin({
                container   : document.getElementById('3d_container'),
                apikey      : '<YourAPIKey>',
                scene_id    : '<YourSceneId>',
            });
            viewport.addEventListener('scene_load:progress', function (event){
                console.log(event.message);
            });
            viewport.addEventListener('scene_load:complete', function (event){
                console.log('Scene loaded successfully');

                loadTexture('<TextureUrl>');
            });

            function loadTexture(textureUrl){
                //Show loader
                for (var i = 0; i < viewport.surfaces.length; i++){
                    viewport.loadTexture(viewport.surfaces[i]['name'],textureUrl, function(){
                        //Hide loader
                    });
                }
            }
        </script>
    </body>
</html>
```

[1]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String

[2]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function

[3]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Error

[4]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise

[5]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[6]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean

[7]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement

[8]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number

[9]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array

[10]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs

[11]: https://developer.mozilla.org/en-US/docs/Web/API/Blob

[12]: https://developer.mozilla.org/en-US/docs/Web/API/ProgressEvent#Properties