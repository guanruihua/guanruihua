# Canvas转图片并下载

> 临时保存, 后面再记录

```html
<!DOCTYPE html>
<html lang="en">

<head>
 <meta charset="UTF-8">
 <meta http-equiv="X-UA-Compatible" content="IE=edge">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/rh-static@0.0.1/css/test-body.css">
 <title>index</title>
</head>

<style>

</style>

<body>
 <canvas id="canvas"></canvas>
 <button class="button-balanced" id="save" onclick="btnClick()">save</button>
 <br />
 <a href="" download="canvas_love.png" id="save_href">
  <img src="" id="save_img" />
 </a>
 <script src="https://cdn.jsdelivr.net/npm/rh-static@0.0.1/js/addWindowSize.js" defer></script>
 <script>
  var c = document.getElementById("canvas");
  function drawLove(canvas) {
   let ctx = canvas.getContext("2d");
   ctx.beginPath();
   ctx.fillStyle = "#E992B9";
   ctx.moveTo(75, 40);
   ctx.bezierCurveTo(75, 37, 70, 25, 50, 25);
   ctx.bezierCurveTo(20, 25, 20, 62.5, 20, 62.5);
   ctx.bezierCurveTo(20, 80, 40, 102, 75, 120);
   ctx.bezierCurveTo(110, 102, 130, 80, 130, 62.5);
   ctx.bezierCurveTo(130, 62.5, 130, 25, 100, 25);
   ctx.bezierCurveTo(85, 25, 75, 37, 75, 40);
   ctx.fill();
  }
  drawLove(c);

  function downLoad(url) {
   var oA = document.createElement("a");
   oA.download = '';// 设置下载的文件名，默认是'下载'
   oA.href = url;
   document.body.appendChild(oA);
   oA.click();
   oA.remove(); // 下载之后把创建的元素删除
  }

  var butSave = document.getElementById("save");
  function btnClick() {
   var svaeHref = document.getElementById("save_href");
   /*
    * 传入对应想要保存的图片格式的mime类型
    * 常见：image/png，image/gif, image/jpg, image/jpeg
    */
   var img = document.getElementById("save_img");
   var tempSrc = canvas.toDataURL("image/png");
   console.log(tempSrc)
   svaeHref.href = tempSrc;
   img.src = tempSrc;

   // location.href = tempSrc
   downLoad(tempSrc)
  }; 
 </script>
</body>

</html>
```
