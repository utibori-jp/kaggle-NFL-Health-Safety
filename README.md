
# kaggle-NFL-Health-Safety
NFL Health Safety -Helmet Assignment　[コンペ](https://www.kaggle.com/c/nfl-health-and-safety-helmet-assignment)
のリポジトリ

![](https://github.com/utibori-jp/kaggle-NFL-Health-Safety/blob/main/data/info/images/NFL_HELTH.png)

* result

- directory tree
```
kaggle-NFL-Health-Safety
├── README.md
├── data         <---- gitで管理するデータ
├── data_ignore  <---- .gitignoreに記述されているディレクトリ(モデルとか、特徴量とか、データセットとか)
├── nb           <---- jupyter lab で作業したノートブック
├── nb_download  <---- ダウンロードした公開されているkagglenb
└── src          <---- .ipynb 以外のコード
```

## Basics
### Overview(DeepL)

National Football League（NFL）とAmazon Web Services（AWS）が協力して、最高のスポーツ傷害監視・軽減プログラムを開発しています。これまでの大会では、Kaggleはヘルメットの衝撃を検出するのに役立ちました。次のステップとして、NFLは各ヘルメットに特定の選手を割り当てたいと考えています。そうすれば、フットボールのプレー中の各選手の「エクスポージャー」を正確に把握することができます。

![](https://storage.googleapis.com/kaggle-media/competitions/NFL%20player%20safety%20analytics/assingment_example.gif.gif)

現在、NFLは毎年、プレーのサブセットに手動で注釈を付け、各選手のエクスポージャーのサンプルを決定しています。このプログラムを拡大するには、現在の選手割り当てでは、選手の位置を決定するためにフィールドマップが必要です。NFLは、マッピングのステップを必要としない、このモデルの精度に合わせることに興味を持っています。NFLは、個々の選手を識別するより良い方法を発明するために、Kagglersに呼びかけています。

ナショナル・フットボール・リーグは、アメリカで最も人気のあるスポーツリーグです。1920年に設立されたNFLは、現代のスポーツリーグの成功モデルを開発し、スポーツによる怪我の診断、予防、治療の進歩に尽力しています。健康と安全への取り組みとしては、独自の医学研究や工学的進歩への支援のほか、医療プロトコルの強化やゲームの指導・プレー方法の改善など、選手をよりよく保護し、ゲームをより安全なものにするための活動に取り組んでいます。NFLの健康と安全に関する取り組みについての詳細は、www.NFL.com/PlayerHealthandSafety。

このコンペティションでは、ビデオ映像からアメフト選手のヘルメットを識別して割り当てます。特に、検出されたヘルメットの衝撃を、トラッキング情報を介して正しい選手に割り当てることができるアルゴリズムを作成していただきます。90％の精度を目標としています。

成功すれば、選手の安全性を効率的に向上させようとするNFLの取り組みをサポートすることができます。NFLが手作業で各露出をラベル付けする必要がなくなれば、ヘルメットの衝撃に関する複雑な研究課題に答えるスピードと規模が劇的に向上します。また、選手を自動検出することで、NFLは過去の被曝の傾向を逆算できるようになり、将来的に被曝を軽減する方法についてより深い洞察が得られるようになります。

### Data(DeepL)

この競技では、ゲームの映像から正しい選手を割り当てることが課題となります。各プレーにはサイドラインとエンドゾーンの2つの関連ビデオがあり，ビデオ間でフレームが一致するようにビデオが配置されています．トレーニングセットのビデオはtrain/にあり，対応するラベルはtrain_labels.csvにあり，予測する必要のあるビデオはtest/フォルダにあります．

ヘルメットの検出を補助するために，ラベル付きのバウンディングボックスを持つヘルメットの画像の補助データセットも提供されています．これらのファイルはimagesに、バウンディングボックスはimage_labels.csvにあります。

train_baseline_helmets.csvは、imagesフォルダの画像とラベルで学習されたベースラインヘルメット検出モデルの出力です。

train_player_tracking.csvは，提供されたプレイ中にフィールド上にいる各プレイヤーの10Hzのトラッキングデータです．

これはコードコンペです。投稿されたモデルは，テストセットに含まれる15の未見のプレーに対して再実行されます．公開されているテストビデオは、スコアリングには使用されない模擬プレー（トレーニングセットからコピーされたもの）のセットに過ぎません。

関連する test_baseline_helmets.csv と test_player_tracking.csv は、提出する際にモデルで利用できます。

注意：この大会で提供されるデータセットは，コンピュータビジョンモデルのトレーニングを目的として慎重に設計されているため，通常よりもはるかに高い確率でヘルメットの衝突が発生するプレーが含まれています．このデータセットは、フットボールの試合中のヘルメット衝撃率の代表的なサンプルではないため、その発生率について推論するために使用すべきではありません。

ファイル
[train/test] 各プレーのmp4動画。各プレーには、エンドゾーンから撮影したものとサイドラインから撮影したものの2つのコピーがあります。ビデオのペアは、時間的にはフレームごとに一致していますが、それぞれのビューで異なるプレーヤーが見えることがあります。あなたは、プレーヤーが実際に見えるビューについてのみ予測を行う必要があります。

train_labels.csv トレーニングセットのヘルメットトラッキングとコリジョンのラベルです。

video_frame: ラベルに関連するビデオとフレーム番号の組み合わせ。
gameKey: ゲームのIDコード。
playID: プレイのIDコード。
view: カメラの向き。
video：関連するビデオのファイル名です。
frame: このプレイのフレーム番号
注：サイドラインビューとエンドゾーンビューは、スナップがビデオの10フレーム後に発生するように時間同期されています。この時間調整は、±3フレームまたは0.05秒以内の精度で行われていると考えてください（ビデオデータは約59.94フレーム/秒で記録されています）。

ラベル：アソシエイトプレーヤーの番号を表示します。
[left/width/top/height]: 予測のバウンディングボックスの指定です。
impactType: ヘルメットの衝撃の種類の説明：ヘルメット、肩、体、地面など。
isDefinitiveImpact 確定的なインパクトの真/偽のインジケータ。確定的なインパクトのボックスは、スコアリングアルゴリズムで追加の重みを与えられます。
isSidelinePlayer ヘルメットボックスがプレーのサイドライン上にあるかどうかのTrue/Falseインジケータ。このフィールドが False である行のみがスコアリングに使用されます。
sample_submission.csv 有効なサンプル投稿ファイルです。

VIDEO_FRAME。ラベルに関連するビデオとフレーム番号の組み合わせです。
label: ヘルメットボックスの予測されるラベル割り当て。
[left/width/top/height]: 予測のバウンディングボックスの指定。
images/ ヘルメット検出器に使用する訓練/テストビデオのフレームに相当する補助的な写真を含みます。

image_labels.csv には，画像に対応するバウンディングボックスが格納されています．

image: 画像のファイル名．
label: ラベルの種類．
[left/width/top/height]: ラベルのバウンディングボックスの指定で，left=0，top=0が左上隅となります．
[train/test]_baseline_helmets.csvには、ヘルメットボックスの不完全なベースライン予測が格納されています。これらのファイルを作成するために使用されたモデルは、imagesフォルダにある追加画像に対してのみ学習されました。

video_frame: ラベルに関連付けられたビデオとフレーム番号の組み合わせです。
[left/width/top/height]: 予測値のバウンディングボックスの指定です。
conf バウンディングボックスにヘルメットが含まれているかどうかのベースラインモデルの確信度です。
[train/test]_player_tracking.csv 各プレイヤーはフィールド上で正確な位置を特定できるセンサーを装着しており，その情報はこれら2つのファイルで報告される．

gameKey: ゲームのIDコードです。
playID: プレーのIDコードです。
player：プレイヤーのIDコード
time：10Hzのタイムスタンプ
x：フィールドの長軸方向のプレーヤーの位置。下図参照
y：フィールドの短軸方向のプレーヤーの位置。下図参照
s：速度（ヤード／秒）。
a：加速度（ヤード／秒^2）。
dis：前の時点からの移動距離（単位：ヤード）。
o：選手の向き（deg）。
dir：選手の動きの角度（deg）。
event: スナップ、ホイッスルなどのゲームイベント
![](https://www.googleapis.com/download/storage/v1/b/kaggle-user-content/o/inbox%2F3258%2F820e86013d48faacf33b7a32a15e814c%2FIncreasing%20Dir%20and%20O.png?generation=1572285857588233&alt=media)

## Log
### 2021/10/19
* join!
* 今日kaggle日記を見よう見まねで書き始めた
* ひたすら他の人のコードの写経しかしてない。今してるのが終わらなかったが、最近ようやく終わりが見えてきた。
 ### 2021/10/20
 * 昨日まで読んでいたコードの写経が終わった
 * コンペで与えられたデータを全てgithubにインポートしようと思ったが、データサイズがあまりにも大きくインポートできなかったので諦めた。リンクだけでも貼っておいた方がいいとは思ったが、いったん放置。


## Reference
* [NFL Starter EDA](https://www.kaggle.com/illgamhoduck/nfl-starter-eda/notebook)
* [Tuning DeepSort + Helmet Mapping_high_score](https://www.kaggle.com/jianghanhan/tuning-deepsort-helmet-mapping-high-score)
* [Camera-Tracking Matching with Gradient Descent](https://www.kaggle.com/coldfir3/camera-tracking-matching-with-gradient-descent)

