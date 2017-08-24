# SVG

SVGの基本構造

```html
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink">
  <circle cx="100" cy="100" r="100" fill="red" />
  <rect x="130" y="130" width="300" height="200" fill="blue" />
</svg>
```
svgタグの子（孫）要素として記述します。上の例ではcircleタグとrectタグを使って円と四角形を描画しています。

## HTMLでの表示方法

- objectタグを用いる
```html
<object type="image/svg+xml" data="sample/sample01.svg">
```
- imgタグやCSSを用いる
通常の画像の用に扱う。このsvgファイルの中にstyleやscriptを埋め込むことも可能。
```html
<style>
  .svgbg {
    background: url(sample/sample01.svg);
    width:430px; height:330px;
  }
</style>
<img src="sample/image01.svg">
<div class="svgbg"></div>
```
- HTMLに直接埋め込む
svgタグをhtmlに埋め込んで使う。今回は、ここで使うことを想定して説明します。
```html
<!DOCTYPE html>
<html>
  <body>
    <svg xmlns="http://www.w3.org/2000/svg"
         xmlns:xlink="http://www.w3.org/1999/xlink">
      <circle cx="100" cy="100" r="100" fill="red" />
      <rect x="130" y="130" width="300" height="200" fill="blue" />
    </svg>
  </body>
</html>
```

## 要素

#### width, height, viewbox
サイズの指定には以上の三つを使います。width, heightは通常の画像と同様の扱いです。

viewboxに関しては、svgタグの中で仮想の２次元空間を持っており、その中のどの部分を使うかを定義するものです。
`x y w h`というように、始点のx、y座標と幅、高さをスペース区切りで指定することによって、その空間から領域を切り出して画像のように扱うことができるのです。

```html
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink"
     viewBox="5 5 10 10" width="100" height="100">
  <circle cx="10" cy="10" r="5" fill="#c44" />
</svg>
```

課題: preserveAspectRatio

### 基本図形

扱える図形は以下の９つです。

- 四角形（rectタグ）
- 円（circleタグ）
- 楕円（ellipseタグ）
- 直線（lineタグ）
- 折れ線（polylineタグ）
- 多角形（polygonタグ）
- パス（pathタグ）
- 画像（imageタグ）
- 文字列（textタグ）

```html
<rect x="0" y="0" width="100" height="70" fill="#c44" />
<rect x="40" y="20" width="100" height="70" rx="10" ry="10"
      fill="#44c" />

<circle cx="190" cy="40" r="40" fill="#c44" />
<ellipse cx="240" cy="60" rx="50" ry="30" fill="#44c" />

<line x1="0" y1="140" x2="90" y2="140" stroke="#c44" />
<line x1="0" y1="100" x2="90" y2="180" stroke="#44c" />

<polyline points="100,140 120,100 160,100 180,140 160,180 120,180"
          stroke="#c44" fill="none" />
<polygon points="200,140 220,100 260,100 280,140 260,180 220,180"
         stroke="#44c" fill="none" />
```

## line
直線を描画します。x1,y1で始点を、x2,y2で終点を定義します。
```html
<svg viewBox="0 0 200 200">
    <line x1="10" y1="10" x2="190" y2="190" stroke="black" stroke-width="1"/>
</svg>
```

## rect
四角形を描画します。x,yで始点を、width,heightで大きさを、rx,ryで角丸を定義することができる。

```html
<svg viewBox="0 0 200 200">
    <rect x="10" y="10" width="120" height="100" stroke="black" stroke-width="1" fill="none"/>
    <rect x="50" y="50" rx="15" ry="10" width="120" height="100" stroke="black" stroke-width="1" fill="none"/>
</svg>
```

## circle,
円を描画します。中心座標をcx,cyで定義をして、半径をrで定義する。(centerX, centerY, Radius)
```html
<svg viewBox="0 0 200 200">
    <circle cx="70" cy="60" r="50" stroke="black" stroke-width="1" fill="yellow"/>
</svg>
```

