# SQL CRUD Workshop
#### By Catherine Benge

# Part 1

## Creating and Importing | Setting up our environment

#### Link to restaurants.csv
[click here](data/restaurants.csv)

#### Creating a database for all tables

`sqlite3 sqlcrud.db`

#### creating the restaurants table

    CREATE TABLE restaurants (
        id INTEGER PRIMARY KEY,
        name TEXT,
        category TEXT,
        price_tier TEXT, 
        neighborhood TEXT,
        opening_hours REAL,
        closing_hours REAL,
        average_rating INTEGER,
        good_for_kids TEXT 
    );

#### creating the temp table
    CREATE TABLE temp_table (
        id INTEGER PRIMARY KEY,
        name TEXT,
        category TEXT,
        price_tier TEXT, 
        neighborhood TEXT,
        opening_hours REAL,
        closing_hours REAL,
        average_rating INTEGER,
        good_for_kids TEXT 
    );

#### switching to csv mode to import

    .mode csv
    .headers on


#### importing the csv file and skipping the first line to avoid error

`.import --skip 1 data/restaurants.csv temp_table`

#### insert fields into restaurant table

`INSERT INTO restaurants (id, name, category, price_tier, neighborhood, opening_hours, closing_hours, average_rating, good_for_kids) SELECT * FROM temp_table;`

#### delete the temp table

`DROP TABLE temp_table;`

## Queries
#### Tables are taken as excerpts of output

1. #### Find all cheap restaurants in a particular neighborhood (pick any neighborhood as an example).

Query: 

`SELECT * FROM restaurants WHERE neighborhood="West Village" AND price_tier="Cheap";`

Result:

|  id |            name           |  category  | price_tier | neighborhood | opening_hours | closing_hours | average_rating | good_for_kids |   
|:---:|:-------------------------:|:----------:|:----------:|:------------:|:-------------:|:-------------:|:--------------:|:-------------:|
| 43  | vulcan cup lichen         | Fast Food  | Cheap      | West Village | 10:00         | 20:00         | 5              | true          |  
| 58  | featherplume              | Fast Food  | Cheap      | West Village | 9:00          | 15:00         | 4              | true          |    
| 128 | tylothallia lichen        | Italian    | Cheap      | West Village | 8:00          | 16:00         | 2              | true          |   
| 182 | bedford springs hawthorn  | Korean     | Cheap      | West Village | 7:00          | 20:00         | 5              | false         |   
| 246 | bahama baybean            | French     | Cheap      | West Village | 8:00          | 22:00         | 5              | true          |     
| 380 | santa monica dust lichen  | Korean     | Cheap      | West Village | 11:00         | 16:00         | 2              | true          |    
| 419 | astomum moss              | Australian | Cheap      | West Village | 8:00          | 15:00         | 1              | false         |     
| 436 | evert's springparsley     | Russian    | Cheap      | West Village | 10:00         | 20:00         | 4              | false         |     
| 460 | white easterbonnets       | Mexican    | Cheap      | West Village | 10:00         | 18:00         | 3              | true          |


2.  #### Find all restaurants in a particular genre (pick any genre as an example) with 3 stars or more, ordered by the number of stars in descending order.

Query: 

`SELECT * FROM restaurants WHERE category="Chinese" AND average_rating>=3 ORDER BY average_rating DESC;`

Result:

|  id |            name            | category | price_tier |   neighborhood   | opening_hours | closing_hours | average_rating | good_for_kids |  
|:---:|:--------------------------:|:--------:|:----------:|:----------------:|:-------------:|:-------------:|:--------------:|:-------------:|
| 4   | muehlenberg's astomum moss | Chinese  | Cheap      | Astoria          | 11:00         | 22:00         | 5              | true          |   
| 21  | threadleaf crowfoot        | Chinese  | Expensive  | Brooklyn Heights | 7:00          | 22:00         | 5              | true          |   
| 53  | texas vervain              | Chinese  | Medium     | Upper East Side  | 11:00         | 22:00         | 5              | true          |   
| 132 | meadow brome               | Chinese  | Cheap      | Chinatown        | 10:00         | 15:00         | 5              | true          |  
| 140 | limestone phacelia         | Chinese  | Expensive  | Murray Hill      | 8:00          | 16:00         | 5              | true          |  
| 160 | tree stonecrop             | Chinese  | Cheap      | Williamsburg     | 7:00          | 20:00         | 5              | false         |   
| 222 | manystem liveforever       | Chinese  | Expensive  | Harlem           | 9:00          | 15:00         | 5              | true          |   
| 262 | shield nasturtium          | Chinese  | Expensive  | Williamsburg     | 10:00         | 18:00         | 5              | true          |   
| 378 | heartleaf arnica           | Chinese  | Cheap      | Chinatown        | 10:00         | 16:00         | 5              | true          |   
| 410 | panamint mountain lupine   | Chinese  | Expensive  | Brooklyn Heights | 10:00         | 15:00         | 5              | true          |   
| 417 | taimyr catchfly            | Chinese  | Cheap      | Tribeca          | 11:00         | 18:00         | 5              | true          |



