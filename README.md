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

日本地図を表示するページです。
index.htmlでJapanMapを読み込むわけではないので、この画面でjsファイルを読み込む必要はありません。
10行目のiframeタグでmap.htmlを読み込むことで、日本地図を表示しています。getパラメータでcanvasWidthを数値で指定することで、動的にマップのサイズを変更可能です。
iframeのサイズはmap.html読み込み後のcanvasのサイズを元に設定しています。（12～23行目）

## map.html

~~~html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8" />
    <title>2段階選択式の日本地図</title>
    <link rel="stylesheet" href="css/japanmap.css">
    <script src="js/jquery-1.12.1.min.js"></script>
    <script src="js/jquery.japan-map.js"></script>
    <script>
        $(function(){

            var $mapContainer = $(document).find("#japan-map-container");
            var canvasWidth = _getUrlParams()['canvasWidth'] && $.isNumeric(_getUrlParams()['canvasWidth']) ? _getUrlParams()['canvasWidth'] : 800;
            var mapWidth = null;
            var mapHeight = null;
            var backgroundPrefColor = "#ababab";

            /*
             * 2段階選択式の日本地図を表示する
             */
            _initJapanMap();
            function _initJapanMap() {
                $("#japan-map-back-btn").hide();
                $mapContainer.empty().japanMap({
                    areas: [
                        {code : 1, name: "北海道", color: "#7f7eda", hoverColor: "#b3b2ee", prefectures: [1]},
                        {code : 2, name: "東北地方",   color: "#759ef4", hoverColor: "#98b9ff", prefectures: [2,3,4,5,6,7]},
                        {code : 3, name: "関東地方",   color: "#7ecfea", hoverColor: "#b7e5f4", prefectures: [8,9,10,11,12,13,14]},
                        {code : 4, name: "中部地方",   color: "#7cdc92", hoverColor: "#aceebb", prefectures: [15,16,17,18,19,20,21,22,23]},
                        {code : 5, name: "関西地方",   color: "#ffe966", hoverColor: "#fff19c", prefectures: [24,25,26,27,28,29,30]},
                        {code : 6, name: "中国地方",   color: "#ffcc66", hoverColor: "#ffe0a3", prefectures: [31,32,33,34,35]},
                        {code : 7, name: "四国地方",   color: "#fb9466", hoverColor: "#ffbb9c", prefectures: [36,37,38,39]},
                        {code : 8, name: "九州地方",   color: "#ff9999", hoverColor: "#ffbdbd", prefectures: [40,41,42,43,44,45,46]},
                        {code : 9, name: "沖縄",   color: "#eb98ff", hoverColor: "#f5c9ff", prefectures: [47]},
                    ],
                    movesIslands : true,
                    borderLineWidth: 0,
                    width: canvasWidth,
                    onSelect:function(data){
                        switch (data.code){

                            // 北海道
                            case 1:
                                $mapContainer.empty().japanMap({
                                    areas: [
                                        {code : 1, name: "北海道", color: "#7f7eda", hoverColor: "#b3b2ee", prefectures: [1]},
                                        {name: "", color: backgroundPrefColor, hoverColor: backgroundPrefColor, prefectures: [2,3,5]},
                                    ],
                                    width: canvasWidth * 2.5,
                                    onSelect:function(data){
                                        _sendData(data);
                                    }
                                });
                                $mapContainer.find("canvas").css({"top": mapHeight * 0.15, "left": 0 - (mapWidth * 1.55)});
                                $("#japan-map-back-btn").show();
                                break;

                            // 東北
                            case 2:
                                $mapContainer.empty().japanMap({
                                    areas: [
                                        {code : 2, name: "青森県",    color: "#759ef4", hoverColor: "#98b9ff", prefectures: [2]},
                                        {code : 3, name: "岩手県",    color: "#759ef4", hoverColor: "#98b9ff", prefectures: [3]},
                                        {code : 4, name: "宮城県",    color: "#759ef4", hoverColor: "#98b9ff", prefectures: [4]},
                                        {code : 5, name: "秋田県",    color: "#759ef4", hoverColor: "#98b9ff", prefectures: [5]},
                                        {code : 6, name: "山形県",    color: "#759ef4", hoverColor: "#98b9ff", prefectures: [6]},
                                        {code : 7, name: "福島県",    color: "#759ef4", hoverColor: "#98b9ff", prefectures: [7]},
                                        {name: "", color: backgroundPrefColor, hoverColor: backgroundPrefColor, prefectures: [1,8,9,10,11,15,16,17,18,20,21]},
                                    ],
                                    width: canvasWidth * 2.8,
                                    onSelect:function(data){
                                        _sendData(data);
                                    }
                                });
                                $mapContainer.find("canvas").css({"top": 0 - mapHeight * 0.55, "left": 0 - (mapWidth * 1.65)});
                                $("#japan-map-back-btn").show();
                                break;

～～～～～～～～～～～～～～～～～～～～
　　　　　　　　　省略
～～～～～～～～～～～～～～～～～～～～

                    },
                });

                // 初回のみ実施
                if (mapWidth == null && mapHeight == null) {
                    mapWidth = $mapContainer.find("canvas").attr("width");
                    mapHeight = $mapContainer.find("canvas").attr("height");
                    $mapContainer.css({"width": mapWidth, "max-width": mapWidth, "height": mapHeight, "max-height": mapHeight});
                }
            }

            /*
             * 都道府県選択時のデータ送信処理
             */
            function _sendData(data) {
                if (data.code) {
                    window.parent.location.href = "/pref/" + data.code + "/";
                }
            }

            /*
             * getパラメータを取得
             */
            function _getUrlParams() {
                var vars = [], hash;
                var hashes = window.location.href.slice(window.location.href.indexOf('?') + 1).split('&');
                for (var i = 0; i < hashes.length; i++) {
                    hash = hashes[i].split('=');
                    vars.push(hash[0]);
                    vars[hash[0]] = hash[1];
                }
                return vars;
            }

            /*
             * 戻るボタンクリック時の全国マップ表示処理
             */
            $("#japan-map-back-btn").on("click", function(){
                _initJapanMap();
            });
        });
    </script>
</head>
<body>
<div id="japan-map-container"></div>
<input type="button" value="戻る" id="japan-map-back-btn"/>
</body>
</html>
~~~
日本地図を実際に描画するhtmlです。  
_initJapanMapメソッドで2段階選択式の日本地図を描画しています。地方選択画面で地方を選択したときのコード値を元に都道府県選択画面を描画するイメージ。  
_sendDataメソッドで都道府県を選択したときの処理を記述しています。  
_getUrlParamsメソッドでmap.html読み込み時のgetパラメータを取得しています。  
areasオプション内の配列でcodeを未定義とすることで都道府県選択画面で選択対象外の近隣の都道府県を灰色表示するようにしています。都道府県選択画面はcanvasを拡大し表示位置をcssで補正しています。そのため、都道府県選択画面は都道府県の輪郭が若干粗くなってしまっています。都道府県選択画面から地方選択画面に戻るボタンを表示しています。
