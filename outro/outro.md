<!SLIDE>
# まとめ #######################################################################


<!SLIDE small incremental transition=uncover>
# Trema で OpenFlow をアジャイルに #############################################

* コーディング、実行、デバッグを短いサイクルで
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
# Sources ######################################################################

* This Tutorial: <http://github.com/trema/tutorial-ja/>
* Trema: <http://github.com/trema/>
* Trema/Apps: <http://github.com/trema/apps/>
* Web Page: <http://trema.github.com/trema/>
* Twitter: <http://twitter.com/trema_news>
* Mailing List: <https://groups.google.com/group/trema-dev>
* Bugs: <https://github.com/trema/trema/issues>


<!SLIDE>
# 質問タイム ###################################################################