3. #### Find all restaurants that are open now (see hint below).

Query: 

`SELECT * FROM restaurants WHERE strftime('%H:%M' , closing_hours) > strftime('%H:%M' , 'now', 'localtime');`

Result:

| id |            name            |  category | price_tier |   neighborhood   | opening_hours | closing_hours | average_review | good_for_kids |
|:--:|:--------------------------:|:---------:|:----------:|:----------------:|:-------------:|:-------------:|:--------------:|:-------------:|
| 3  | sechium                    | Italian   | Expensive  | Williamsburg     | 11:00         | 22:00         | 4              | false         |
| 4  | muehlenberg's astomum moss | Chinese   | Cheap      | Astoria          | 11:00         | 22:00         | 5              | true          |
| 8  | american water starwort    | Ethiopian | Medium     | Astoria          | 11:00         | 22:00         | 4              | false         |
| 10 | trans-pecos giant hyssop   | Japanese  | Expensive  | West Village     | 7:00          | 22:00         | 1              | true          |
| 14 | hawai'i jadevine           | Peruvian  | Medium     | West Village     | 9:00          | 22:00         | 1              | true          |
| 15 | calico aster               | Fast Food | Medium     | Brooklyn Heights | 8:00          | 22:00         | 5              | false         |
| 18 | maquei                     | Italian   | Expensive  | Flushing         | 9:00          | 22:00         | 1              | false         |
| 21 | threadleaf crowfoot        | Chinese   | Expensive  | Brooklyn Heights | 7:00          | 22:00         | 5              | true          |
| 25 | albany hawthorn            | Chinese   | Cheap      | Greenpoint       | 9:00          | 22:00         | 2              | false         |
| 27 | crenate fishscale lichen   | Diner     | Medium     | Upper East Side  | 7:00          | 22:00         | 5              | true          |


4. #### Leave a review for a restaurant (pick any restaurant as an example).

Query:

    CREATE TABLE reviews (
        id INTEGER,
        name TEXT,
        category TEXT,
        price_tier TEXT,
        neighborhood TEXT,
        opening_hours REAL,
        closing_hours REAL,
        average_review INTEGER,
        good_for_kids TEXT,
        reviews TEXT
    );

    INSERT INTO reviews VALUES(
        15,
        'Calico Aster',
        'Fast Food',
        'Medium',
        'Brooklyn Heights',
        '8:00',
        '22:00',
        5,
        'false',
        'Really great fast food! And vegan too! Highly recommend.'
    );

| id |     name     |  category | price_tier |   neighborhood   | opening_hours | closing_hours | average_review | good_for_kids |                          reviews                         |
|:--:|:------------:|:---------:|:----------:|:----------------:|:-------------:|:-------------:|:--------------:|:-------------:|:--------------------------------------------------------:|
| 15 | Calico Aster | Fast Food | Medium     | Brooklyn Heights | 8:00          | 22:00         | 5              | false         | Really great fast food! And vegan too! Highly recommend. |

5. #### Delete all restaurants that are not good for kids.

Query:

`DELETE FROM restaurants WHERE good_for_kids='false';`

