<!SLIDE>
# Task A: Hello Trema! #########################################################


<!SLIDE commandline>
## "Hello Trema!" を実行 #######################################################

### 以下のコマンドを入力し、Trema を起動してみよう:

	$ cd trema-tutorial
	$ trema run hello-trema.rb
	Hello Trema!   # Ctrl-C to quit


<!SLIDE center>
![overview](hello_trema.png)


<!SLIDE medium>
# コントローラの起動 ###########################################################

	$ trema run [controller-file]

* コントローラを実行
* Ctrl-c で停止
* `trema help run` でオプションを表示


<!SLIDE medium incremental>
# すぐに動かせる ###############################################################

* `trema run` で、書いたコントローラをすぐ実行
* 仮想ネットワークで動作をテスト
* "コーディング、テスト、デバッグ" の短いサイクル


<!SLIDE small>
# Trema でのコントローラの書き方 ###############################################


<!SLIDE medium>
# hello-trema.rb ###############################################################

	@@@ ruby
	class HelloController < Controller
	  def start
	    info "Hello Trema!"
	  end
	end

* シンプル! これだけでコントローラが動く
* (ただし Hello Trema! と表示するだけ)


<!SLIDE small>
# コントローラクラス ###########################################################

	@@@ ruby
	class HelloController < Controller
	  # ...
	end

* すべてのコントローラはクラス (`class HelloController)
* Trema の `Controller` クラスを継承
* コントローラに必要なメソッドも継承 (FlowMod の送信等)


<!SLIDE small>
# イベントハンドラ #############################################################

	@@@ ruby
	class MyController < Controller
	  def start  # start-up event handler
	    # ...
	  end

	  def packet_in(dpid, msg)  # Packet-in received handler
	    # ...
	  end

	  # ...
	end

* イベントドリブンにコントローラを記述
* 各イベントハンドラをインスタンスメソッドとして実装


<!SLIDE small>
# イベントハンドラ (Floodlight の場合) #########################################

	@@@ java
	// Packet-in handling in Floodlight
	public Command receive(IOFSwitch sw, ...) {
	  switch (msg.getType()) {
	    case PACKET_IN:
	      return this.handlePacketIn(sw, ...);

	    // ...

	private Command handlePacketIn(IOFSwitch sw, ...) {
	    // ...

* Floodlight では複雑なイベント振り分けが必要
* 「おまじない」が多く、コードの見通しが悪い


<!SLIDE small>
# イベントの振り分け ###########################################################

	@@@ ruby
	# Packet-in handling in Trema
	class MyController < Controller
	  def start  # automatically called at startup
	    # ...
	  end

	  def packet_in(dpid, msg)  # automatically caled when receiving a packet-in
	    # ...
	  end
	end

* Trema はイベントの振り分けにリフレクションを使う
* 面倒なイベントディスパッチやハンドラの登録が不要


<!SLIDE small transition=uncover>
# コーディングのための工夫 #####################################################

* "handler name" == "message name"
* イベントディスパッチのような「おまじない」を不要に
* お約束やつまらない部分を削減し, 楽しくプログラミング


<!SLIDE small>
# 短く書こう ###################################################################

* コードの長さと生産性の間には強い相関関係
  * e.g., Arc Programming Language [Paul Graham]
* コードを短くすることで、
  * お約束コードを書く時間を最小にする
  * バグの可能性を少なくする

## Trema は性能よりも
## <b>プログラマーの生産性</b>を重視


<!SLIDE small>
# Logging API ##################################################################

	@@@ ruby
	class HelloController < Controller
	  def start
	    info "Hello Trema!"	 # outputs an info level message
	  end
	end

* ロギングレベルに応じたシンプルな API (debug, info, etc.)
* `trema ruby` で Logging API を含む API リファレンスを表示