## ellipse
楕円を描画します。中心座標をcx,cyで定義をして、半径をrx,ryで定義する。(CenterX, CenterY, RadiusX, RadiusY)

```html
<svg viewBox="0 0 200 200">
    <ellipse cx="120" cy="140" rx="50" ry="30" stroke="black" stroke-width="1" fill="yellow"/>
</svg>
```
## polyline
折れ線を描画します。points属性に頂点のリストを定義します。
```html
<svg viewBox="0 0 200 200">
    <polyline points="50,50 150,50 150,150, 50,150 125,75 150,150" stroke="black" stroke-width="1" fill="lightgreen"/>
</svg>
```

## polygon
多角形を描画します。同様にpoints属性に頂点のリストを定義します。polyline要素との違いはpathが閉じられるか否かです。
```html
<svg viewBox="0 0 200 200">
    <polygon points="50,50 150,50 150,150, 50,150 125,75 150,150" stroke="black" stroke-width="1" fill="lightgreen"/>
</svg>
```

## path

任意の線を描くための要素です。d属性に、描画する内容をコマンドを使い定義する。
コマンドは以下となります。大文字は絶対位置指定，小文字は現在位置からの相対位置指定に対応します。

- M.m 初期位置,位置のスキップ
- L,l 直線を引く
- H,h 水平線を引く
- V,v 垂直線を引く
- C,c,S,s,Q,q,T,t 曲線を引く．端点と制御点とで曲線を指定する．
- A,a 円弧を引く
- Z,z 直近のM位置まで直線を引いて線を閉じる.なお，座標を重ねただけでは線が閉じられた事にはならない． 
- B,b x軸の方向を設定する(他のコマンドによる描画に影響を及ぼす)† 
- R,r Catmull-Romスプライン曲線を引く†

```html
<svg viewBox="0 0 200 200">
    <path stroke="black" stroke-width="1" fill="none" d="
            M 0 200 
            L 25 50 
            l 50 -25 
            V 175 
            H 100 
            v -50 
            h 25 
            m 25 0 
            l 50 0 
            L 175 175 
            z
    "/>
</svg>
```

## styling

stroke, strike-width, fillに色やグラデーション、パターンを指定することで行います。

```html
<svg viewBox="0 0 200 200">
    <path d="M 20 20 h 160" stroke="black" stroke-width="1"/>
    <path d="M 20 40 h 160" stroke="red" stroke-width="3"/>
    <path d="M 20 60 h 160" stroke="green" stroke-width="5"/>
    <path d="M 20 80 h 160" stroke="blue" stroke-width="7"/>
    <!--pathを閉じていない例．左上の描画に注目-->
    <path d="M 20 100 h 160 v 30 h -160 v -30" stroke="aqua" stroke-width="10" fill="none"/>
    <!--pathを閉じた例．左上の描画に注目-->
    <path d="M 20 150 h 160 v 30 h -160 z" stroke="aqua" stroke-width="10" fill="blue"/>
    <path d="M 20 100 h 160 v 30 h -160 v -30" stroke="black" stroke-width="1" fill="none"/>
    <path d="M 20 150 h 160 v 30 h -160 z" stroke="black" stroke-width="1" fill="none"/>
</svg>
```

## 直線の描画
path要素では直線を引くコマンドとしてLコマンド，Hコマンド，Vコマンドの3つが提供されています。
Lコマンドは現在位置から任意の座標に，Hコマンドは現在の位置から水平方向（x軸方向）に，Vコマンドは垂直方向（y軸方向）に線を引く場合に用います。

```html
<svg viewBox="0 0 200 200">
    <g stroke="black" stroke-width="5">
        //絶対座標指定
        <path d="M 20 20 L 80 80"/>
        <path d="M 20 20 H 80"/>
        <path d="M 20 20 V 80"/>
        //相対座標指定
        <path d="M 120 120 l 60 60"/>
        <path d="M 120 120 h 60"/>
        <path d="M 120 120 v 60"/>
    </g>
</svg>
```