| id |            name            | category | price_tier |   neighborhood   | opening_hours | closing_hours | average_review | good_for_kids |
|:--:|:--------------------------:|:--------:|:----------:|:----------------:|:-------------:|:-------------:|:--------------:|:-------------:|
| 1  | american beech             | Korean   | Medium     | Rockaway         | 7:00          | 16:00         | 1              | true          |
| 4  | muehlenberg's astomum moss | Chinese  | Cheap      | Astoria          | 11:00         | 22:00         | 5              | true          |
| 6  | tuftedstem rush            | Korean   | Expensive  | Upper West Side  | 7:00          | 18:00         | 1              | true          |
| 7  | pineland dewberry          | Ghanian  | Expensive  | Flushing         | 9:00          | 18:00         | 2              | true          |
| 10 | trans-pecos giant hyssop   | Japanese | Expensive  | West Village     | 7:00          | 22:00         | 1              | true          |
| 12 | mealy goosefoot            | Mexican  | Cheap      | Flushing         | 7:00          | 15:00         | 2              | true          |
| 13 | belladonna                 | Peruvian | Expensive  | Upper West Side  | 7:00          | 20:00         | 3              | true          |
| 14 | hawai'i jadevine           | Peruvian | Medium     | West Village     | 9:00          | 22:00         | 1              | true          |
| 20 | northern biscuitroot       | French   | Expensive  | Long Island City | 8:00          | 20:00         | 2              | true          |
| 21 | threadleaf crowfoot        | Chinese  | Expensive  | Brooklyn Heights | 7:00          | 22:00         | 5              | true          |

6. #### Find the number of restaurants in each NYC neighborhood.

All restaurants, before kid-unfriendly data was deleted:

Query:

`SELECT neighborhood, COUNT(name) FROM restaurants GROUP BY neighborhood;`

Results:

|   Neighborhood   | Count |
|:----------------:|:-----:|
| Astoria          | 52    |
| Brooklyn Heights | 49    |
| Bushwick         | 56    |
| Chinatown        | 57    |
| Corona           | 51    |
| East Village     | 30    |
| Flushing         | 52    |
| Greenpoint       | 50    |
| Harlem           | 44    |
| Hell's Kitchen   | 55    |
| Jackson Heights  | 42    |
| Long Island City | 59    |
| Murray Hill      | 33    |
| Nolita           | 53    |
| Rockaway         | 59    |
| Tribeca          | 49    |
| Upper East Side  | 52    |
| Upper West Side  | 53    |
| West Village     | 58    |
| Williamsburg     | 46    |

Kid-friendly restaurants (after query 5 deleted values)

Query:

`SELECT neighborhood, COUNT(name) FROM restaurants GROUP BY neighborhood;`

Results:

|   Neighborhood   | Count |
|:----------------:|:-----:|
| Astoria          | 29    |
| Brooklyn Heights | 25    |
| Bushwick         | 25    |
| Chinatown        | 36    |
| Corona           | 25    |
| East Village     | 15    |
| Flushing         | 24    |
| Greenpoint       | 33    |
| Harlem           | 19    |
| Hell's Kitchen   | 30    |
| Jackson Heights  | 21    |
| Long Island City | 36    |
| Murray Hill      | 12    |
| Nolita           | 26    |
| Rockaway         | 33    |
| Tribeca          | 30    |
| Upper East Side  | 30    |
| Upper West Side  | 27    |
| West Village     | 36    |
| Williamsburg     | 24    |



# Part 2

#### Link to users.csv and posts.csv
[users](data/users.csv)
[posts](data/posts.csv)

#### creating the users table

    CREATE TABLE users (
        user_id INTEGER PRIMARY KEY,
        email TEXT,
        password TEXT,
        username TEXT
    );


#### creating the temp table

    CREATE TABLE temp_table (
        user_id INTEGER PRIMARY KEY,
        email TEXT,
        password TEXT,
        username TEXT
    );

#### switching to csv mode to import

    .mode csv
    .headers on

#### importing the csv file and skipping the first line to avoid error

`.import --skip 1 data/users.csv temp_table`

#### insert fields into users table

`INSERT INTO users (user_id, email, password, username) SELECT * FROM temp_table;`

#### delete the temp table

`DROP TABLE temp_table;`

#### creating the posts table

    CREATE TABLE posts (
        post_id INTEGER PRIMARY KEY,
        user_id INTEGER,
        type TEXT,
        recipient_id INTEGER,
        post_content TEXT,
        viewed TEXT,
        time_posted REAL
    );

#### creating the temp table
    CREATE TABLE temp_table (
        post_id INTEGER PRIMARY KEY,
        user_id INTEGER,
        type TEXT,
        recipient_id INTEGER,
        post_content TEXT,
        viewed TEXT,
        time_posted REAL
    );

#### importing the csv file and skipping the first line to avoid error

