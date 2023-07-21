## VC Client

[English](/README_en.md)

## What's New!

- v.1.5.3.10

  - New Feature
    - Support Diffusion SVC(only combo model)
    - System audio capture(only for win)
    - Support RMVPE
  - improvement
    - directml: set device id
  - some bugfixes:
    - noise suppression2
  - etc.

- v.1.5.3.9a

  - some improvements:
    - keep f0 detector setting
    - MMVC: max chunksize for onnx
    - etc
  - some bugfixs:
    - RVC: crepe fail to estimate f0
    - RVC: fallback from half-precision when half-precision failed.
    - etc

- v.1.5.3.9

  - New feature:
    - Add Crepe Full/Tiny (onnx)
  - some improvements:
    - server info includes python version
    - contentvec onnx support
    - etc
  - some bugfixs:
    - server device mode chuttering
    - new model add sample rate
    - etc

- v.1.5.3.8a

  - Bugfix(test): force client device samplerate
  - Bugfix: server device filter

- v.1.5.3.8

  - RVC: performance improvement ([PR](https://github.com/w-okada/voice-changer/pull/371) from [nadare881](https://github.com/nadare881))

- v.1.5.3.7

  - Feature:
    - server device monitor
  - Bugfix:
    - device output recorder button is showed in server device mode.

# VC Client とは

1. 各種音声変換 AI(VC, Voice Conversion)を用いてリアルタイム音声変換を行うためのクライアントソフトウェアです。サポートしている音声変換 AI は次のものになります。

- サポートする音声変換 AI （サポート VC）
  - [MMVC](https://github.com/isletennos/MMVC_Trainer)
  - [so-vits-svc](https://github.com/svc-develop-team/so-vits-svc)
  - [RVC(Retrieval-based-Voice-Conversion)](https://github.com/liujing04/Retrieval-based-Voice-Conversion-WebUI)
  - [DDSP-SVC](https://github.com/yxlllc/DDSP-SVC)

2. 本ソフトウェアは、ネットワークを介した利用も可能であり、ゲームなどの高負荷なアプリケーションと同時に使用する場合などに音声変換処理の負荷を外部にオフロードすることができます。

![image](https://user-images.githubusercontent.com/48346627/206640768-53f6052d-0a96-403b-a06c-6714a0b7471d.png)

3. 複数のプラットフォームに対応しています。

- Windows, Mac(M1), Linux, Google Colab (MMVC のみ)

# 使用方法

大きく 2 つの方法でご利用できます。難易度順に次の通りです。

- 事前ビルド済みの Binary での利用
- Docker や Anaconda など環境構築を行った上での利用

本ソフトウェアや MMVC になじみの薄い方は上から徐々に慣れていくとよいと思います。

## (1) 事前ビルド済みの Binary での利用

- 実行形式のバイナリをダウンロードして実行することができます。

- チュートリアルは[こちら](tutorials/tutorial_rvc_ja_latest.md)をご覧ください。

- Windows 版と Mac 版を提供しています。

  - Windows かつ Nvidia の GPU をご使用の方は、ONNX(cpu,cuda), PyTorch(cpu,cuda)をダウンロードしてください。
  - Windows かつ AMD/Intel の GPU をご使用の方は、ONNX(cpu,DirectML), PyTorch(cpu,cuda)をダウンロードしてください。AMD/Intel の GPU は onnx のモデルを使用する場合のみ有効になります。
  - いずれの GPU のサポート状況についても、PyTorch、Onnxruntime がサポートしている場合のみ有効になります。
  - Windows で GPU をご使用にならない方は、ONNX(cpu,cuda), PyTorch(cpu,cuda)をダウンロードしてください。

- Windows 版は、ダウンロードした zip ファイルを解凍して、`start_http.bat`を実行してください。

- Mac 版はダウンロードファイルを解凍したのちに、`startHttp.command`を実行してください。開発元を検証できない旨が示される場合は、再度コントロールキーを押してクリックして実行してください(or 右クリックから実行してください)。

- 初回起動時は各種データをダウンロードします。ダウンロードに時間がかかる可能性があります。ダウンロードが完了すると、ブラウザが立ち上がります。

- リモートから接続する場合は、`.bat`ファイル(win)、`.command`ファイル(mac)の http が https に置き換わっているものを使用してください。

- DDPS-SVC の encoder は hubert-soft のみ対応です。

- ダウンロードはこちらから。

| Version    | OS  | フレームワーク                        | link                                                                                                                                                            | サポート VC                                                               | サイズ |
| ---------- | --- | ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------ |
| v.1.5.3.10 | mac | ONNX(cpu), PyTorch(cpu,mps)           | [google](https://drive.google.com/uc?id=1useZ4gcI0la5OhPuvt2j94CbAhWikpV4&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, Diffusion-SVC           | 795MB  |
|            | win | ONNX(cpu,cuda), PyTorch(cpu,cuda)     | [google](https://drive.google.com/uc?id=13abR2xs4KmNIg9b5RJXFez9g6zwZqMj4&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC, Diffusion-SVC | 3237MB |
|            | win | ONNX(cpu,DirectML), PyTorch(cpu,cuda) | [google](https://drive.google.com/uc?id=1ZxPp-HF7vSEJ8m00WnQaGbo4bTN4LqYD&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC, Diffusion-SVC | 3122MB |
| v.1.5.3.9a | mac | ONNX(cpu), PyTorch(cpu,mps)           | [google](https://drive.google.com/uc?id=1GsPTUTUbMvwNwAA8SGvSplwsf-yui0iw&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC                          | 794MB  |
|            | win | ONNX(cpu,cuda), PyTorch(cpu,cuda)     | [google](https://drive.google.com/uc?id=1eKZCozh37QDfAr33ZG7lGFUOQv1tOooR&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3237MB |
|            | win | ONNX(cpu,DirectML), PyTorch(cpu,cuda) | [google](https://drive.google.com/uc?id=1sxUNBPkeSPPNOE1ZknVF-0kx2jHP3kN6&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3122MB |
| v.1.5.3.9  | mac | ONNX(cpu), PyTorch(cpu,mps)           | [google](https://drive.google.com/uc?id=1pTTcTseSdIfCyNUjB-K1mYPg9YocSYz6&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC                          | 795MB  |
|            | win | ONNX(cpu,cuda), PyTorch(cpu,cuda)     | [google](https://drive.google.com/uc?id=1KWg-QoF6XmLbkUav-fmxc7bdAcD3844V&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3238MB |
|            | win | ONNX(cpu,DirectML), PyTorch(cpu,cuda) | [google](https://drive.google.com/uc?id=1_TXUkDcofYz9mJd2L1ajAoyIBCQF29WL&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3123MB |
| v.1.5.3.8a | mac | ONNX(cpu), PyTorch(cpu,mps)           | [normal](https://drive.google.com/uc?id=1hg6lynE3wWJTNTParTa2qB2L06OL9KJ9&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC                          | 794MB  |
|            | win | ONNX(cpu,DirectML), PyTorch(cpu,cuda) | [normal](https://drive.google.com/uc?id=1C9PCu8pdafO6jJ2yCaB7x54Ls7LcM0Xc&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3122MB |
|            | win | ONNX(cpu,cuda), PyTorch(cpu,cuda)     | [normal](https://drive.google.com/uc?id=1bzrGhHPc9GdaRAMxkksTGtbuRLEeBx9i&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3237MB |
| v.1.5.3.8  | mac | ONNX(cpu), PyTorch(cpu,mps)           | [normal](https://drive.google.com/uc?id=1ptmjFCRDW7M0l80072JVRII5tJpF13__&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC                          | 794MB  |
|            | win | ONNX(cpu,DirectML), PyTorch(cpu,cuda) | [normal](https://drive.google.com/uc?id=19DfeACmpnzqCVH5bIoFunS2pGPABRuso&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3122MB |
|            | win | ONNX(cpu,cuda), PyTorch(cpu,cuda)     | [normal](https://drive.google.com/uc?id=1AYP_hMdoeacX0KiF31Vd3oEjxwdreSbM&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3237MB |
| v.1.5.3.7  | mac | ONNX(cpu), PyTorch(cpu,mps)           | [normal](https://drive.google.com/uc?id=1HdJwgo0__vR6pAkOkekejUZJ0lu2NfDs&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC                          | 794MB  |
|            | win | ONNX(cpu,cuda), PyTorch(cpu,cuda)     | [normal](https://drive.google.com/uc?id=1JIF4PvKg-8HNUv_fMaXSM3AeYa-F_c4z&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3237MB |
|            | win | ONNX(cpu,DirectML), PyTorch(cpu,cuda) | [normal](https://drive.google.com/uc?id=1cJzRHmD3vk6av0Dvwj3v9Ef5KUsQYhKv&export=download), [hugging face](https://huggingface.co/wok000/vcclient000/tree/main) | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC, DDSP-SVC                | 3122MB |

(\*1) Google Drive からダウンロードできない方は[hugging_face](https://huggingface.co/wok000/vcclient000/tree/main)からダウンロードしてみてください
(\*2) 開発者が AMD のグラフィックボードを持っていないので動作確認していません。onnxruntime-directml を同梱しただけのものです。
(\*3) 解凍や起動が遅い場合、ウィルス対策ソフトのチェックが走っている可能性があります。ファイルやフォルダを対象外にして実行してみてください。（自己責任です）

## (2) Docker や Anaconda など環境構築を行った上での利用

本リポジトリをクローンして利用します。Windows では WSL2 の環境構築が必須になります。また、WSL2 上で Docker もしくは Anaconda などの仮想環境の構築が必要となります。Mac では Anaconda などの Python の仮想環境の構築が必要となります。事前準備が必要となりますが、多くの環境においてこの方法が一番高速で動きます。**<font color="red"> GPU が無くてもそこそこ新しい CPU であれば十分動く可能性があります </font>（下記のリアルタイム性の節を参照）**。

[WSL2 と Docker のインストールの解説動画](https://youtu.be/POo_Cg0eFMU)

[WSL2 と Anaconda のインストールの解説動画](https://youtu.be/fba9Zhsukqw)

Docker での実行は、[Docker を使用する](docker_vcclient/README.md)を参考にサーバを起動してください。

Anaconda の仮想環境上での実行は、[サーバ開発者向けのページ](README_dev_ja.md)を参考にサーバを起動してください。

# トラブルシュート

- [通信編](tutorials/trouble_shoot_communication_ja.md)

# リアルタイム性（MMVC）

GPU を使用するとほとんどタイムラグなく変換可能です。

https://twitter.com/DannadoriYellow/status/1613483372579545088?s=20&t=7CLD79h1F3dfKiTb7M8RUQ

CPU でも最近のであればそれなりの速度で変換可能。

https://twitter.com/DannadoriYellow/status/1613553862773997569?s=20&t=7CLD79h1F3dfKiTb7M8RUQ

古い CPU( i7-4770)だと、1000msec くらいかかってしまう。

# 開発者の署名について

本ソフトウェアは開発元の署名しておりません。下記のように警告が出ますが、コントロールキーを押しながらアイコンをクリックすると実行できるようになります。これは Apple のセキュリティポリシーによるものです。実行は自己責任となります。

![image](https://user-images.githubusercontent.com/48346627/212567711-c4a8d599-e24c-4fa3-8145-a5df7211f023.png)

# Acknowledgments

- [立ちずんだもん素材](https://seiga.nicovideo.jp/seiga/im10792934)
- [いらすとや](https://www.irasutoya.com/)
- [つくよみちゃん](https://tyc.rei-yumesaki.net/)

```
  本ソフトウェアの音声合成には、フリー素材キャラクター「つくよみちゃん」が無料公開している音声データを使用しています。
  ■つくよみちゃんコーパス（CV.夢前黎）
  https://tyc.rei-yumesaki.net/material/corpus/
  © Rei Yumesaki
```

- [あみたろの声素材工房](https://amitaro.net/)
- [れぷりかどーる](https://kikyohiroto1227.wixsite.com/kikoto-utau)

# 利用規約

- リアルタイムボイスチェンジャーつくよみちゃんについては、つくよみちゃんコーパスの利用規約に準じ、次の目的で変換後の音声を使用することを禁止します。

```

■人を批判・攻撃すること。（「批判・攻撃」の定義は、つくよみちゃんキャラクターライセンスに準じます）

■特定の政治的立場・宗教・思想への賛同または反対を呼びかけること。

■刺激の強い表現をゾーニングなしで公開すること。

■他者に対して二次利用（素材としての利用）を許可する形で公開すること。
※鑑賞用の作品として配布・販売していただくことは問題ございません。
```

- リアルタイムボイスチェンジャーあみたろについては、あみたろの声素材工房様の次の利用規約に準じます。詳細は[こちら](https://amitaro.net/voice/faq/#index_id6)です。

```
あみたろの声素材やコーパス読み上げ音声を使って音声モデルを作ったり、ボイスチェンジャーや声質変換などを使用して、自分の声をあみたろの声に変換して使うのもOKです。

ただしその場合は絶対に、あみたろ（もしくは小春音アミ）の声に声質変換していることを明記し、あみたろ（および小春音アミ）が話しているわけではないことが誰でもわかるようにしてください。
また、あみたろの声で話す内容は声素材の利用規約の範囲内のみとし、センシティブな発言などはしないでください。
```

- リアルタイムボイスチェンジャー黄琴まひろについては、れぷりかどーるの利用規約に準じます。詳細は[こちら](https://kikyohiroto1227.wixsite.com/kikoto-utau/ter%EF%BD%8Ds-of-service)です。

# 免責事項

本ソフトウェアの使用または使用不能により生じたいかなる直接損害・間接損害・波及的損害・結果的損害 または特別損害についても、一切責任を負いません。

# (1) レコーダー（トレーニング用音声録音アプリ）

MMVC トレーニング用の音声を簡単に録音できるアプリです。
Github Pages 上で実行できるため、ブラウザのみあれば様々なプラットフォームからご利用可能です。
録音したデータは、ブラウザ上に保存されます。外部に漏れることはありません。

[録音アプリ on Github Pages](https://w-okada.github.io/voice-changer/)

[解説動画](https://youtu.be/s_GirFEGvaA)

# 過去バージョン

| Version    | OS  | フレームワーク                    | link                                                                                           | サポート VC                                                                   | サイズ |
| ---------- | --- | --------------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------ |
| v.1.5.2.9e | mac | ONNX(cpu), PyTorch(cpu,mps)       | [normal](https://drive.google.com/uc?id=1W0d7I7619PcO7kjb1SPXp6MmH5Unvd78&export=download) \*1 | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC                              | 796MB  |
|            | win | ONNX(cpu,cuda), PyTorch(cpu,cuda) | [normal](https://drive.google.com/uc?id=1tmTMJRRggS2Sb4goU-eHlRvUBR88RZDl&export=download) \*1 | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, so-vits-svc 4.0v2, RVC, DDSP-SVC | 2872MB |
| v.1.5.3.1  | mac | ONNX(cpu), PyTorch(cpu,mps)       | [normal](https://drive.google.com/uc?id=1oswF72q_cQQeXhIn6W275qLnoBAmcrR_&export=download) \*1 | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, RVC                              | 796MB  |
|            | win | ONNX(cpu,cuda), PyTorch(cpu,cuda) | [normal](https://drive.google.com/uc?id=1AWjDhW4w2Uljp1-9P8YUJBZsIlnhkJX2&export=download) \*1 | MMVC v.1.5.x, MMVC v.1.3.x, so-vits-svc 4.0, so-vits-svc 4.0v2, RVC, DDSP-SVC | 2872MB |
