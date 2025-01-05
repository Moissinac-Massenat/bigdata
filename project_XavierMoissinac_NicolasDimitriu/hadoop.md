pb 1. docker exec -it hadoop bash
[root@45b0aa1cf1d2 /]# hdfs dfs -touchz /user/iot/sensor_data/sensor_data.json
touchz: `/user/iot/sensor_data/sensor_data.json': No such file or directory: `hdfs://45b0aa1cf1d2:8020/user/iot/sensor_data/sensor_data.json'

 faire avant  : hdfs dfs -mkdir -p /user/iot/sensor_data/