## 2次ベジェ曲線
2次ベジェ曲線を引くにはQコマンド及びTコマンドを用いる．Qコマンドでは制御点と次頂点の2つを指定する．
Tコマンドを用いて制御点の指定を省略することが出来る．前回用いた制御点から点対称の位置を新たな制御点として利用することで，曲線を滑らかにつなぐ
```html
<svg viewBox="0 0 200 200">
    <path d="M 0 0 Q 10 90 100 100 Q 110 10 200 0" stroke="black" stroke-width="1" fill="purple"/>
    <path d="M 0 0 L 10 90 L 100 100" stroke="black" stroke-width="2" fill="none"/>
    <path d="M 100 100 L 110 10 L 200 0" stroke="black" stroke-width="2" fill="none"/>
    <path d="M 0 200 Q 50 110 100 150 T 200 200" stroke="black" stroke-width="1" fill="yellow"/>
    <path d="M 0 200 L 50 110 L 100 150" stroke="black" stroke-width="2" fill="none"/>
    <path d="M 100 150 L 150 190 L 200 200" stroke="blue" stroke-width="2" fill="none" stroke-dasharray="2"/>
</svg>
```
課題:3次ベジェ曲線

## 円弧

円弧を引くにはA操作を行う．一般に始点と終点を通る半径が一定の円弧を考えると，長くて時計回り短くて時計回り長くて反時計回り短くて反時計回りの4曲線が得られる．従って，始点から終点に円弧を書く際にどのような円弧を引くかについてlarge-arc-flagでは円弧の長さの選択し，sweep-flagでは円弧の方向を選択する．
構文は「A rx,ry x-axis-rotation large-arc-flag,sweep-flag x,y」と非常に長いので面倒ではある．

```html
<svg viewBox="0 0 200 200">
    <g fill="none" stroke-width="15">
        <path d="M 75,125 A 60,40 15 0,1 125,75" stroke="blue"/>
        <path d="M 75,125 A 60,40 15 0,0 125,75" stroke="red"/>
        <path d="M 75,125 A 60,40 15 1,1 125,75" stroke="yellow"/>
        <path d="M 75,125 A 60,40 15 1,0 125,75" stroke="green"/>
    </g>
</svg>
```

課題:fill-rule

## stroke-dasharray

stroke-dasharrayを設定すると点線になる．描画部と非描画部の長さをリスト形式で記述し，点線のパターンを定義する．stroke-dashoffset属性は点線の開始位置を指定する。
これを利用してJSで描くようなアニメーションを作成している。

```html
<svg viewBox="0 0 200 200">
    <line x1="20" y1="25" x2="180" y2="25" stroke="red" stroke-width="5" stroke-dasharray="10"/>
    <line x1="20" y1="40" x2="180" y2="40" stroke="green" stroke-width="5" stroke-dasharray="20,10"/>
    <path d="M20,80Q50,30,100,70T180,60" stroke="gray" stroke-width="5" stroke-dasharray="10" fill="none"/>
    <circle cx="50" cy="140" r="40" stroke="blue" stroke-width="5" stroke-dasharray="10" fill="none"/>
    <rect x="100" y="100" width="80" height="80" stroke="orange" stroke-width="5" stroke-dasharray="10" fill="none"/>
</svg>
```

