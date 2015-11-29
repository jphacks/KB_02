# オモビュー
<img src="https://github.com/jphacks/KB_02/raw/master/source/movie.gif" width="640">

※青枠が平常時, 赤枠が笑顔を検知したことを示します

## 製品概要
その場の"フンイキ"を可視化する, 拡張現実アプリです. 実装先として, googleglass, smart eyeglassなどの眼鏡型デバイス, あるいはロボットの視覚を想定しています. 

フンイキの可視化として, 私たちは"エモーションの遷移"に着目しました. エモーションの遷移とは, 表情や発話から会話によって時々刻々と変化するエモーションを力学的モデルで表現し, その動きを可視化したものです. 

この技術を用いることで, 

・その場の盛り上がり感を共有

・自分の発話に対してエモーションの変化が大きい人物の特定

・会話に参加している各自の盛り上がりへの寄与度を可視化

などが可能になります.

### 背景(製品開発のきっかけ、課題等）
まず, 開発における課題設定と課題解決のストーリーを説明します.

人間は日々, 他者とのコミュニケーションを通じて, 他者が自分に抱く好意を推定したり, 他者への好意を持ったり, 自己肯定感を得たりなど, 様々な心情の変化が起こります. コミュニケーションこそが他者との接点であり, 社会的な存在である私達人間にとって最も重要な行為と言っても過言ではないと考えます. 

しかし, 私たちはコミュニケーションの中で生まれる他者の感情の変化をすぐに忘れてしまい, 記録しておくことが出来ません. 例えば, 自分の発話に対して好意的な人・そうでない人は, その人が自分の発話に対してとったリアクションを全て覚えていれば判別することが可能ですが, 人間の記憶力ではそんなことはできません. 

そこで, 私達が見ている風景から「場の盛り上がり感」と「発話に対する各自のエモーション変化」を独自の力学モデルからリアルタイムで算出し, 表示するアプリケーションを開発しました. これによって, 長い時系列のエモーション変化が完全に記録することができ, 人と人との関係性を統計的かつ定量的に評価することが可能になります. 

まずは人と人とのコミュニケーションという状況を想定しますが, ロボットが周囲の人間の関係性を学習するためにも使うことができると思っていて, ゆくゆくはロボットが「空気を読む」ことができればと考えています.

また, このプロダクトを作った背景として, 近年の機械学習関連技術の発展により, これまで解かれていなかったマルチモーダルな複合タスクへのアプローチが盛んに行われるようになり, イノベーションが生まれやすい環境が醸成されていた, ということがあると思っています. 私たちのチームは機械学習の研究室の先輩後輩で構成されていますが, 研究で使っている技術などをアプリケーションレベルで活かしていけるような風潮があると感じていて, 研究者がこういったプロダクトを発表しやすくなってきているのだと思います.

### 製品説明（具体的な製品の説明）
### 特長
サーバー通信によって複数デバイスに対応することを念頭に置き, サーバー上で全てのプログラムを保持しています.

####1. 特長1
コミュニケーションにおける人毎のエモーション変化を記録していること

####2. 特長2
画像と音声を用いた感情分析の力学モデルを提案していること

####3. 特長3
場の盛り上がり感を共有できる

### 解決出来ること
背景に書いたことと重なりますが, コミュニケーション時のフンイキを可視化・保持することによって人と人との関係性を統計的かつ定量的に推定することができるようになります. これによって解決できる実問題として, 例えばロボットが周囲の人間関係を学習することができるようになることなどが挙げられます. さらに, 可視化されたフンイキの共有による新たなコミュニケーションの形が生まれることも期待しています.

また, このプロダクトの主要な点として, 人と人との関係性を推定するための新たな指標を提案していることがあり, このような指標を必要とする他のプロダクトにはいくらでも応用が効くと考えています.

### 今後の展望
商用利用に向けて, コミュニケーションの活発さを可視化するだけにとどまらず, 活発なコミュニケーションを促す工夫を考えたいと思います.

また, 技術的には, 音声認識と顔認識, 話者識別が動いている点を活かして, 自動翻訳を行った結果を顔枠の下に表示させることによって外国語話者とのコミュニケーションを可能にし, アプリの適用範囲を広げたいと考えています. 

さらに, これは完全にjust ideaですが, 表情(特に口部分)と音声を正準相関分析することによって相関の高さをランク付けし, 話者が複数人いる場合に発話内容を発話者に割り当て, 各自がポジティブな発言をしているかネガティブな発言をしているかであったり, 社会的な立場の上下関係を推測するなどしてみたいと考えています.

### 注力したこと（こだわり等）
* 顔の追跡
* 笑顔検出
* マルチスレッド化

## 開発技術
### 活用した技術
画像認識, 音声認識, 物体追跡, マルチスレッド化の技術を用いました. 

画像認識は, 顔認識と笑顔検出の二つの部分から成り立っており, 顔認識はあらゆるサイズの顔を想定してキャプチャしhaar-like特徴量を用いてAdaBoostで顔判定しており, cascadeフィルタを用いて非顔領域を高速排除する仕組みをOpenCVから呼び出しています. さらに, 抽出した顔領域の中から笑顔口を検出する仕組みも合わせて笑顔検出を行っています.

音声認識は, 音響モデルをGMM-HMM, 言語モデルを単語N-gramとして, 音声と単語を関連付ける(読みを定義する)辞書を用意して行う実装をjuliusから呼び出しました.

物体追跡は, 直前の顔フレームに対する現在の最近傍顔フレームに同じラベルを割り振ることで行い, さらに顔認識の失敗によるトラッキングの途切れや新しい顔枠が発生した時の新規ラベル割り当てにも対応できるように実装しました.

また, 動画認識(顔認識)と言語認識を同時に行う必要が有るためにマルチスレッド化を行いました。マルチスレッド化することにより両方の認識の更新を同時に行うことができプログラムのソースコードとしてもわかりやすい形に落としこむことができました。

#### API・データ
* 形態素解析API(本番環境では未反映)

#### フレームワーク・ライブラリ・モジュール
* OpenCV
* julius

#### デバイス
* 使用なし

### 独自技術
#### ハッカソンで開発した独自機能・技術
* 動画をキャプチャーや音声認識やグラフ描画などの同時処理。特に、Python上では実行できないモジュールとの連携。
* 顔画像の時系列トラッキング

#### 製品に取り入れた研究内容（データ・ソフトウェアなど）（※アカデミック部門の場合のみ提出必須）
* 音声と画像のマルチモーダル処理
* 表情と発話内容からエモーションを推定する力学的なモデル
