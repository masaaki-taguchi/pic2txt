# PIC2TXT.R
X680x0でPICファイルを8色、または16色のテキスト画像に変換します。

元画像   
[<img src="./images/original.png" width="300">](./images/original.png)   
変換後画像   
[<img src="./images/converted.png" width="300">](./images/converted.png)   
変換後画像(Floyd Steinberg法使用)   
[<img src="./images/converted_fs.png" width="300">](./images/converted_fs.png)   

## 使用方法
以下の形式で起動できます。
```
PIC2TXT -[switch] <pic-file-path>
```
スイッチ無しで、PICファイルの指定のみで起動した場合は、8色のテキスト画像に変換し、直接画面に変換結果を表示します。表示後はReturn、またはESCを押せば元画面に戻ります。

-Sスイッチにより、変換後のテキスト画像を任意のファイル名で保存可能です。-Lスイッチにより保存したテキスト画像ファイルを表示します。

-Fスイッチ、または-Gスイッチは、ディザリングアルゴリズムとして広く使用されているFloyd Steinberg法による変換となります。デフォルトの変換に比べると、かなりの時間を要します。10MHzのX68Kで1分50秒程度掛かります。デフォルトの変換の場合は25秒程度となります。

本プログラムでは以下のスイッチが指定可能です。
| &nbsp;&nbsp;&nbsp;&nbsp;switch&nbsp;&nbsp;&nbsp;&nbsp; | 説明 |
| ---- | ---- |
|  -L \<text-file-path>  |  指定したファイル名のテキスト画像を読み込みます。  |
|  -S \<text-file-path>  |  指定したファイル名でテキスト画像を書き込みます。  |
|  -F  |   Floyd Steinberg法により16色のテキスト画像に変換します。  |
|  -G  |   Floyd Steinberg法によりグレースケールのテキスト画像に変換します。  |

## ビルド方法
ビルドはyosshinさんの[xdev68k](https://github.com/yosshin4004/xdev68k)を使用させていただいています。
また、PICファイルのロードはAPICGLIB.aを利用しており、付属のMakefileはxdev68k/include/miscにapicglib.macを、xdev68k/lib/miscにAPICGLIB.aを配置する前提としています。

xdev68kの環境構築後に、bashコンソールで以下を実行してください。
```
cd src
make
```

## 謝辞
PICファイルのロード部分に、GORRY様の[APICG](http://retropc.net/x68000/software/graphics/pic/apicg/)を使用させていただいております。有難うございます。

また、本README.md上のサンプル画像は、出所がはっきりとしておりませんが、おそらくOh!X付属のものを使用させていただいております。有難うございます。

## ライセンス
PIC2TXTはMITライセンスを適用しています。
