# PySpark ETL

# Data Lakes with PySpark and S3

### Concepts:

- Data Lake:

    A data lake is a system or repository of data stored in its natural/raw format, usually object blobs or files.
    
- Spark:

    Apache Spark is a unified analytics engine for big data processing, with built-in modules for streaming, SQL, machine learning and graph processing.

### Project:

        A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

### Dataset:
- Song Dataset :
    
    The song data set is a subset of the [Million Song Dataset](https://labrosa.ee.columbia.edu/millionsong/).

    Each item contains metadata about a song and it's artist.

    Example format:

    ``` json
    {
        "num_songs": 1, 
        "artist_longitude": null,
        "artist_latitute": null,
        "artist_location": "", 
        "artist_name": "Line Renaud", 
        "song_id": "SOUPIRU12A6D4FA1E1", 
        "title": "Der Kleine Dompfaff", 
        "duration": 152.92036, 
        "year": 0
    }
    ```

- Logs Dataset:

    The logs dataset is a simulation of events generated by this [Event Simulator](https://github.com/Interana/eventsim).

    They are stored in s3 with the following bucket organization structure:

    ``` 
    log_data/2018/11/2018-11-12-events.json
    log_data/2018/11/2018-11-13-events.json
    ```
    
    The json file format has the following structure:

    ``` json
    {
        "artist":null,
        "auth":"Logged In",
        "firstName":"Walter",
        "gender":"M",
        "itemInSession":0,
        "lastName":"Frye",
        "length":null,
        "level":"free",
        "location":"San Francisco-Oakland-Hayward, CA",
        "method":"GET",
        "page":"Home",
        "registration":1540919166796.0,
        "sessionId":38,
        "song":null,
        "status":200,
        "ts":1541105830796,
        "userAgent":"\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"",
        "userId":"39"
    }
    ```

### Data Lake Schema:

The target schema should look as follows:

- **Fact Table**:
    - **Table songplays** - records in event data associated with song plays i.e. records with page NextSong
        - Columns: songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

- **Dimension Tables**:
    - **Table users**: users in the app
        - Columns: user_id, first_name, last_name, gender, level
    - **Table songs**: songs in music database
        - Columns: song_id, title, artist_id, year, duration
    - **Table artists**: artists in music database
        - Columns: artist_id, name, location, lattitude, longitude
    - **Table time**: timestamps of records in songplays broken down into specific units
        - Columns: start_time, hour, day, week, month, year, weekday

### Project Files:

`etl.py`: 
    Runs the logic to copy the data from one S3 bucket to another, transforming it using PySpark.

`dl.cfg`:
    Contains the needed AWS Keys to access s3.

### Running this project:
- Make sure you have java8 installed and set as default.

    - On linux
    ```
    sudo apt install openjdk-8-jdk
    
    sudo update-alternatives --config java
    ```
    Choose Java 8 after running the command above and then
    Check if the Java 8 version was set propperly
    
     ```
    java -version
     ```
- Update dl.cfg with
    -  AWS Access Keys

- Update etl.py with a new public s3 bucket address assigned to `output_data`

- Run: `etl.py`
- Check your created `output_data` s3 bucket.

### AWS Services:

    S3:
        Object storage service. Commonly used as a Data Lake central data storage layer. In this project, data from application logs is stored on s3 and should be Extracted, Transformed and Loaded into redshift.
    

    EMR:
       Amazon EMR is the industry leading cloud-native big data platform for processing vast amounts of data quickly and cost-effectively at scale. Using open source tools such as Apache Spark.
    