`.import --skip 1 data/posts.csv temp_table`

#### insert fields into posts table

`INSERT INTO posts (post_id, user_id, type, recipient_id, post_content, viewed, time_posted) SELECT * FROM temp_table;`

#### delete the temp table

`DROP TABLE temp_table;`


## Queries

1. #### Register a new User.

        INSERT INTO users (
            user_id,
            email,
            password,
            username
        )
        VALUES(
            1001,
            'jeremy@jeremy.com',
            'password555',
            'jeremyiscool'
        );

| user_id |       email       |   password  |   username   |
|:-------:|:-----------------:|:-----------:|:------------:|
| 1001    | jeremy@jeremy.com | password555 | jeremyiscool |

2. #### Create a new Message sent by a particular User to a particular User (pick any two Users for example).

        INSERT INTO posts (
            post_id,
            user_id,
            type,
            recipient_id,
            post_content,
            viewed,
            time_posted
        )
        VALUES (
            1001,
            346,
            'message',
            475,
            'Hey there!',
            'FALSE',
            '2021-03-06 18:34:51'
        );


| post_id | user_id |   type  | recipient_id | post_content | viewed |     time_posted     |
|:-------:|:-------:|:-------:|:------------:|:------------:|:------:|:-------------------:|
| 1001    | 346     | message | 475          | Hey there!   | FALSE  | 2021-03-06 18:34:51 |

3. #### Create a new Story by a particular User (pick any User for example).

        INSERT INTO posts (
            post_id,
            user_id,
            type,
            recipient_id,
            post_content,
            viewed,
            time_posted
        )
        VALUES (
            1002,
            212,
            'story',
            ' ',
            'My story today is cool.',
            ' ',
            '2021-03-10 18:34:51'
        );

| post_id | user_id |  type | recipient_id |       post_content      | viewed |     time_posted     |
|:-------:|:-------:|:-----:|:------------:|:-----------------------:|:------:|:-------------------:|
| 1002    | 212     | story |              | My story today is cool. |        | 2021-03-10 18:34:51 |

4. #### Show the 10 most recent visible Messages and Stories, in order of recency.

`SELECT * FROM posts WHERE ROUND((JULIANDAY('now') - JULIANDAY(time_posted)) * 24) < 24 OR viewed='FALSE' ORDER BY time_posted DESC LIMIT 10;`

| post_id | user_id |   type  | recipient_id |                               post_content                               | viewed |     time_posted     |
|:-------:|:-------:|:-------:|:------------:|:------------------------------------------------------------------------:|:------:|:-------------------:|
| 1002    | 212     | story   |              | My story today is cool.                                                  |        | 2021-03-10 18:34:51 |
| 589     | 188     | message | 213          | She wrote him a long letter, but he didn't read it.                      | FALSE  | 2021-03-09 9:37:27  |
| 49      | 178     | message | 180          | Carol drank the blood as if she were a vampire.                          | FALSE  | 2021-03-09 6:40:53  |
| 689     | 196     | message | 189          | He had concluded that pigs must be able to fly in Hog Heaven.            | FALSE  | 2021-03-09 6:07:37  |
| 245     | 181     | message | 194          | Eating eggs on Thursday for choir practice was recommended.              | FALSE  | 2021-03-09 22:23:12 |
| 101     | 185     | message | 178          | The sign said there was road work ahead so he decided to speed up.       | FALSE  | 2021-03-09 20:49:15 |
| 149     | 231     | message | 226          | She always had an interesting perspective on why the world must be flat. | FALSE  | 2021-03-09 19:10:31 |
| 135     | 209     | message | 206          | Traveling became almost extinct during the pandemic.                     | FALSE  | 2021-03-09 10:10:01 |
| 959     | 188     | message | 206          | Her hair was windswept as she rode in the black convertible.             | FALSE  | 2021-03-08 7:34:29  |
| 41      | 167     | message | 185          | Shakespeare was a famous 17th-century diesel mechanic.                   | FALSE  | 2021-03-08 5:54:45  |

5. #### Show the 10 most recent visible Messages sent by a particular User to a particular User (pick any two Users for example), in order of recency.

`SELECT * FROM posts WHERE type='message' AND viewed='FALSE' AND user_id=187 AND recipient_id=213 ORDER BY time_posted DESC LIMIT 10;`

