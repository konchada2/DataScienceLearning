# HDFS

__Hdfs commands__

- ```hadoop fs``` : Use to communicate with HDFS from Edge Node
- _File Line Count_ - This command below calls a specific file in Hadoop file system and returns the line count.  The WC command returns the line, words and characters in a file.  The -l only returns the line value.
	
  ```
	hadoop fs -cat location | wc -l
	```

- ``` hadoop fs -ls /user/folder``` : List files in HDFS directory

- ``` hadoop fs -mkdir folder```: make a Directory in HDFS
	
- Show Jobs: ```hadoop job -list```, ``` mapred job -list```

- Kill Job: ``` hadoop job -kill job_number```

- Job Status: ```mapred job -status job_num```, ```mapred job -status job_num```