```html
<svg viewBox="0 0 200 200">
    <g stroke="red" stroke-width="18">
        <path d="M10,20h180" stroke-dasharray="20"/>
        <path d="M10,40h180" stroke-dasharray="20,30"/>
        <path d="M10,60h180" stroke-dasharray="20,30,10"/>
    </g>
    <g stroke="green" stroke-width="18" stroke-dashoffset="40" transform="translate(0,60)">
        <path d="M10,20h180" stroke-dasharray="20"/>
        <path d="M10,40h180" stroke-dasharray="20,30"/>
        <path d="M10,60h180" stroke-dasharray="20,30,10"/>
    </g>
    <g stroke="blue" stroke-width="18" stroke-dashoffset="-40" transform="translate(0,120)">
        <path d="M10,20h180" stroke-dasharray="20"/>
        <path d="M10,40h180" stroke-dasharray="20,30"/>
        <path d="M10,60h180" stroke-dasharray="20,30,10"/>
    </g>
</svg>

課題　:stroke-linecap, stroke-linejoin, stroke-miterlimit, path.getTotalLength()
                                   
## グループ化

gタグによってグループ化することができる。
```html
<svg viewBox="0 0 200 200">
    <g stroke="black" stroke-width="1" fill="yellow" transform="scale(1,0.4)">
        <circle cx="120" cy="80" r="50"/>
        <ellipse cx="70" cy="80" rx="50" ry="30"/>
    </g>
    <g stroke="black" stroke-width="1" fill="orange" opacity="0.5">
        <circle cx="120" cy="80" r="50"/>
        <ellipse cx="70" cy="80" rx="50" ry="30"/>
    </g>
</svg>
```

## 図形の変形

省略((*´∀｀*))
http://defghi1977.html.xdomain.jp/tech/svgMemo/svgMemo_04.htm

## defs, use
defs要素は各種定義情報（テンプレートとなる図形やグラデーション等）を格納する。プログラムにおけるサブルーチンを記述する場所に相当する。
この要素内部の図形はレンダリング対象とはならず。use要素等により外部から参照されることで初めて意味をもつ。
use要素はxlink:href属性に設定した要素の内容を元に新しい図形を描画することを示す。位置，サイズについてはrect要素と同様に指定する。
```html
<svg viewBox="0 0 200 200">
    <defs>
        <circle id="c" r="50"/>
    </defs>
    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#c" x="20" y="10" fill="purple"/>
    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#c" x="100" y="100" stroke="gold" stroke-width="10" fill="red"/>
</svg>
```

```html
<svg viewBox="0 0 200 200">
    <g id="part">
        <path d="M 100,100 Q 50,50 80,10 L 100,20 L 120,10 Q 150,50 100,100 z" fill="pink"/>
        <path d="M 100,95 v -40" stroke="white" stroke-width="5" stroke-linecap="round"/>
    </g>
    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#part" transform="rotate(72,100,100)"/>
    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#part" transform="rotate(144,100,100)"/>
    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#part" transform="rotate(-144,100,100)"/>
    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#part" transform="rotate(-72,100,100)"/>
</svg>
```

## image

htmlのimg要素と同様にimage要素を用いて外部のpng，jpg，svg等の画像ファイルを取り込むことが出来る．外部ファイルの参照の他，dataスキームによる画像データの埋め込みに対応しているなど役割的に非常に似通った要素であるが，細かい部分で動作が異なる．
image要素においてはpreserveAspectRatio属性を用いて，画像の引き伸ばし方法を指定することが出来る．この機能は使い方次第でグラフィックの左詰め・右詰めと言った処理に応用できる．以下抜粋。

- none 範囲いっぱいに引き伸ばす
- meet 比率を保ちつつ，画像を内部にフィットさせる．image要素の領域に隙間が開く．規定値
- slice 比率を保ちつつ，領域いっぱいに広げる．画像が見切れる．

```html
<svg id="image" viewBox="0 0 200 200">
    <image xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="img.png" width="200" height="100"/>
    <image xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="img.png" preserveAspectRatio="none" y="100" width="200" height="100"/>
