> command
[ instruction

__________________________
[get terminal as root user
>sudo su

[open ubuntu terminal, login ,  18.04 or higher recommended 
[execute following commands one after one use 'sudo' before command as necessary
>apt update
>apt -y install build-essential
>cd /usr/local
>sudo wget "https://go.dev/dl/go1.17.3.linux-amd64.tar.gz"
>sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.17.3.linux-amd64.tar.gz
[open file /etc/profile add this line at the bottom without quots "PATH=$PATH:/usr/local/go/bin" then save and reboot the system
[execute following command
>go env -w GO111MODULE=on
[replace your github link in below command and execute
>git clone https://github.com/stars-labs/heco-chain
[go to your source folder, 'heco-chain' is just an example go inside your folder name using below command
>cd heco-chain
[compile source using below command
>make all
[go to the folder of compiled output using below command 
>cd build/bin
[make folder where chain data will be saved
>mkdir .datafolder
>chmod a+rwx .datafolder
[copy genesis.json file in same folder '...../build/bin' can upload or can use nano editor and paste (right click) text using below command 
[to save press ctrl+o and press enter, to exit from editor press ctrl+X
>nano genesis.json
[start tmux session
>tmux

[below commands are required only if you are going to setup miner node
______________________________________________________________________
[to create account for miner make sure your are inside the parth '.......build/bin' , after entering command enter password twice for your new account
[also after creating update the same in genesis.json file and edit other values as per need 
>./geth --datadir .datafolder account new
[make genesis block using this command
>./geth --datadir .datafolder init genesis.json
[start mining using below command 'enter password to unlock when promped' replace xxx wity your network id   
>./geth --datadir .datafolder --networkid xxx  -mine --unlock 0 console



[below commands are required only if you are going to setup rpc node
_____________________________________________________________________
[make genesis block using this command
>./geth --datadir .datafolder init genesis.json
[start rpc using below command replace all ip (10.10.10.10) address in below command with your server public ip also replace xxx with your network id  
>./geth --datadir .datafolder --networkid xxx --ws --ws.addr 10.10.10.10 --ws.origins "*" --ws.port 8545 --http --http.port 80 --rpc.txfeecap 0  --http.corsdomain "*" --nat "any" --http.api db,eth,net,web3,personal,txpool,miner,debug --http.addr 0.10.10.10 --http.vhosts=mainnet-rpc.xyz.com --vmdebug --pprof --pprof.port 6060 --pprof.addr 0.10.10.10 --syncmode full --gcmode=archive  --ipcpath ".datafolder/geth.ipc" console

----------------------------xxxxxxxxxxxxxxx-----------------------------------------------------------------------------

[some other commands can be used as per need 
_____________________________________________
[list running sessions of tmux
> tmux ls

[to go into a running tmux session for example session 0
> tmux attach -t 0

[to check bootnode address apply thin command from inside "......build/bin" folder
>./geth attach --exec admin.nodeInfo.enr .datafolder/geth.ipc




[to start geth on reboot
_________________________
[add below lines in /etc/profile and save that

	cd /heco-chain/build/bin
	
	if tmux has-session -t SES1 > /dev/null 2>&1; then
        :
	else
		tmux new-session -d -s SES1 
		tmux send-keys -t SES1 "./geth_start" Enter
	fi


[make a file in start.sh inside ".......build/bin" and save last geth command in that file and execute following command
>chmod +x start.sh






