# Bigdata Platform QuickStart
This guide describes how to download and use QuickStart dockers, which provide everything you need to try CDH, Cloudera Manager, Impala, Cloudera Search and PySpark 2.4 with Jupyter Notebook

## [Cloudera Platform](https://www.cloudera.com/documentation/enterprise/5-13-x/topics/quickstart_docker_container.html)
This section describes how to deploy a single-host Cloudera open-source distribution, including CDH and Cloudera Manager. You can use this environment to learn Hadoop, try new ideas, and test and demonstrate your application.

**Requirements**
- At least 8 GB of RAM and 2 virtual CPU. To increase the default configuration see [here](https://docs.docker.com/docker-for-mac/#advanced)


Download the cloudera docker image:
```bash
$ wget https://downloads.cloudera.com/demo_vm/docker/cloudera-quickstart-vm-5.13.0-$ 0-beta-docker.tar.gz
$ tar xzf cloudera-quickstart-vm-5.13.0-0-beta-docker.tar.gz
$ cd cloudera-quickstart-vm-5.13.0-0-beta-docker
$ docker import cloudera-quickstart-vm-5.13.0-0-beta-docker.tar cloudera_513
```
Run the container:
```bash
$ docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888:8888 -p 80:80 -p 7180:7180 cloudera_513 /usr/bin/docker-quickstart
```
Required flags and options

| Option   |      Description      | 
|----------|:-------------:|
| --hostname=quickstart.cloudera |  Required: Pseudo-distributed configuration assumes this hostname.d | 
|--privileged=true| Required: For HBase, MySQL-backed Hive metastore, Hue, Oozie, Sentry, and Cloudera Manager. | 
| -t| Required: Allocate a pseudoterminal. Once services are started, a Bash shell takes over. This switch starts a terminal emulator to run the services. | 
|-i| Required: If you want to use the terminal, either immediately or connect to the terminal later.|
|-p 8888| Recommended: Map the Hue port in the guest to another port on the host.|
|p [PORT]| Optional: Map any other ports (for example, 7180 for Cloudera Manager, 80 for a guided tutorial).|
|-d|Optional: Run the container in the background.|

Start Cloudera Manager service:
```bash
[root@quickstart /]$ sudo /home/cloudera/cloudera-manager --express --force
```

**Note**
The container contains a relational dataset *retail_dba* in as mysql db. To export it on Hive run:
 ```bash
[cloudera@quickstart ~]$ sqoop import-all-tables \
    -m 1 \
    --connect jdbc:mysql://quickstart:3306/retail_db \
    --username=retail_dba \
    --password=cloudera \
    --compression-codec=snappy \
    --as-parquetfile \
    --warehouse-dir=/user/hive/warehouse \
    --hive-import
```

Now you can find the dataset's tables as parquet files on HDFS:
```bash
[root@quickstart /]$ hdfs dfs -ls /user/hive/warehouse/
```

and Hive:

![alt text](https://github.com/fabiana001/bigdata_platform_quickstart/blob/master/imgs/example_hive.png)


## [PySpark](https://hub.docker.com/r/jupyter/pyspark-notebook/)
This section describes how to deploy a single-host Hadoop 2.7 distribution, including Spark 2.2.0. You can use this environment to learn PySpark on small, local data through Jupyter Notebook.

The following command starts a container with the Notebook server listening for HTTP connections on port 8888 with a randomly generated authentication token configured.

```bash
docker run -it --rm -p 8888:8888 jupyter/pyspark-notebook
```

Take note of the authentication token included in the notebook startup log messages (e.g. http://localhost:8888/?token=e0967346858196004e70c70d2ee05e56743caab6e2a6eb5e). Include it in the URL you visit to access the Notebook server or enter it in the Notebook login form.

![alt text](https://github.com/fabiana001/bigdata_platform_quickstart/blob/master/imgs/example_jupyter.png)