</svg>
```

## text

text要素はグラフィック内のテキストを表す．文字の描画は他の図形と同じくfill，stroke属性にて行う．描画位置の指定はx属性y属性で行う．テキストの描画は文字の下部を基準に行われるため，y座標の指定を適切に行わないと文字が描画されないことがある．また改行文字やタブ文字等は空白文字として扱われるため，改行を表現するには描画位置を指定し直す必要がある．

```html
<svg viewBox="0 0 200 200">
    <g font-family="sans-serif" fill="black" font-size="20">
        <text x="0" y="20">本日は晴天なり</text>
        <text x="0,60" y="50">本日は晴天なり</text>
        <text x="0,30,60,90,120,150,180" y="80">本日は晴天なり</text>
        <text x="0,30,60,90,120,150,180" y="110,112,114,116,118,120,122">本日は晴天なり</text>
    </g>
</svg>
```

```html
<svg viewBox="0 0 200 200">
    <g font-family="sans-serif" fill="black" font-size="20">
        <text x="0" y="20">本日は晴天なり</text>
        <text x="0" y="50" dx="10">本日は晴天なり</text>
        <text x="0" y="80" dx="10,10,10,10,10,10,10">本日は晴天なり</text>
        <text x="0" y="110" dy="0,4,8,12,16,20,24">本日は晴天なり</text>
    </g>
</svg>
```
### text-anchor
text要素に設定した座標を基準にテキストが描画されるが，テキスト列のどこを基準に文字列を配置するかをtext-anchor属性で指定する．
```html
<svg viewBox="0 0 200 200">
    <line x1="100" y1="0" x2="100" y2="200" stroke="black"/>
    <g font-family="sans-serif" fill="blue" font-size="20">
        <text x="100" y="40" text-anchor="start">
            本日は晴天なり
        </text>
        <text x="100" y="100" text-anchor="middle">
            本日は晴天なり
        </text>
        <text x="100" y="160" text-anchor="end">
            本日は晴天なり
        </text>
    </g>
</svg>
```

### textPath
textPath要素を用いると予め定義しておいた曲線の上に文字列を配置することが出来る．文字描画の基底線が曲線に接するように配置される．（chromeでは不具合が出るケースがある．）
テキストの幅はパス上の長さで計算されるため，文字間の間隔が広がって見えたり詰まって見えてしまうのは致し方ない．

```html
<svg viewBox="0 0 200 200">
    <defs>
        <marker id="tpm" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="strokeWidth" preserveAspectRatio="none" markerWidth="15" markerHeight="15" orient="auto">
            <polyline points="0,0 10,5 0,10" fill="none" stroke="black"/>
        </marker>
        <path id="tp" d="M 100,100 m 0,-50 a 50,50 0 1,1 -1,0 h 1" stroke="black" fill="none" marker-end="url(#tpm)"/>
    </defs>
    <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#tp"/>
    <text fill="purple" font-family="sans-serif" font-size="20">
        <textPath xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#tp" startOffset="10%">
            本日は晴天なり<tspan fill="red">本日</tspan>は晴天なり
        </textPath>
    </text>
</svg>
```
課題: marker


## グラデーション
グラデーションは塗り潰しに用いる色を徐々に変化させることで，単色によるものに比べて滑らかな見た目をもたらす．グラフィックを描く場合には必須な機能であり，SVGにおいても帯状にグラデーションを掛けるlinearGradient要素と放射状に掛けるradialGradient要素, 及び不定形グラデーションを定めるmeshGradient要素の3つが提供されている．

SVGによるグラデーションはCSS3におけるグラデーションのように特定のプロパティに埋め込む形ではなく，別途専用の要素で定義した上で，そのグラデーションを利用する図形から参照する形を取る．グラデーションにはid属性を設定しておき，図形要素においてそのid値をfill属性，stroke属性からurl関数を用いて参照することで，グラデーションが描画される．

```html
<svg viewBox="0 0 200 200">
    <defs>
        <linearGradient id="g0">
            <stop offset="0.2" stop-color="yellow"/>
            <stop offset="0.8" stop-color="pink"/>
        </linearGradient>
    </defs>
    <rect x="10" y="10" width="180" height="80" fill="url(#g0)"/>
    <rect x="10" y="110" width="180" height="80" stroke="url(#g0)" stroke-width="10" fill="none"/>
