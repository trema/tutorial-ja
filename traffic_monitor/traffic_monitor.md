<!SLIDE small>
# Task E: Traffic Monitor ######################################################

## FlowRemoved メッセージからトラフィックデータを取得する


<!SLIDE small>
# トラフィックデータを表示する #################################################

	$ trema run traffic-monitor.rb -c traffic-monitor.conf


	(別のターミナルで)
	$ trema send_packets --source host1 --dest host2
	$ trema send_packets --source host1 --dest host2
	$ trema send_packets --source host2 --dest host1

* "トラフィックモニター付き L2 スイッチ" コントローラを起動
* テストパケットをランダムに送る
* コントローラは、各ホストのトラフィック情報を表示する


<!SLIDE center>
![overview](traffic_monitor.png)


<!SLIDE smaller>
# トラフィック量を取得する #####################################################

	@@@ ruby
	class TrafficMonitor < Controller
	  # ...
	  def flow_removed(dpid, message)
	    @counter.add message.match.dl_src, message.byte_count
	  end

	  private

	  def flow_mod(dpid, macsa, macda, out_port)
	    send_flow_mod_add(
	      dpid,
	      :hard_timeout => 10,  # flows lifetime = 10 seconds.
	      :match => Match.new(:dl_src => macsa, :dl_dst => macda),
	      :actions => ActionOutput.new(out_port)
	    )
	  end
	  # ...
	end


* 各フローを 10 秒でタイムアウトさせる
* フローがタイムアウトした時に送られる FlowRemoved メッセージをハンドリング
* フローにより転送されたトラフィック量を記録


<!SLIDE small>
# トラフィック量を表示する #####################################################

	@@@ ruby
	class TrafficMonitor < Controller
	  periodic_timer_event :show_counter, 10

	  # ...

	  private

	  def show_counter
	    puts Time.now
	    @counter.each_pair do | mac, nbytes |
	      puts "#{mac} #{nbytes} bytes"
	    end
	  end

	  # ...
	end

* 現在時刻と `@counter` に記録されているトラフィック量を 10 秒ごとに表示


<!SLIDE small>
# Timer Attribute ##############################################################

	@@@ ruby
	class TrafficMonitor < Controller
	  periodic_timer_event :show_counter, 10

	  # ...

	  def show_counter ...
	end

* タイマーハンドラをクラス属性のように定義
* スレッドなどで自分で作らなくていい
* <i>coding by convention</i> の一例


<!SLIDE small>
# Traffic Monitor: まとめ ######################################################

* LearningSwitch を少し改造したもの
* FlowRemoved メッセージ中のトラフィックデータの取り扱い
