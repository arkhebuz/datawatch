# DATAMINERWATCH
###### *A bash script designed to look after Xolo's Primeminer when mining Datacoins* 

## Why? For what?

When I have started mining primecoins on my PC, occasionally two thing were happening: miner hangs during nighttime hours and (much much rarer) miner crashes. Well, there are better things to do at night than watching miner output, so I have searched the net and surprisingly didn't found any useful solution. 

So I have written one myself - Primewatch. This is fork for Datacoin mining, as both coins are miner-compatibile. Main difference is jump-bettwen-pools mode. Not a cutting edge programming but address three things:
* Is Internet connection working at all? Script periodically pings Google DNS and if errors are returned pings four more servers and checks connection carrier, then writes info to logfile if less than half of packets is received.
* Is miner running at all? If not, launch again.
* Did miner hang somewhere at connection after all? I had hangs lasting an hour or two with lots of `force reconnect if possible!` or `system:111` communicates (on my box usually one of them was clearly dominating, not both at the same time), without any `[MASTER]` line printed to output. Script simply compares line numbers of last `force reconnect if possible!` and `system:111` communicates with line number of last `[MASTER]` communicate. If `[MASTER]` is not the last one, primeminer is killed, launched (if in jump mode pool is changed) and info is written to logs.

Note that automatic pool switching on errors can save you from downtime caused by pool crash, as the second of two currently running Datacoin pools is in early beta stage and still a bit unstable.

## Quick how-to
* I'm assuming you have primeminer binary and basic Linux skills. If not, google it, check peercointalk.org forum, etc.
* Get dataminerwatch script, either copy-paste from this site or use `git clone https://github.com/arkhebuz/datawatch` command. Use `chmod +x datawatch.sh` to make the script executable if necessary.
* **Edit** datawatch.sh file, all this is commented in code.  What you need to do there:
  1. Change catalog where logs will be stored;
  2. Set your network interface virtual filesystem catalog (like `/sys/class/net/eth0`);
  3. Set interval in seconds between checks. Should be large enough to let the miner recover under its own steam in most cases. Also, too small will make script steal cpu cycles from miner;
  4. Set primeminer binary location;
  5. Edit primeminer launch parameters. This script is written with [http://dtc.xpool.xram.co](http://dtc.xpool.xram.co) and [http://dtc.gpool.net](http://dtc.gpool.net) pools in mind, see my comments in code and check their sites;
* Launch script, pass to it which pool and mode it should use, like `./datawatch.sh gpool stay`. 
* There are two modes: "stay" and "jump". When stay (default) is used, script doesn't change the pool on connection hang. When jump is used, script will jump from one pool to another on hangs, constantly mining to the same Datacoin address. Currently xpool has 0.5 DTC payout barrier, while gpool 0.3 DTC. 
* You can launch it in terminal and just leave it on. I prefer keeping it inside GNU Screen, so I can easily attach it and kill when I need more computing power on my box, then relaunch when I'm done. In case of VPS you might want to keep script alive using supervisor, for example.

## Quirks
* Every primeminer run has it's own logfile with it's output, named with launch time (YYYY-MM-DD_hh.mm.ss). Additionally there's a `netlog` file, where only communicates from script about connection are stored.
* In jump mode script should be able to change pool if miner can't connect at all to it at the very beginning, I hope.
* Dependencies? Primeminer, pkill, pgrep, bash, grep, sed, ping... Nothing special.
* Aha, works for me. May not work for you. Tested on Debian Wheezy.
* Thread on ( [bitcointalk.org](https://bitcointalk.org/index.php?topic=390911.0) | [datacointalk.org](https://datacointalk.org/index.php?topic=112.0) )
* Donate? Just a tip maybe?  
   DTC: DMy7cMjzWycNUB4FWz2YJEmh8EET2XDvqz  
   XPM: AV3w57CVmRdEA22qNiY5JFDx1J5wVUciBY  
