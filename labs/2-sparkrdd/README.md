# Analyzing Tweets using Spark RDD

The goal of this lab is to use Spark RDDs to analyze a large volume of Tweets in a Spark cluster.

# Table of contents

- [Grading](#grading). Where does the grade of this lab come from?
- [Required exercises](#required-exercises). You must deliver all these exercises to be awarded a 10.
    - [Lab 3: Downloading Tweets from S3 and parsing them from JSON](#lab-3-downloading-tweets-from-s3-and-parsing-them-from-json) - 3 exercises (15 marks)
    - [Seminar 3: Using Spark RDDs](#seminar-3-using-spark-rdds) - 5 exercises (35 marks)
    - [Lab 4: Analyzing Tweets with Spark](#lab-4-analyzing-tweets-with-spark) - 4 exercises (40 marks)
    - [Seminar 4: Running Spark in AWS](#seminar-4-running-spark-in-aws) - 1 exercise (15 marks)

# Grading

The grade for this lab is computed as:

```
lab2_grade = delivered_marks / required_exercises_total_marks * 10
```

The final labs grade is computed as:

```
labs_grade = (lab1_grade + lab2_grade + lab3_grade) / 3
```

If plagiarism is detected, `labs_grade` is a 0.

# Required exercises

## Lab 3: Downloading Tweets from S3 and parsing them from JSON

### EX0. Downloading Tweet data from S3 [5 marks]

- Watch [AWS S3 Tutorial For Beginners](https://www.youtube.com/watch?v=tfU0JEZjcsg)
- Download `s3://lsds2022/twitter-eurovision-2018.tar.gz` from S3 using the AWS CLI. [Help. Example 4: Copying an S3 object to a local file](https://docs.aws.amazon.com/cli/latest/reference/s3/cp.html)
- Extract the `.tar.gz` contents into the `data` folder. [Help](https://askubuntu.com/questions/25347/what-command-do-i-need-to-unzip-extract-a-tar-gz-file).
- Take a look at the first Tweet: `cat Eurovision3.json -n | head -n 1 | jq`. [Help](https://unix.stackexchange.com/questions/288521/with-the-linux-cat-command-how-do-i-show-only-certain-lines-by-number#:~:text=cat%20%2Fvar%2Flog%2Fsyslog%20-n%20%7C%20head%20-n%2050%20%7C,-b10%20-a10%20will%20show%20lines%2040%20thru%2060.)
- **[1 mark]** What field in the JSON object of a Tweet contains the user bio?
- **[1 mark]** What field in the JSON object of a Tweet contains the language?
- **[1 mark]** What field in the JSON object of a Tweet contains the text content?
- **[1 mark]** What field in the JSON object of a Tweet contains the number of followers?
- Take a look at the first two lines: `cat Eurovision3.json -n | head -n 2`.
- **[1 mark]** How many Tweets does each line contain?

### EX1. Parsing JSON with Python [5 marks]

- Create a file `tweet_parser.py`
- Create a `Tweet` dataclass with fields for the `tweet_id` (int), `text` (str), `user_id` (int), `user_name` (str), `language` (str), `timestamp_ms` (int) and `retweet_count` (int). [Help](https://realpython.com/python-data-classes/)
- Create a function `parse_tweet(tweet: str) -> Tweet` that takes in a Tweet as a Json string and returns a Tweet object. [Help](https://stackoverflow.com/a/7771071)
- Read the first line of `Eurovision8.json` and print the result of `parse_tweet`. [Help](https://stackoverflow.com/questions/1904394/read-only-the-first-line-of-a-file)
- Take a screenshot and add it to the README.
- Push your changes.

### EX2. Counting Tweets by language [5 marks]

- Create a file `simple_tweet_language_counter.py`
- Implement a script that reads each line of `Eurovision8.json` one by one. [Help](https://stackoverflow.com/a/3277512)
    - You might need to skip any invalid lines, such as empty lines with only a `\n` or Tweets with an invalid JSON format.
- Parse each Tweet using the `parse_tweet` function from the previous exercise.
- Count the number of Tweets of each language using a dictionary. [Help](https://www.w3schools.com/python/python_dictionaries.asp)
- Print the dictionary. Take a screenshot and add it to the README.
- Push your changes.

## Seminar 3: Using Spark RDDs

> Before starting this section, read [RDD Programming Guide](https://spark.apache.org/docs/latest/rdd-programming-guide.html) and [Resilient Distributed Datasets](https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf)

### EX3. What is Spark RDD? [10 marks]

- **[1 mark]** What is the difference between a transformation and an action?
- **[1 mark]** What is the difference between a wide and a narrow dependency? What is a stage in Spark RDD?
- Start up a Spark cluster locally using Docker compose: `docker-compose up`.
- **[1 mark]** How many Spark workers exist in your local cluster? Take a screenshot of Docker Desktop and add it to the README.
- **[3 mark]** What is a lambda function in Python?
- **[3 mark]** What do the RDD operations `map`, `filter`, `groupByKey` and `flatMap` do?
- Check the local IP for the Spark Master service in the `spark-master-1` container logs. You should see a log similar to `Starting Spark master at spark://172.20.0.2:7077`.
- Run the job with Spark: `docker-compose exec spark-master spark-submit --master spark://{IP_FRM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_sum.py /opt/bitnami/spark/app/data/numbers1.txt`
- **[1 mark]** Take a close look at the logs. What was the result of your job?

### EX4. Sum the numbers [5 marks]

The file [numbers2.txt](./data/numbers2.txt) has many lines, each with many numbers.

- Create a file `spark_sum2.py`
- Implement and run a Spark job that computes the sum of all the numbers.
- Write the command you used to run it in the README and show a screenshot of the result.

### EX5. Sum the even numbers [5 marks]

The file [numbers2.txt](./data/numbers2.txt) has many lines, each with many numbers.

- Create a file `spark_sum3.py`
- Implement and run a Spark job that computes the sum of all the even numbers.
- Write the command you used to run it in the README and show a screenshot of the result.

### EX6. Find how many people live in each city [5 marks]

The file [people.txt](./data/people.txt) has many lines, each with `{NAME} {LANGUAGE} {CITY}`.

- Create a file `spark_count_people.py`
- Implement and run a Spark job that counts how many people live in each city.
- Write the command you used to run it in the README and show a screenshot of the result.

### EX7. Count the bigrams [5 marks]

The file [cat.txt](./data/cat.txt) has many lines, each with a sentence.

- Create a file `spark_count_bigrams.py`
- Implement and run a Spark job that counts how many people live in each city.
- Write the command you used to run it in the README and show a screenshot of the result.

## Lab 4: Analyzing Tweets with Spark

### EX8. Filtering Tweets by language with Spark [10 marks]

- Create a file `spark_tweet_language_filter.py`.
- Implement a Spark job that finds all the tweets in a file for a given language (e.g. `zh`)
- Saves the result to a file
- Run your code in your local Spark cluster:
```zsh
docker-compose exec spark-master spark-submit --master spark://{IP_FROM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_tweet_language_filter.py zh /opt/bitnami/spark/app/data/Eurovision10.json /opt/bitnami/spark/output/Eurovision10Zh.json
```

> You might need to `chmod 755 data` if you get "file not found" errors


### EX9. Get the most repeated bigrams [10 marks]

- Create a file `spark_tweet_bigrams.py`.
- Implement a Spark job that finds the most repeated bigrams for a language (e.g. `es`)
- Filter out bigrams that only appear once
- Saves the result to a file (sorted by how many times they appear in descending order)
- Run your code in your local Spark cluster:
```zsh
docker-compose exec spark-master spark-submit --master spark://{IP_FROM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_tweet_bigrams.py es /opt/bitnami/spark/app/data/Eurovision10.json /opt/bitnami/spark/output/Eurovision10EsBigrams
```


### EX10. Get the 10 most retweeted tweets [10 marks]

- Create a file `spark_tweet_retweets.py`.
- Implement a Spark job that finds the users with the top 10 most retweeted Tweets for a language
- Run your code in your local Spark cluster:
```zsh
docker-compose exec spark-master spark-submit --master spark://{IP_FROM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_tweet_retweets.py es /opt/bitnami/spark/app/data/Eurovision10.json
```

### EX11. Get the 10 most retweeted users [10 marks]

- Create a file `spark_tweet_user_retweets.py`.
- Implement a Spark job that finds the users with the top 10 most retweets (in total) for a language. I.e., sum all the retweets each user has and get the top 10 users.
- Run your code in your local Spark cluster:
```zsh
docker-compose exec spark-master spark-submit --master spark://{IP_FROM_PREVIOUS_STEP}:7077 /opt/bitnami/spark/app/spark_tweet_user_retweets.py es /opt/bitnami/spark/app/data/Eurovision10.json
```

## Seminar 4: Running Spark in AWS

### EX12. Run EX9 in AWS using EMR [1 mark]

TODO

