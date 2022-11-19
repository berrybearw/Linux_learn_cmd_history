# Linux_learn_cmd_history
User 過往執行指令紀錄

參考 : 

* [https://www.modb.pro/db/438127](https://www.modb.pro/db/438127)
* [http://emmanuel.iffly.free.fr/doku.php?id=aix:aix_history](http://emmanuel.iffly.free.fr/doku.php?id=aix:aix_history)

bash 方式
---    
    
### 方案 1 紀錄在 .bash_history
-----
- step 1 :
        
建立一個執行 script

```bash
CLIENT_IP=`echo -n $SSH_CLIENT|cut -d' ' -f1`
#if [[ `tail -n1 ~/.bash_history|rev|cut -c -4|rev` != `date +%Y` ]] ;then
if [[ `tail -n1 /u1/usr/tiptop/.sh_history|rev|cut -c -4|rev` != `date +%Y` ]] ;then
    echo $CLIENT_IP
    #echo "$CLIENT_IP" >> ~/.bash_history
    #echo "$CLIENT_IP" >> /u1/usr/tiptop/.ssh_history
fi
```
 
 - step 2 :
 
 調整 ~/.bash_profile 
 
 ```bash
 
 export HISTTIMEFORMAT="%Y-%m-%d %H:%M:%S : "
 
 export PROMPT_COMMAND="history -a; /bin/bash /usr/local/bin/bash_detailed_history; $PROMT_COMMAND"
 
 shopt -s histappend
 ```
     
 - step 3 :
     
直接重新讀取 . ~/.bash_profile

或 開 新 session

執行指令接著查看 $HOME/.bash_history 是否有紀錄
     
### 方案 2 自己選擇記錄在哪
-----

- step 1 調整 /etc/profile

設定 PROMPT_COMMAND

```bash
IP=`who am i | awk '{print $NF}' | sed -e 's/[()]//g'`
#export HISTTIMEFORMAT=$USER@$IP' %F %T'
#HISTTIMEFORMAT='%F %T '
LOGIN_IP=$(who am i | awk '{print $NF}')
export PROMPT_COMMAND='{ msg=$(history 1 | { read a b c d; echo $d; });echo $(date +"%Y-%m-%d %H:%M:%S") [$(whoami)@$SSH_USER$LOGIN_IP `pwd` ]" $msg" >> /ut/topprd/tmp/bash_history; }'
```
        
- step 2 測試

開 新 session

執行指令接著查看是否有紀錄

ksh 方式
---

`trap ' 指令 | read -s ' debug`