*(there are not a full 10 messages sent between these users)*

| post_id | user_id |   type  | recipient_id |                                  post_content                                 | viewed |     time_posted     |
|:-------:|:-------:|:-------:|:------------:|:-----------------------------------------------------------------------------:|:------:|:-------------------:|
| 15      | 187     | message | 213          | He was willing to find the depths of the rabbit hole in order to be with her. | FALSE  | 2021-02-04 15:19:48 |

6. #### Make all Stories that are more than 24 hours old invisible.

`SELECT * FROM posts WHERE ROUND((JULIANDAY('now') - JULIANDAY(time_posted)) * 24) < 24 AND type='story';`

7. #### Show all invisible Messages and Stories, in order of recency.

`SELECT * FROM posts WHERE ROUND((JULIANDAY('now') - JULIANDAY(time_posted)) * 24) >= 24 ORDER BY time_posted DESC;`

| post_id | user_id |   type  | recipient_id |                                         post_content                                         | viewed |     time_posted     |
|:-------:|:-------:|:-------:|:------------:|:--------------------------------------------------------------------------------------------:|:------:|:-------------------:|
| 754     | 218     | story   | 222          | He walked into the basement with the horror movie from the night before playing in his head. |        | 2021-03-09 23:57:08 |
| 38      | 157     | story   | 200          | The fish dreamed of escaping the fishbowl and into the toilet where he saw his friend go.    |        | 2021-03-09 22:38:39 |
| 245     | 181     | message | 194          | Eating eggs on Thursday for choir practice was recommended.                                  | FALSE  | 2021-03-09 22:23:12 |
| 340     | 217     | story   | 158          | I hear that Nancy is very pretty.                                                            |        | 2021-03-09 21:09:36 |
| 397     | 188     | message | 251          | He colored deep space a soft yellow.                                                         | TRUE   | 2021-03-09 20:54:44 |
| 101     | 185     | message | 178          | The sign said there was road work ahead so he decided to speed up.                           | FALSE  | 2021-03-09 20:49:15 |
| 572     | 229     | story   | 219          | Not all people who wander are lost.                                                          |        | 2021-03-09 19:30:18 |
| 149     | 231     | message | 226          | She always had an interesting perspective on why the world must be flat.                     | FALSE  | 2021-03-09 19:10:31 |
| 864     | 192     | story   | 231          | She saw the brake lights, but not in time.                                                   |        | 2021-03-09 18:40:56 |
| 787     | 216     | message | 208          | I often see the time 11:11 or 12:34 on clocks.                                               | TRUE   | 2021-03-09 17:59:22 |

8. #### Show the number of posts by each User.

`SELECT users.username, COUNT(post_content) FROM posts,users WHERE posts.user_id=users.user_id GROUP BY post_content;`

|   username  | posts |
|:-----------:|:-----:|
| gdobie4x    | 3     |
| ablazi5r    | 1     |
| acaygill5x  | 1     |
| srapin4b    | 5     |
| pspuffard5y | 2     |
| mcanelas5q  | 1     |
| mcanelas5q  | 3     |
| scall49     | 2     |
| pspuffard5y | 1     |
| lcorkel59   | 5     |


9. #### Show the post text and email address of all posts and the User who made them within the last 24 hours.

`SELECT users.username, users.email, posts.post_content FROM users, posts WHERE ROUND((JULIANDAY('now') - JULIANDAY(time_posted)) * 24) < 24 AND posts.user_id=users.user_id;`

|  username |      email      |        post_content       |
|:---------:|:---------------:|:-------------------------:|
| bbernon5v | bwill5v@aol.com | "My story today is cool." |

10. #### All the email addresses of all Users who have not posted anything yet.

`SELECT users.user_id, users.email, users.username FROM users LEFT JOIN posts ON posts.user_id=users.user_id WHERE posts.user_id IS NULL;`

| user_id |             email            |   username   |
|:-------:|:----------------------------:|:------------:|
| 1       | dquick0@examiner.com         | ddelbergue0  |
| 2       | lbullier1@archive.org        | sminogue1    |
| 3       | aspratling2@domainmarket.com | gmitchinson2 |
| 4       | bstamps3@dagondesign.com     | jlademann3   |
| 5       | ngaine4@xinhuanet.com        | acolgan4     |

## Thank you
Thank you for reading! Have a nice day.

Sincerely,
Catherine Benge
