# Analyzing Tweets using Spark RDD

The goal of this lab is to use Spark RDDs to analyze a large volume of Tweets in a Spark cluster.

# Table of contents

- [Grading](#grading). Where does the grade of this lab come from?
- [Delivering](#delivering-the-exercises). How to deliver these exercises.
- [Required exercises](#required-exercises). You must deliver all these exercises to be awarded a 10.
    - [Lab 3: Downloading Tweets from S3 and parsing them from JSON](#lab-3-downloading-tweets-from-s3-and-parsing-them-from-json) - 3 exercises (15 marks)
    - [Seminar 3: Using Spark RDDs](#seminar-3-using-spark-rdds) - 5 exercises (35 marks)
    - [Lab 4: Analyzing Tweets with Spark](#lab-4-analyzing-tweets-with-spark) - 4 exercises (40 marks)
    - [Seminar 4: Running Spark in AWS](#seminar-4-running-spark-in-aws) - 1 exercise (15 marks)

# Required exercises

## Lab 3: Parsing Tweets as JSON

### [L3Q0] [5 marks] The tweets dataset

- Follow the Developer Setup to download the needed data if you did not at the beginning of the course.
- Take a look at the first Tweet: `cat Eurovision3.json -n | head -n 1 | jq`. [Help](https://unix.stackexchange.com/questions/288521/with-the-linux-cat-command-how-do-i-show-only-certain-lines-by-number#:~:text=cat%20%2Fvar%2Flog%2Fsyslog%20-n%20%7C%20head%20-n%2050%20%7C,-b10%20-a10%20will%20show%20lines%2040%20thru%2060.)
- **[1 mark]** What field in the JSON object of a Tweet contains the user bio?
- **[1 mark]** What field in the JSON object of a Tweet contains the language?
- **[1 mark]** What field in the JSON object of a Tweet contains the text content?
- **[1 mark]** What field in the JSON object of a Tweet contains the number of followers?
- Take a look at the first two lines: `cat Eurovision3.json -n | head -n 2`.
- **[1 mark]** How many Tweets does each line contain?

### [L3Q1] [5 marks] Parsing JSON with Python

- Create a file `tweet_parser.py`
- Create a `Tweet` dataclass with fields for the `tweet_id` (int), `text` (str), `user_id` (int), `user_name` (str), `language` (str), `timestamp_ms` (int) and `retweet_count` (int). [Help](https://realpython.com/python-data-classes/)
- Create a function `parse_tweet(tweet: str) -> Tweet` that takes in a Tweet as a Json string and returns a Tweet object. [Help](https://stackoverflow.com/a/7771071)
- Read the first line of `Eurovision3.json` and print the result of `parse_tweet`. [Help](https://stackoverflow.com/questions/1904394/read-only-the-first-line-of-a-file)
- Take a screenshot and add it to the README.
- Push your changes.

### [L3Q2] [5 marks] Counting Tweets by language

- Create a file `simple_tweet_language_counter.py`
- Implement a script that reads each line of `Eurovision3.json` one by one. [Help](https://stackoverflow.com/a/3277512)
    - You might need to skip any invalid lines, such as empty lines with only a `\n` or Tweets with an invalid JSON format.
- Parse each Tweet using the `parse_tweet` function from the previous exercise.
- Count the number of Tweets of each language using a dictionary. [Help](https://www.w3schools.com/python/python_dictionaries.asp)
- Print the dictionary. Take a screenshot and add it to the README.
- Push your changes.

## Seminar 3: Using Spark RDDs

> Before starting this section, read [RDD Programming Guide](https://spark.apache.org/docs/latest/rdd-programming-guide.html) and [Resilient Distributed Datasets](https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf)

### [S3Q0] [10 marks] What is Spark RDD?

- **[1 mark]** What is the difference between a transformation and an action?
- **[1 mark]** What is the difference between a wide and a narrow dependency? What is a stage in Spark RDD?
- Start up a Spark cluster locally using Docker compose: `docker-compose up`.
- **[1 mark]** How many Spark workers exist in your local cluster? Take a screenshot of Docker Desktop and add it to the README.
- **[3 mark]** What is a lambda function in Python?
- **[3 mark]** What do the RDD operations `map`, `filter`, `groupByKey` and `flatMap` do?
- Check the local IP for the Spark Master service in the `spark-master-1` container logs. You should see a log similar to `Starting Spark master at spark://172.20.0.2:7077`.
- Run the job with Spark: `docker-compose exec spark-master spark-submit --master spark://{IP_FRM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_sum.py /opt/bitnami/spark/app/data/numbers1.txt`
- **[1 mark]** Take a close look at the logs. What was the result of your job?

### [S3Q1] [5 marks]  Sum the numbers

The file [numbers2.txt](./data/numbers2.txt) has many lines, each with many numbers.

- Create a file `spark_sum2.py`
- Implement and run a Spark job that computes the sum of all the numbers.
- Write the command you used to run it in the README and show a screenshot of the result.

### [S3Q3] [5 marks] Sum the even numbers

The file [numbers2.txt](./data/numbers2.txt) has many lines, each with many numbers.

- Create a file `spark_sum3.py`
- Implement and run a Spark job that computes the sum of all the even numbers.
- Write the command you used to run it in the README and show a screenshot of the result.

### [S3Q6] [5 marks] Find how many people live in each city

The file [people.txt](./data/people.txt) has many lines, each with `{NAME} {LANGUAGE} {CITY}`.

- Create a file `spark_count_people.py`
- Implement and run a Spark job that counts how many people live in each city.
- Write the command you used to run it in the README and show a screenshot of the result.

### [S3Q7] [5 marks] Count the bigrams

The file [cat.txt](./data/cat.txt) has many lines, each with a sentence.

- Create a file `spark_count_bigrams.py`
- Implement and run a Spark job that counts how many people live in each city.
- Write the command you used to run it in the README and show a screenshot of the result.

## Lab 4: Analyzing Tweets with Spark

### [L4Q0] [10 marks] Filtering Tweets by language with Spark

- Create a file `spark_tweet_language_filter.py`.
- Implement a Spark job that finds all the tweets in a file for a given language (e.g. `zh`)
- Saves the result to a file
- Run your code in your local Spark cluster:
```zsh
docker-compose exec spark-master spark-submit --master spark://{IP_FROM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_tweet_language_filter.py zh /opt/bitnami/spark/app/data/Eurovision3.json /opt/bitnami/spark/output/Eurovision3Zh.json
```

> You might need to `chmod 755 data` if you get "file not found" errors


### [L4Q1] [10 marks] Get the most repeated bigrams

- Create a file `spark_tweet_bigrams.py`.
- Implement a Spark job that finds the most repeated bigrams for a language (e.g. `es`)
- Filter out bigrams that only appear once
- Saves the result to a file (sorted by how many times they appear in descending order)
- Run your code in your local Spark cluster:
```zsh
docker-compose exec spark-master spark-submit --master spark://{IP_FROM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_tweet_bigrams.py es /opt/bitnami/spark/app/data/Eurovision3.json /opt/bitnami/spark/output/Eurovision3EsBigrams
```


### [L4Q2] [10 marks] Get the 10 most retweeted tweets

- Create a file `spark_tweet_retweets.py`.
- Implement a Spark job that finds the users with the top 10 most retweeted Tweets for a language
- Run your code in your local Spark cluster:
```zsh
docker-compose exec spark-master spark-submit --master spark://{IP_FROM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_tweet_retweets.py es /opt/bitnami/spark/app/data/Eurovision3.json
```

### [L4Q3] [10 marks] Get the 10 most retweeted users

- Create a file `spark_tweet_user_retweets.py`.
- Implement a Spark job that finds the users with the top 10 most retweets (in total) for a language and how many retweets they have. I.e., sum all the retweets each user has and get the top 10 users.
- Run your code in your local Spark cluster:
```zsh
docker-compose exec spark-master spark-submit --master spark://{IP_FROM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_tweet_user_retweets.py es /opt/bitnami/spark/app/data/Eurovision3.json
```

## Seminar 4: Running Spark in AWS

AWS allows us to rent virtual servers and deploy a Spark cluter to do data anlysis at scale. In this seminar, you will learn how to:
- Use S3 to store and read files
- Use AWS EMR to host a Spark cluster in AWS EC2 servers
- Run some of your Spark applications in the cluster.

### [S4Q0] [10 marks] Run L4Q1 in AWS using EMR

- Accept the invitation to AWS academy.
- Open the [AWS Academy](https://awsacademy.instructure.com/courses) course
- In `Modules`, select `Launch AWS Academy Learner Lab`
- Click `Start Lab`
- Wait until the `AWS` indicator has a green circle
- Click the `AWS` text with the green circle to open the AWS console

> [!TIP]
> When you launch a cluster, you start spending AWS credit! Remember to terminate your cluster at the end of your experiments!

- [Create a bucket in S3](https://us-east-1.console.aws.amazon.com/s3/home?region=us-east-1#):
    - Bucket type: `General purpose`
    - Name: `lsds-2025-{group_number}-t{theory_number}-p{lab_number}-s{seminar_number}-s3bucket`

- In the bucket, create 4 folders: `input`, `app`, `logs` and `output`

- Upload the `Eurovision3.json` file inside the `input` folder

- Upload `spark_tweet_user_retweets.py` and `tweet_parser.py` in the `app` folder

- Open the [EMR console](https://us-east-1.console.aws.amazon.com/emr/home?region=us-east-1#/clusters)

- Create a cluster
    - Application bundle: `Spark Interactive`
    - Name: `lsds-2025-{group_number}-t{theory_number}-p{lab_number}-s{seminar_number}-sparkcluster`
    - Choose this instance type: `m4.large`
    - Instance(s) size: `3`
    - Cluster logs: select the `logs` folder in the S3 bucket you created
    - Service role: `EMR_DefaultRole`
    - Instance profile: `EMR_EC2_DefaultRole`

- In `Steps`, select `Add step`.
    - Type: `Spark application`
    - Name: `lab2-ex13`
    - Deploy mode: `Cluster mode`
    - Application location: select the `spark_tweet_user_retweets.py` in the S3 bucket
    - Spark-submit options: specify the `tweet_parser.py` module. For example: `--py-files s3://lsds-2025-miquel-test/app/tweet_parser.py`
    - Arguments: specify the input and output. For example: `es s3://lsds-2025-miquel-test/input/Eurovision3.json`.

- When you submit a step, wait until the `Status` is `Completed`. 

> [!TIP]
> You can find the logs in your S3 bucket: `logs/{cluster id}/containers/application_*_{run number}/container_*_000001/stdout.gz` - they might take some minutes to appear

- Paste a screenshot of the log where we can see: how much time it took, what are the ids of the ten most retweeted users.
