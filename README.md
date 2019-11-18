# jQueryプラグインJapan Mapを利用して2段階選択式の日本地図を描画する

Canvasに選択可能な日本地図を描画するJapan MapというjQueryプラグインを使用して、地方選択→都道府県選択という2段階の選択が行えるようにしてみました。

[japanmapを使用した日本地図選択](http://etc.imo-tikuwa.com/japanmap/)

## index.html

~~~html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>japanmapを使用した日本地図選択</title>
    <link rel="stylesheet" href="css/japanmap.css">
    <script src="js/jquery-1.12.1.min.js"></script>
</head>
<body>
<iframe id="map" src="map.html?canvasWidth=800" frameborder="0" scrolling="no"></iframe>
<script>
  $(function(){
    var $mapObj = $("#map");
    $mapObj.on("load", function(){
      // iframe読み込み後、iframe内のcanvasのサイズを取得し、iframeのサイズを設定する
      var $iframeCanvas = $('iframe:first').contents().find("#japan-map-container > canvas");
      $mapObj.attr({
        width : $iframeCanvas.attr("width"),
        height : $iframeCanvas.attr("height")
      });
    });
  });
</script>
</body>
</html>
~~~
