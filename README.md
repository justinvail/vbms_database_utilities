# Oracle 19c in Docker with data backup utilities

This data utility uses oracle functionality (expdp, exp, impdp, imp) to export and import VBMS databases (both structure and data).  
This is great for making backups of your database and sharing them with other people.  
The data dump of a clean liquibase install is only about 15mb.  
The days of doing a 20+ minute liquibase clean install are over.  Just grab the dmp files from a friend.
These scripts are completely compatible with the 19c image we are using.  

##Instructions:
###Set these environment variables:
These are suggested default values.  Change them at your own discretion. 
```
export VBMS_DB_CONTAINER_NAME=vbms-dev-docker-19c
export VBMS_DB_DUMP_LOCATION=~/VBMS_DB/VBMS_Database_Exports
export VBMS_DB_PASSWORD=welcome1
```
Run these commands (Make sure you are off the devvpn to pull the container): 
```
docker pull -a oracledb19c/oracle.19.3.0-ee;
docker run \
        -v $VBMS_DB_DUMP_LOCATION:/opt/backup \
        -p 1521:1521 \
        -e ORACLE_PDB=XE \
        -e ORACLE_PDB_SID=XE \
        -e ORACLE_SID=ORCLCDB \
        -e ORACLE_PWD=$VBMS_DB_PASSWORD \
        -e ORACLE_HOME=/opt/oracle/product/19c/dbhome_1 \
        -ti \
        --shm-size=1g \
        --name=$VBMS_DB_CONTAINER_NAME oracledb19c/oracle.19.3.0-ee:oracle19.3.0-ee;
```
Stop your container and start it back up normally. 

You can now either run liquibase to build you DB or import one with ./importVBMSUsers
```
Export: ./dumpVBMSUsers
Import: ./importVBMSUsers
```
