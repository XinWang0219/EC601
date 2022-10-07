# EC601
EC601 Product Design of Electrical and Computer Engineering in Boston University

## Project One
### Project Name: Visual Question Answering
### Project Description:

Visual Question Answering (VQA) using machine learning techniques is an important field.  A person watching a scene does not only recognize objects (e.g., car, apple, tree, dog, etc.).  The person describes the scene.  For example, they will say:  it is a fast moving car or it is a car with three passengers or it is a car waiting on a stop sign for people to cross the street.
You are expected to research the problem, describe different approaches and explain use cases that will be solved with such technologies.  It is always better to define the user story then find the solution.  However, in this case, there are many applications that are need such technologies and it will be good if you can define the user story and how the technology will be applied.
It will be good to run some examples that have been posted as part of research papers or solution providers.  Your assessment of the solution will be of great value.
(EC601 Project and Research https://docs.google.com/spreadsheets/d/1DxogEUHIVr7TX0Gba7E80-u2gMqLPesWqFVa42qt6RE/edit?usp=sharing)


## Project Two
### Phase 1 a

#### Tweeter API test searching. 
Requirement: Tweeter API app, and bearer token. 
Test: Get 100 tweets about covid without retweet and in United States. Get tweets id, user name, and profile image url.
Code:
```
import tweepy
import config

client = tweepy.Client(bearer_token=config.BEARER_TOKEN)

query = 'covid -is:retweet place_country:US'
response = client.search_recent_tweets(query=query, max_results=100, tweet_fields=['created_at', 'lang'], expansions=['author_id'], user_fields=['profile_image_url'])

users = {u['id']: u for u in response.includes['users']}

for tweet in response.data:
    if users[tweet.author_id]:
        user = users[tweet.author_id]
        print(tweet.id)
        print(user.username)
        print(user.profile_image_url)
```
Result:
```
1578461223653642242
jeremy_graham
https://pbs.twimg.com/profile_images/1524878640143802368/DOwpFQiE_normal.jpg
1578461222982516736
hollystortholme
https://pbs.twimg.com/profile_images/1571313634189668352/7D3Ia8G9_normal.jpg
1578461222906626053
SimonMoose
https://pbs.twimg.com/profile_images/1545195093405073409/tEqptiQe_normal.jpg
1578461221761667072
GermanDZ
https://pbs.twimg.com/profile_images/1560189924070068224/3PQ2JWha_normal.jpg
1578461215990640640
FluffyFireman
https://pbs.twimg.com/profile_images/1470848542734360576/APMnYWVG_normal.jpg
1578461213599485954
Sticky_37BS

...

```

#### Botometer API testing
Requirement: RapidAPI account (X-RapidAPI-Key), Tweeter API app (consumer key, consumer secret, access token, access token secret)
Test: Check a single account by name or id
```
import botometer
import config

rapidapi_key = config.X_RapidAPI_Key
twitter_app_auth = {
    'consumer_key': config.API_KEY,
    'consumer_secret': config.API_SECRET,
    'access_token': config.ACCESS_TOKEN,
    'access_token_secret': config.ACCESS_TOKEN_SECRET,
  }
bom = botometer.Botometer(wait_on_ratelimit=True,
                          rapidapi_key=rapidapi_key,
                          **twitter_app_auth)

# Check a single account by screen name
result = bom.check_account('@clayadavis')
print(result)

# # Check a single account by id
# result = bom.check_account(1548959833)
```
Result:
```
{'cap': {'english': 0.4197222421546159, 'universal': 0.5222398722127631}, 'display_scores': {'english': {'astroturf': 0.0, 'fake_follower': 1.0, 'financial': 0.0, 'other': 0.4, 'overall': 0.4, 'self_declared': 0.2, 'spammer': 0.0}, 'universal': {'astroturf': 0.1, 'fake_follower': 1.0, 'financial': 0.0, 'other': 0.3, 'overall': 0.6, 'self_declared': 0.0, 'spammer': 0.0}}, 'raw_scores': {'english': {'astroturf': 0.01, 'fake_follower': 0.19, 'financial': 0.0, 'other': 0.07, 'overall': 0.08, 'self_declared': 0.04, 'spammer': 0.01}, 'universal': {'astroturf': 0.02, 'fake_follower': 0.19, 'financial': 0.0, 'other': 0.06, 'overall': 0.11, 'self_declared': 0.0, 'spammer': 0.01}}, 'user': {'majority_lang': 'en', 'user_data': {'id_str': '1548959833', 'screen_name': 'clayadavis'}}}

```
