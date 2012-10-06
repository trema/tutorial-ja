<!SLIDE>
# サマリー #######################################################################


<!SLIDE small incremental transition=uncover>
# Trema: "Rails のように OpenFlow を" ##################################################

* <i>書いたコードをすぐ動かせる</i>: コーディング、実行、デバッグのループを短いサイクルで
  * 仮想ネットワーク DSL
  * `trema {run, send_packets, show_stats, up, kill}`
* <i>Coding by Convention</i>: 短く書く
  * naming coversion によるイベントの自動振り分け
  * Class アトリビュート: `periodic_timer_event`
  * Syntactic sugars: `ExactMatch.from`
  * デフォルトオプション: `send_flow_mod_add`
* <i>Trema のサブコマンド</i>
  * `trema {dump_flows, ruby}`


<!SLIDE small>
# 開発者、次の一歩のために #####################################################

* `[trema]/src/examples`
  * API の使い方を示すシンプルなサンプルアプリケーション
  * Ruby と C の API を理解するために最適なリファレンス
* Trema/Apps <http://github.com/trema/apps>
  * より実用的・実験的なコントローラアプリケーション
  * 実際に使うためのコントローラ開発の出発点として最適


<!SLIDE>
# Love Ruby? ###################################################################


<!SLIDE>
# Love C? ######################################################################


<!SLIDE small>
# Trema C ######################################################################

* Trema は Ruby と C 両方のライブラリを提供 … 開発者が選択可能
* Trema C もまた Trema Ruby のようにシンプル

<br />

	$ gcc myapp.c `trema-config -c -l` -o myapp
	$ trema run myapp


<!SLIDE small>
# Sources ######################################################################

* This Tutorial: <http://github.com/trema/GEC13/>
* Trema: <http://github.com/trema/>
* Trema/Apps: <http://github.com/trema/apps/>
* Web Page: <http://trema.github.com/trema/>
* Twitter: <http://twitter.com/trema_news>
* Mailing List: <https://groups.google.com/group/trema-dev>
* Bugs: <https://github.com/trema/trema/issues>


<!SLIDE small>
# コントリビュータ! ################################################################

* コントリビュータには、Trema T シャツを差し上げます
  * パッチ (New features/apps, enhancements, bug-fixes)
  * バグレポート
  * ドキュメント等
* pull-requests を送って、T シャツを手に入れよう！


<!SLIDE>
# Questions? ###################################################################


