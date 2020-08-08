# mixing_music_by_VAE

## データセットの作成
今回はmidiファイルの音源を行列に変換して扱った。
### 1.データの読み込み
今回用いた音源は斉藤和義の歌うたいのバラッド、倉木麻衣の渡月橋である。<br>
それぞれのmidiファイルをpypianorollによりテンポが120の行列に変換した。
### 2.データの標準化
音源データはpitch(音の高さ)が0 ~ 127, velocity(音の強弱)が0 ~ 127で与えられている。<br>
今回はそれぞれのピッチに対する音の強弱を学習させるためvelocityの全ての値を90で割り、扱いやすい大きさに変換した。
以下が今回用いたデータのヒートマップである。横軸がtime-step、縦軸がpitchである。
#### 歌うたいのバラッド
![togetsukyou](https://github.com/Jumpei-Fujita/mixing_music_by_VAE/blob/master/utautai.png)
#### 渡月橋
![togetsukyou](https://github.com/Jumpei-Fujita/mixing_music_by_VAE/blob/master/togetsukyou.png)

## モデル構築
今回は以下のようなVAEを構築した。
![model](https://github.com/Jumpei-Fujita/mixing_music_by_VAE/blob/master/VAE.png)<br>
このVAEを128個構築し、それぞれにピッチを割り当てて学習を進めた。以下がそのイメージである。
![model](https://github.com/Jumpei-Fujita/mixing_music_by_VAE/blob/master/multi-VAE.png)<br>

## 学習
VAEはEncoderによって平均と分散が出力され、それらに従う正規分布に従う乱数を生成する。その平均と分散を以下のような誤差関数により学習していく。
![loss](https://github.com/Jumpei-Fujita/mixing_music_by_VAE/blob/master/vaeloss.png)<br>
最適化手法はAdamを用いた。
以下は学習の様子である。<br>
![model](https://github.com/Jumpei-Fujita/mixing_music_by_VAE/blob/master/graph.png)

## 結果
歌うたいのバラッドの正規分布に従う乱数、渡月橋の正規分布に従う乱数をそれぞれ半分ずつ足した時の結果を以下に示す。
![model](https://github.com/Jumpei-Fujita/mixing_music_by_VAE/blob/master/heat50.png)<br>


## コードの実行手順
multi_VAE.ipynbを訓練部分まで順に実行していく。<br>
その後２曲をミックスさせた音楽を生成する場合はtest関数を実行する。<br>
ここで、test関数の引数となるalphaは２曲の割合であり、0 ~ 1である。<br>
alphaが高ければ高いほど渡月橋に近い曲になる。

