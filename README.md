# 素のHTMLでのOcticonsの使い方

- Octicons 6.0.1

昔はフォントとして提供されていた[Octicons](https://octicons.github.com/)ですが、今ではSVGでしか提供されていません。
[README](https://github.com/primer/octicons)ではNode.jsやRubyでの使い方は説明されているのものの、素のHTMLでの利用方法が分かりません。
と思ったら、単にSVGタグをそのまま使うだけでした。
MITライセンスなので商用でも問題ありません。

## デモページ
https://sutara79.github.io/demo-octicons/

## 利用手順
### 1.使いたいアイコンを探す
[Octiconsの公式サイト](https://octicons.github.com/)で使いたいアイコンを探し、個別のページへ移動します。
今回は[.octicon-alert](https://octicons.github.com/icon/alert/)を選びました。
![001.png](https://qiita-image-store.s3.amazonaws.com/0/27816/64c9fc0b-ca8e-3740-52e9-963968a0392c.png)

### 2.SVGタグを丸ごとコピーする
HTMLのソースコードを直接見て、`<svg>`を丸ごとコピーします。
Chromeのデベロッパーツールで、`<svg>`のソースコードの上で`Copy Element`を使うと簡単にコピーできます。
![002.png](https://qiita-image-store.s3.amazonaws.com/0/27816/2104255a-4b39-2544-516d-f384827876ec.png)

### 3.width, height属性を削除する
アイコンの大きさはCSSで一括指定したほうが管理しやすいので、svg要素のwidth, height属性は削除します。

&lt;svg <strong>width="256"</strong> <strong>height="256"</strong> class="octicon octicon-alert" viewBox="0 0 16 16" version="1.1" aria-hidden="true"&gt;&lt;path fill-rule="evenodd" d="M8.865 1.52c-.18-.31-.51-.5-.87-.5s-.69.19-.87.5L.275 13.5c-.18.31-.18.69 0 1 .19.31.52.5.87.5h13.7c.36 0 .69-.19.86-.5.17-.31.18-.69.01-1L8.865 1.52zM8.995 13h-2v-2h2v2zm0-3h-2V6h2v4z"&gt;&lt;/path&gt;&lt;/svg&gt;

### 4.見栄えはCSSで調整する
公式のCSSを参考にします。
:link: https://github.com/primer/octicons/blob/master/lib/octicons_node/index.scss

私は下記のようにしました。

```css
.octicon {
  display: inline-block; /* 公式と同じ */
  fill: currentColor;    /* 公式と同じ */
  vertical-align: text-bottom;
  height: 1.4em; /* 大きさは height で指定 */
}
```

アイコンの色は`color`で指定します。
指定しなければ親要素の色を引き継ぎます。これは`fill: currentColor`のおかげです。

```css
.octicon-alert {
  color: red;
}
```

### 5.JavaScriptで動的にスタイル変更
マウスオーバー時に色を変えることもできます。

```javascript
document.querySelectorAll('.octicon-zap').forEach(function(elem, idx) {
  elem.onmouseover = function() {
    this.style.color = 'gold'; // マウスオーバーで金色に
  };
  elem.onmouseout = function() {
    this.style.color = ''; // 元の色に戻す
  }
});
```

## SVGをHTMLで表示する最適な方法は?
SVGファイルをIMGタグで表示することもできます。
ただ、これではCSSやJavaScriptで色を変えることができません。
現状ではSVGタグを使うのが一番だと思います。
複雑な図だとSVGのソースコードも長くなって扱いづらくなるのが難点ですが…。
ただ、フォントファイルをダウンロードしたりCDNで呼び出す必要がなく、使うアイコンのSVGタグだけを記述すればいいので、その点は良いと思います。

:link: [HTML5でのSVGファイル操作のおさらい - Qiita](https://qiita.com/ka215/items/f9834dca40bb3d7e9c8b)
