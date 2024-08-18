redis-3.2.11.tar.gz
make
redis.conf
#bind 127.0.0.1
protected-mode no

cp redis.conf  ./src/redis.conf
./src/redis-server redis.conf


redis-cli -h xxx -p 6379 


nmap -v -n -Pn -p 6379 --script redis-info  IP 


msf:
auxiliary/scanner/redis/file_upload normal Redis File Upload
redis_login 
redis_server


https://github.com/nodejs/node.git
https://github.com/evilpacket/redis-sha-crack
node ./redis-sha-crack.js -w wordlist.txt -s shalist.txt 127.0.0.1 hosts2.example.com:5555





![[7b473ac9d0366dd05e7abf7c32213de.jpg]]
!

![[711c59107929e2af4414c32d9c29cec.jpg]]

![[87ee5abcae6bf80854caa733ba17d8d.jpg]]

![[35a58c939e78014e53820981a0cfb12.jpg]]