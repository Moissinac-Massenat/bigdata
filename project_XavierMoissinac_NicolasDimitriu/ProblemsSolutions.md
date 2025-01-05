# Common Issues and Solutions

- **`hdfs` command not found:**

    **Solution:** Ensure you are inside the Hadoop container using `docker exec -it hadoop bash`.

- **`/user/iot/sensor_data/` directory does not exist:**

    **Solution:** Create it with `hdfs dfs -mkdir -p /user/iot/sensor_data`.

- **Local file not found with `hdfs dfs -put`:**

    **Solution:** Replace `/path/to/local/file.json` with the actual path to a file on your host system.

- **`docker` command not found inside the Hadoop container:**

    **Solution:** Run `docker` commands from the host machine, not from within the Hadoop container.

- **Empty file when using `hdfs dfs -cat`:**

    **Solution:** Add data to the file using `hdfs dfs -put` or `docker cp`.
