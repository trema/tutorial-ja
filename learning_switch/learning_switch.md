<!SLIDE small>
# Task D: Learning Switch ######################################################

## flow\_mod と packet\_out を送る


<!SLIDE smaller>
# 演習: 送受信パケット量を表示する ##################################################

## L2 スイッチコントローラ (learning_switch) を起動する:

	$ trema run learning-switch.rb -c learning-switch.conf

<br />
<br />

## 別のターミナルを開き、テストパケットを送る
## `show_stats` で送受信パケット量に関する情報を表示する
	
	$ trema send_packet --source host1 --dest host2
	$ trema show_stats host1
	$ trema show_stats host2


<!SLIDE center>
![overview](show_stats.png)


<!SLIDE small>
# 演習: フローテーブル ####################################################

	$ trema send_packet --source host2 --dest host1
	$ trema dump_flows 0xabc

## 上記のコマンドは、スイッチ 0xabc のフローテーブルを表示する


<!SLIDE center>
![overview](dump_flows.png)


<!SLIDE small>
# 今回使用した Trema のサブコマンド ##########################################################

* `trema show_stats HOST_NAME`
* `trema dump_flows SWITCH_NAME`

## 様々な統計情報と内部情報を表示可能


<!SLIDE small>
# Learning Switch のソースコード #################################################


<!SLIDE smaller>
# Learning Switch ##############################################################

	@@@ ruby
	class LearningSwitch < Controller
	  # ...
	  def packet_in dpid, message
	    @fdb.learn message.macsa, message.in_port
	    port_no = @fdb.lookup( message.macda )
	    if port_no
	      flow_mod dpid, message, port_no
	      packet_out dpid, message, port_no
	    else
	      flood dpid, message
	    end
	  end
	  # ...
	end

# 擬似コードのように簡単に読むことができるはず？


<!SLIDE smaller>
# 詳しく見ていこう ###############################################################

	@@@ ruby
	class LearningSwitch < Controller
	  # ...
	  def packet_in dpid, message
	    @fdb.learn message.macsa, message.in_port
	    port_no = @fdb.lookup( message.macda )
	    if port_no
	      flow_mod dpid, message, port_no
	      packet_out dpid, message, port_no
	    else
	      flood dpid, message
	    end
	  end
	  # ...
	end

* Packet In メッセージが送られてきた時に、送信元 MAC アドレス (macsa) と受信ポート (in_port) を Forwarding DB (FDB) に記録する
* 宛先 MAC アドレス (macda) から送出ポートを検索する
* もし見つかれば、スイッチのフローテーブルを更新して、パケットを Packet-Out する
* 見つからなければ、パケットを flood する


<!SLIDE smaller>
# プライベートメソッド ##############################################################

* `flow_mod`, `packet_out`, `flood` は、learning_switch のプライベートメソッドです (Trema API ではありません)

* 適切なネーミングは、コードの可読性を高めます


<!SLIDE smaller>
# flow_mod #####################################################################

	@@@ ruby
	class LearningSwitch < Controller
	  # ...
	  private
	  def flow_mod dpid, message, port_no
	    send_flow_mod_add(
	      dpid,
	      :match => ExactMatch.from( message ),
	      :actions => ActionOutput.new( port_no )
	    )
	  end
	  # ...
	end

* 受信パケットからマッチ条件を作り、指定のポートに出力
* 短くて、見通しのよいコード


<!SLIDE smaller>
# Syntactic Sugar: `ExactMatch.from()` #########################################

	@@@ ruby
	ExactMatch.from( message )

# vs.

	@@@ ruby
	Match.new(
	  :in_port => message.in_port,
	  :nw_src => message.nw_src,
	  :nw_dst => message.nw_dst,
	  :tp_src => message.tp_src,
	  :tp_dst => message.tp_dst,
	  :dl_src => message.dl_src,
	  :dl_dst => message.dl_dst,
	  ...
	)


<!SLIDE smaller>
# Trema vs. NOX Python #########################################################

	@@@ ruby
	# Trema
	send_flow_mod_add(
	  dpid,
	  :match => ExactMatch.from( message ),
	  :actions => ActionOutput.new( port_no )
	)

# vs.

	@@@ python
	# NOX Python
	inst.install_datapath_flow(
	  dpid,
	  extract_flow(packet),
	  CACHE_TIMEOUT, 
	  openflow.OFP_FLOW_PERMANENT,
	  [[openflow.OFPAT_OUTPUT, [0, prt[0]]]],
	  bufid,
	  openflow.OFP_DEFAULT_PRIORITY,
	  inport,
	  buf
	)


<!SLIDE small>
# Learning Switch: サマリー #####################################################

* 内部の状態表示 : `trema show_stats`, `trema dump_flows`
* 短く書くための API: `ExactMatch.from`, `send_flow_mod_add`
