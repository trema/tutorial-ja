<!SLIDE>
# Task B: Hello Switch! ########################################################
## OpenFlow スイッチとコントローラを接続


<!SLIDE>
## "OpenFlow スイッチを持っていないけど、<br />どうすればいい？"


<!SLIDE small>
# 演習 : Hello Switch コントローラ ############################################

	$ trema run hello-switch.rb -c hello-switch.conf
	Password: xxxxxxxx  # Enter your password here
	Hello 0xabc!  # Ctrl-c to quit

* <b>ソフトウェア版 OpenFlow スイッチ (dpid = 0xabc)</b> を起動し、コントローラと接続します
* コントローラは `"Hello 0xabc!"` と表示します
* ソフトウェア版 OpenFlow スイッチの起動は `hello-switch.conf` に定義します


<!SLIDE center>
![overview](hello_switch.png)


<!SLIDE small>
# hello-switch.conf ############################################################

	@@@ ruby
	#    
	# Add a switch with dpid == 0xabc
	#    
	vswitch { dpid "0xabc" }
	# or
	vswitch { datapath_id "0xabc" }

* ソフトウェア版 OpenFlow スイッチが起動し、コントローラとのコネクションを確立します
* Trema は <b>Full-stack</b> の開発フレームワークです。ノート PC が一台あれば、物理スイッチを持っていなくても開発ができます


<!SLIDE small>
# `hello-switch.rb` ############################################################

	@@@ ruby
	class HelloSwitch < Controller
	  def switch_ready dpid
	    info "Hello #{ dpid.to_hex }!"
	  end
	end

* `switch_ready` は、スイッチがコントローラに接続したときに呼ばれるハンドラです
* 引数の `dpid` には接続したスイッチの ID が格納されます
* `.to_hex` をつけることで `dpid` を 16 進数で表示します


<!SLIDE small>
# 演習: スイッチの追加 ####################################################

	@@@ ruby
	# hello-switch.conf
	vswitch { dpid "0x1" }
	vswitch { dpid "0x2" }
	vswitch { dpid "0x3" }
	  ...


	$ trema run hello-switch.rb -c hello-switch.conf
	???

* `hello-switch.conf` にスイッチを追加して `trema run` したとき、何が表示される？
* 注 : 各スイッチの dpid はユニークである必要があります


<!SLIDE small incremental transition=uncover>
# ここまでのサマリー ###############################################################

## Trema は "Post-Rails" なモダンなフレームワークです

<br />

* <i>書いたコードをすぐ動かせる</i>: `trema run`
* <i>Coding by Convention</i>: 分かりやすく名付けられた各種メソッド
* <i>Full-Stack</i>: ネットワーク DSL によるエミュレーション
* 便利なサブコマンド : `trema ruby`
