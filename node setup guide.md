# SETUP Instructions

__________________________

## open ubuntu terminal, login ,  18.04 or higher recommended ,execute following commands one after one use 'sudo' before command as necessary

>apt update

>apt -y install build-essential

>cd /usr/local

>sudo wget "https://go.dev/dl/go1.17.3.linux-amd64.tar.gz"

>sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.17.3.linux-amd64.tar.gz

## Add go in PATH 

[open file /etc/profile add this line at the bottom without quots "PATH=$PATH:/usr/local/go/bin" then save and reboot the system, execute following command, than enter the following command ]

>go env -w GO111MODULE=on

## To clone source use below command and execute

>git clone https://github.com/tyco-core

## Go to your source folder, 'tyco-core' is just an example go inside your folder name using below command

>cd tyco-core

## Compile source using below command

>make all

## Go to the folder of compiled output using below command 

>cd build/bin

## Make folder where chain data will be saved

>mkdir .datafolder

>chmod a+rwx .datafolder

## Place genesis.json file
[copy genesis.json file in same folder '...../build/bin' can upload or can use nano editor and paste (right click) text using below command ,to save press ctrl+o and press enter, to exit from editor press ctrl+X]

>nano genesis.json

## Start tmux session

>tmux

## Below commands are required only if you are going to setup miner node
______________________________________________________________________

## to create account for miner make sure your are inside the parth '.......build/bin' , after entering command enter password twice for your new account

>./geth --datadir .datafolder account new

[or to use your existing account, save your private key in a file on the same location and use below command, after below command execution you can delete your private key file here assumed 'pk.txt' is contains your private key]

>./geth --datadir .datafolder account import pk.txt

## Make genesis block using this command

>./geth --datadir .datafolder init genesis.json

## Start mining using below command 'enter password to unlock when promped' replace xxx wity your network id  for tyco mainnet use 3630   

>./geth --datadir .datafolder --networkid xxx  -mine --unlock 0 console

[once miner node with your validator address started, then complete the process of staking required coin to become a validator]




## Below commands are required only if you are going to setup rpc node
_____________________________________________________________________

## Make genesis block using this command

>./geth --datadir .datafolder init genesis.json

[start rpc using below command replace all ip (10.10.10.10) address in below command with your server public ip also replace xxx with your network id ]

>./geth --datadir .datafolder --networkid xxx --ws --ws.addr 10.10.10.10 --ws.origins "*" --ws.port 8545 --http --http.port 80 --rpc.txfeecap 0  --http.corsdomain "*" --nat "any" --http.api db,eth,net,web3,personal,txpool,miner,debug --http.addr 0.10.10.10 --http.vhosts=mainnet-rpc.xyz.com --vmdebug --pprof --pprof.port 6060 --pprof.addr 0.10.10.10 --syncmode full --gcmode=archive  --ipcpath ".datafolder/geth.ipc" console

----------------------------xxxxxxxxxxxxxxx-----------------------------




## Some other commands can be used as required
_____________________________________________
## List running sessions of tmux

> tmux ls

## To go into a running tmux session for example session 'SES1'

> tmux attach -t SES1

#t# To check bootnode address apply thin command from inside "......build/bin" folder

[this boot node address can be used with geth command to make sync automatically]

>./geth attach --exec admin.nodeInfo.enr .datafolder/geth.ipc




## to start geth on reboot
_________________________

## Add below lines in /etc/profile and save that 


[note here tmux session name is 'SES1' ] 

	cd /tyco-core/build/bin
	
	if tmux has-session -t SES1 > /dev/null 2>&1; then
        :
	else
		tmux new-session -d -s SES1 
		tmux send-keys -t SES1 "./start.sh" Enter
	fi


## Mmake a file 'start.sh' inside ".......build/bin" and save last geth command (according to miner or rpc) in that file and execute following command

>chmod +x start.sh






