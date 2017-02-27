# MyBB-FancyboxThumbnails
Small Fancybox Integration on MyBB 1.8.x


I was looking for someting like `Fit on page` for MyBB 1.8, but without good results. Build-in autoscaling is good, but not comfortable for me. 
I tried some code and find better solution. We can you Fancybox (or potential, any other similar JS library) to load images on demand in good-looking dialogs. Let's do this. 

## 1. Edit theme CSS
Just edit your theme CSS and add this code:

```css
.scaleimages img {
    max-width: 400px;
    max-height: 400px;
}
```

That `400 px` is only an example. You should use best value for your board.


##2. Download Fancybox 
You must download Fancybox 3.0 from official site and upload files to your board root directory:


http://fancyapps.com/fancybox/3/

Let's say, that your index.php file is on /index.php.

You should move files from fancyBox-3.0/dist to /fancybox on your server (please create new directory).


##3. Add Fancybox to posts images
Log in into board ACP and edit your theme `headerinclude` template.
Add this code after `{$stylesheets}`:

```html
<link rel="stylesheet" href="/fancybox/jquery.fancybox.min.css" type="text/css" media="screen" />
<script type="text/javascript" src="/fancybox/jquery.fancybox.min.js"></script>
<script type="text/javascript">
$(document).ready(function() {
    $('.post_body img').each(function() {
        var currentImage = $(this);
        
        if (currentImage.hasClass('smilie') == false && currentImage.parent().is('a') == false) {
            currentImage.wrap("<a target='_blank' data-fancybox data-type='image' href='" + currentImage.attr("src") + "'</a>");
        }
    });
});
</script>
```

This code will add links to all board images and enable Fancybox for them.

###4. Add Fancybox to thumbnails 
Log in into board ACP and edit your theme `postbit_attachments_thumbnails_thumbnail` template.
Change:

```html
<a href="attachment.php?aid={$attachment['aid']}"
```

to:

```html
<a href="attachment.php?aid={$attachment['aid']}" data-fancybox="data-{$post['pid']}" data-type="image"
```

This code will enabled Fancybox for your thumbnails in posts. We use post ID to provide images grouping.


## 5. Optional: move Fancybox CSS
You don't have to use Fancybox CSS file from third point of that instruction. You can always copy Fancybox CSS code and add it to your theme CSS or add new stylesheet with that. It's nice solution, because you can modify that code quickly and directly from your ACP.


## It's all!
It's all. Just save all templates and test Fancybox on your MyBB board!

