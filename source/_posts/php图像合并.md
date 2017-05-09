---
title: php圆形头像生成与合并
date: 2017-05-09 15:01:29
tags: php
---
> 实习中的一个项目，用到将两张图片合并成一张，使用了PHP的图像函数，发现挺好处理的。

#圆形头像的生成#

在项目中需要获取用户头像并制作成固定大小的圆形头像，如果用css是很容易解决的，`border-radius`设置为尺寸的50%,但是不能用css，因为要求返回的是一张图片而不是页面，这确实让我困惑了很久。
说一说代码的实现：
 

 1. 明确头像要求的尺寸，假设为`new_size`，然后获取原始头像的url

   ` $url = 'xxxx';`
    `$new_size = 'xxx';`
    `$oldimg = imagecreatefromjpeg($url);`
    `size = [imagesx($oldimg),imagesy($oldimg)];
    $newimg = imagecreatetruecolor($new_size);
    imagecopyresized($newimg,$oldimg,0,0,0,0,$new_size,$new_size,size[1],size[2]);`
   ` //圆形透明图像`
   ` imagealphablending($newpic,false); ` 
    `$transparent = imagecolorallocatealpha($newimg, 0, 0, 0, 127);`  
   ` $r=$new_size/2; ` 
    `for($x=0;$x<$new_size;$x++) {`
       ` for($y=0;$y<$new_size;$y++){  `
           ` $c = imagecolorat($src,$x,$y);`  
          `  $_x = $x - $r;  `
           ` $_y = $y - $r;  `
          `  if((($_x*$_x) + ($_y*$_y)) < ($r*$r)){  `
               `  imagesetpixel($newimg,$x,$y,$c);  `
           ` }else{  `
               ` imagesetpixel($newimg,$x,$y,$transparent); ` 
           `}  `
                `}  `
           ` imagesavealpha($newimg, true);  `
           ` header('Content-Type: image/png');  `
           ` imagepng($newpic);  `
          `  imagedestroy($newpic);  `
           ` imagedestroy($old);  `
        `} `

注意：图片最后保存为png格式，之前尝试过保存为jpeg，然后透明部分就变成了黑色，估计应该是png能保留透明通道的原因。

#合并图片#
合并图片用的是php的`imagecopymerge()`函数，接上代码：

    $bgimgurl = 'xxx';
$bgimg = imagecreatefromjpeg($bgimgurl);    $src_size= [imagesx($bgimg),imagesy($bgimg)];
    imagecopymerge($bgimg, $newimg, $dst_x , $dst_y , $src_x , $src_y , $src_w , $src_h , $pct);
    
一些参数的释义如下：
dst_x	目标图像开始 x 坐标
dst_y	目标图像开始 y 坐标，x,y同为 0 则从左上角开始
src_x	拷贝图像开始 x 坐标
src_y	拷贝图像开始 y 坐标，x,y同为 0 则从左上角开始拷贝
src_w	（从 src_x 开始）拷贝的宽度
src_h	（从 src_y 开始）拷贝的高度
pct	图像合并程度，取值 0-100 ，当 pct=0 时，实际上什么也没做，反之完全合并。

