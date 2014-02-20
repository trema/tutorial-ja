<!SLIDE medium>
# Task C: PacketIn Dumper ######################################################

## PacketIn メッセージを扱う


<!SLIDE medium>
## PacketIn の中身を表示 #######################################################

	$ trema run packetin-dumper.rb \
        -c packetin-dumper.conf

* PacketIn dumper コントローラを起動
* 仮想スイッチ 1 台と仮想ホスト host1, host2 も起動


<!SLIDE center>
![overview](packetin_dumper1.png)


<!SLIDE medium>
## パケットを送って PacketIn を起こす ##########################################

	(別のターミナルで実行)
	$ trema send_packets --source host1 --dest host2

* host1 から host2 へとパケットを送信
* コントローラに Packet In が送られ中身を表示


<!SLIDE center>
![overview](packetin_dumper2.png)


<!SLIDE>
## Q: テストパケットを出すには？ ###############################################


<!SLIDE medium>
# 仮想ホストと仮想リンク #######################################################

## A-1: 仮想ホストを作り仮想スイッチに接続

	@@@ ruby
	# Add one virtual switch
	vswitch { dpid "0xabc" }

	# Add two virtual hosts
	vhost "host1"
	vhost "host2"

	# Then connect them to the switch 0xabc
	link "0xabc", "host1"
	link "0xabc", "host2"

## A-2: `send_packets` でパケットを送る

	$ trema send_packets --source host1 --dest host2


<!SLIDE medium>
# 仮想ネットワーク設定ファイル #################################################

* DSL でノート PC 上にテスト環境を構築
* シンプルなコマンドでテストパケットを送信
* Ruby の文法がフルに使える (言語内 DSL)


<!SLIDE medium>
# 例: より複雑なネットワーク ###################################################

	@@@ ruby
	vswitch { dpid "0x1" }
	vswitch { dpid "0x2" }
	...
	vhost "host1"
	vhost "host2"
	vhost "host3"
	vhost "host4"
	  ...
	link "0x1", "0x2
	  ...
	link "0x1", "host1"
	link "0x1", "host2"
	link "0x2", "host3"
	link "0x2", "host4"
	  ...

<!SLIDE medium>
# 例: Ruby の文法も使える ######################################################

	@@@ ruby
	# スイッチ 10 台のフルメッシュ
	$num_switch = 10

	(1..$num_switch).each do |each|
	 vswitch { dpid each.to_hex }
	  (1..$num_switch).each do |other|
	    if each != other
	      link each.to_hex, other.to_hex
	    end
	  end
	end


<!SLIDE>
# PacketIn のハンドリング ######################################################


<!SLIDE small>
# `#packet_in` #################################################################

	@@@ ruby
	# packetin-dumper.rb
	class PacketinDumper < Controller
	  def packet_in(dpid, message)
	    info "received a packet_in"
	    info "dpid: #{datapath_id.to_hex}"
	    info "in_port: #{message.in_port}"
	  end
	end

* `packet_in` の引数: dpid と PacketIn メッセージ (`message`)
* `message#attr` : Packet-In メッセージの各種情報


<!SLIDE small>
# PacketIn のほかの情報を見る ##################################################

	@@@ ruby
	# packetin-dumper.rb
	class PacketinDumper < Controller
	  def packet_in(dpid, message)
	    info "received a packet_in"
	    info "dpid: #{datapath_id.to_hex}"
	    info "in_port: #{message.in_port}"
	    info "total_len: #{message.total_len}"
	      ...
	  end
	end

* total_len, macsa, macda などなど
* ヒント: `trema ruby` で PacketIn クラスの API を調べる