</svg>
```

```html
<svg veiwbox="0 0 200 200">
    <defs>
        <linearGradient id="g4" gradientUnits="userSpaceOnUse" x1="20" y1="100" x2="180" y2="100">
            <stop offset="0.2" stop-color="green"/>
            <stop offset="0.5" stop-color="yellow"/>
            <stop offset="0.5" stop-color="blue"/>
            <stop offset="0.8" stop-color="red"/>
        </linearGradient>
    </defs>
    <rect width="100%" height="100%" fill="url(#g4)"/>
    <text x="100" y="90" text-anchor="middle">勾配ベクトル</text>
    <path d="M20,100L180,100" stroke-width="3" stroke="black"/>
    <path d="M52,90v20" stroke="black"/>
    <path d="M100,90v20" stroke="black"/>
    <path d="M148,90v20" stroke="black"/>
</svg>
```
課題: linearGradient, radialGradient, gradientUnits, spreadMethod

## clip-path

clipPath要素は任意のクリップ領域を定義する．他の要素はこのclipPath要素をclip-path属性に設定することでstrokeやfillによる描画の範囲を制限することが出来る．clipPath要素の子に配置された図形要素の外形がクリップ領域として解釈される．image要素なども設定できるが，単なる矩形として扱われるため意味がない．
またg要素，use要素を用いることはできないので，どうしても必要であればmask要素を用いると良い

```html
<svg viewBox="0 0 200 200">
    <defs>
        <clipPath id="clip" clip-rule="evenodd">
            <rect x="25" y="25" width="50" height="50"/>
            <rect x="125" y="25" width="50" height="50"/>
            <text x="30" y="100">ここがクリップ領域</text>
            <path d="M 25,125 v 30 h 100 v -30 h -100 m 50,30 v 20 h 100 v -30 h -100"/>
        </clipPath>
    </defs>
    <rect x="0" y="0" width="200" height="200" fill="lightblue"/>
    <circle clip-path="url(#clip)" cx="100" cy="100" r="90" stroke="pink" stroke-width="10" fill="red"/>
</svg>
```
## mask

クリップと同様の機能を提供するものとしてマスクがある．mask要素を設定することでマスク画像を定義することが出来る．クリップが画像の繰り抜きであることに対し，マスクは画像の合成を行なって透かしのような効果を得る．マスク画像の輝度に応じた描画が為され，暗い部分ほど薄く（透明に）描画される．よってマスク画像の色味にはそれほど意味がない．なおマスク画像の色がwhiteである場合はクリップの場合と同じ結果となる．

```html
<svg viewBox="0 0 200 200">
    <mask id="mask" maskUnits="userSpaceOnUse" x="0" y="0" width="200" height="200">
        <g fill="white" fill-rule="evenodd">
            <rect x="25" y="25" width="50" height="50"/>
            <rect x="125" y="25" width="50" height="50"/>
            <text x="30" y="100">ここがマスク領域</text>
            <path d="M 25,125 v 30 h 100 v -30 h -100 m 50,30 v 20 h 100 v -30 h -100"/>
        </g>
    </mask>
    <rect x="0" y="0" width="200" height="200" fill="lightpink"/>
    <circle mask="url(#mask)" cx="100" cy="100" r="90" stroke="aqua" stroke-width="10" fill="blue"/>
</svg>
```

### appendix
拡張機能
- https://chrome.google.com/webstore/detail/web-maker/lkfkkhfhhdkiemehlpkgjeojomhpccnh
資料
- http://www.atmarkit.co.jp/ait/articles/1206/01/news143.html
- http://defghi1977.html.xdomain.jp/tech/svgMemo/svgMemo_01.htm
