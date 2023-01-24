https://github.com/databacker/mysql-backup


#### # Immediate Backup
```
docker run -d --restart=always --network=upy-network --name auto_bak -e DB_DUMP_BEGIN=+0 -e DB_DUMP_TARGET=/db -e DB_SERVER=dbs_maria -e DB_USER=youpie -e DB_PASS=tf4CcEuN3dENXLgs -e DB_NAMES=youpie  -v /db:/db databack/mysql-backup
```


#### # Daily backup at 09.00 AM
```
docker run -d --restart=always --network=upy-network --name auto_bak -e DB_DUMP_BEGIN=0900 -e DB_DUMP_TARGET=/db -e DB_SERVER=dbs_maria -e DB_USER=youpie -e DB_PASS=tf4CcEuN3dENXLgs -e DB_NAMES=youpie  -v /db:/db databack/mysql-backup
```

Output will be on `/db` dir
```
root@vmi928512:/db# 
```
