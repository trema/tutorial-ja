<!SLIDE title-slide>
# 作ってわかる OpenFlow ########################################################
## Trema チュートリアル＆ハンズオン 

### 鈴木 一哉
### 高宮 安仁


<!SLIDE small>
# 本日のゴール #################################################################

## "トラフィックモニターつき L2 スイッチ" の実装

* Tutorial Kit: <https://github.com/trema/tutorial.files>
* "Hello World" から始め、5 つの段階を踏んで開発を行います
* Trema を使い OpenFlow コントローラの開発サイクルを体験します


<!SLIDE small incremental>
# Trema とは ################################################################

* Ruby と C 向けの OpenFlow コントローラ開発フレームワーク
  * GPL2
  * <http://github.com/trema/trema>
* "Post-Rails" 高い生産性を実現
  * <i>書いたコードをすぐ動かせる</i>
  * <i>よくある処理を短く書ける</i>
  * <i>統合されたテスト環境</i>


<!SLIDE>
# Trema = ######################################################################
## OpenFlow コントローラ向けライブラリ 
## (Ruby and C)
## +
## ネットワークエミュレータ
## +
## `trema` コマンド


<!SLIDE center>
![overview](overview.png)
