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

### Phase 2 TweeterViewpointMeter
EC601/ProjectTwo/Tweeter_Viewpoint_Meter/
#### Request
python lib:     
import tweepy
import requests
from collections import Counter
import matplotlib.pyplot as plt
import json

Tweepy Authentication:
access token
access token secret
bearer secret
api key
api secret

RapidAPI Authentication:
X-RapidAPI-Key
X-RapidAPI-Host

#### Manual
main.py is a using demo
```
import config
import TweeterViewpointMeter as TVM

if __name__ == "__main__":
    # load TweeterViewpointMeter
    covid_meter = TVM.TweeterViewpointMeter()

    # load topic
    covid_meter.topic = "Covid"

    # load language
    covid_meter.language = "en"

    # load N number data
    covid_meter.datanum = 100

    # load tweepy keys
    covid_meter.register_tweepy(config.BEARER_TOKEN, consumer_key=config.API_KEY, consumer_secret=config.API_SECRET, access_token=config.ACCESS_TOKEN, access_token_secret=config.ACCESS_TOKEN_SECRET)

    # load RapidAPI key to use NPL Sentiment Analysis API and Botometer API
    covid_meter.register_rapidapi(config.Sentimeter_RapidAPI_Key, config.Sentimeter_RapidAPI_Host)

    # run the program
    covid_meter.run()
```
#### Demo Result
```
{'agreement': 'AGREEMENT', 'confidence': '100', 'irony': 'NONIRONIC', 'model': 'general_en', 'score_tag': 'N', 'sentence_list': [{'agreement': 'AGREEMENT', 'bop': 'y', 'confidence': '100', 'endp': '15', 'inip': '0', 'score_tag': 'NONE', 'segment_list': [{'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '15', 'inip': '0', 'polarity_term_list': [], 'score_tag': 'NONE', 'segment_type': 'secondary', 'sentimented_entity_list': [{'endp': '15', 'form': '@TashaKheiriddin', 'id': '__14978305156356868134', 'inip': '0', 'score_tag': 'NONE', 'type': 'Top>Id>Nickname', 'variant': '@TashaKheiriddin'}], 'text': '@TashaKheiriddin'}], 'sentimented_concept_list': [], 'sentimented_entity_list': [{'form': '@TashaKheiriddin', 'id': '__14978305156356868134', 'score_tag': 'NONE', 'type': 'Top>Id>Nickname'}], 'text': '@TashaKheiriddin'}, {'agreement': 'AGREEMENT', 'bop': 'n', 'confidence': '100', 'endp': '56', 'inip': '17', 'score_tag': 'N', 'segment_list': [{'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '42', 'inip': '17', 'polarity_term_list': [{'confidence': '100', 'endp': '24', 'inip': '17', 'score_tag': 'N', 'text': 'shock@V'}], 'score_tag': 'N', 'segment_type': 'main', 'text': 'Shocking that people still'}, {'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '55', 'inip': '44', 'polarity_term_list': [], 'score_tag': 'NONE', 'segment_type': 'secondary', 'text': 'believe this'}], 'sentimented_concept_list': [], 'sentimented_entity_list': [], 'text': 'Shocking that people still believe this.'}, {'agreement': 'AGREEMENT', 'bop': 'y', 'confidence': '100', 'endp': '124', 'inip': '59', 'score_tag': 'NONE', 'segment_list': [{'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '79', 'inip': '59', 'polarity_term_list': [], 'score_tag': 'NONE', 'segment_type': 'secondary', 'text': 'If you get vaccinated'}, {'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '124', 'inip': '82', 'polarity_term_list': [], 'score_tag': 'NONE', 'segment_type': 'main', 'sentimented_concept_list': [{'endp': '120', 'form': 'COVID-19', 'id': '4a889b46b7', 'inip': '116', 'score_tag': 'NONE', 'type': 'Top>OtherEntity>Disease', 'variant': 'covid'}], 'text': 'how can you be less likely to get covid if:'}], 'sentimented_concept_list': [{'form': 'COVID-19', 'id': '4a889b46b7', 'score_tag': 'NONE', 'type': 'Top>OtherEntity>Disease'}], 'sentimented_entity_list': [], 'text': 'If you get vaccinated, how can you be less likely to get covid if:'}, {'agreement': 'AGREEMENT', 'bop': 'y', 'confidence': '100', 'endp': '199', 'inip': '127', 'score_tag': 'NONE', 'segment_list': [{'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '168', 'inip': '127', 'polarity_term_list': [], 'score_tag': 'NONE', 'segment_type': 'secondary', 'text': '50% of the population is triple vaccinated'}, {'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '198', 'inip': '174', 'polarity_term_list': [], 'score_tag': 'NONE', 'segment_type': 'secondary', 'text': 'they take up 77% of cases'}, {'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '199', 'inip': '199', 'polarity_term_list': [], 'score_tag': 'NONE', 'segment_type': 'secondary', 'text': '?'}], 'sentimented_concept_list': [], 'sentimented_entity_list': [], 'text': '50% of the population is triple vaccinated but they take up 77% of cases?'}, {'agreement': 'AGREEMENT', 'bop': 'n', 'confidence': '100', 'endp': '242', 'inip': '201', 'score_tag': 'NONE', 'segment_list': [{'agreement': 'AGREEMENT', 'confidence': '100', 'endp': '241', 'inip': '201', 'polarity_term_list': [], 'score_tag': 'NONE', 'segment_type': 'main', 'sentimented_concept_list': [{'endp': '241', 'form': 'COVID-19', 'id': '4a889b46b7', 'inip': '237', 'score_tag': 'NONE', 'type': 'Top>OtherEntity>Disease', 'variant': 'covid'}], 'text': 'You are actually MORE likely to get covid'}], 'sentimented_concept_list': [{'form': 'COVID-19', 'id': '4a889b46b7', 'score_tag': 'NONE', 'type': 'Top>OtherEntity>Disease'}], 'sentimented_entity_list': [], 'text': 'You are actually MORE likely to get covid.'}], 'sentimented_concept_list': [{'form': 'COVID-19', 'id': '4a889b46b7', 'score_tag': 'NONE', 'type': 'Top>OtherEntity>Disease'}], 'sentimented_entity_list': [{'form': '@TashaKheiriddin', 'id': '__14978305156356868134', 'score_tag': 'NONE', 'type': 'Top>Id>Nickname'}], 'status': {'code': '0', 'msg': 'OK', 'credits': '1', 'remaining_credits': '223416'}, 'subjectivity': 'SUBJECTIVE'}
['AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT', 'AGREEMENT', 'AGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'DISAGREEMENT', 'AGREEMENT']
dict_keys(['AGREEMENT', 'DISAGREEMENT'])
dict_values([63, 37])
['100', '86', '94', '94', '100', '100', '94', '94', '94', '86', '98', '100', '100', '100', '100', '100', '100', '100', '100', '94', '94', '100', '94', '92', '91', '100', '100', '86', '100', '100', '100', '100', '94', '94', '100', '100', '100', '86', '100', '86', '92', '100', '100', '91', '100', '100', '100', '92', '100', '100', '86', '100', '100', '97', '94', '98', '86', '100', '94', '100', '94', '100', '94', '86', '100', '100', '94', '86', '94', '92', '100', '100', '100', '100', '100', '98', '100', '92', '100', '100', '100', '100', '94', '92', '100', '100', '91', '100', '94', '100', '100', '92', '86', '100', '100', '100', '94', '94', '94', '100']
dict_keys(['100', '86', '94', '98', '92', '91', '97'])
dict_values([55, 10, 21, 3, 7, 3, 1])
['NONE', 'P', 'N', 'N', 'P', 'N', 'NEU', 'P', 'P', 'NEU', 'P+', 'N+', 'NONE', 'P', 'N', 'N', 'NONE', 'P', 'NONE', 'NEU', 'N', 'P+', 'N', 'N', 'N', 'NONE', 'P', 'NEU', 'N', 'NONE', 'N+', 'N+', 'N', 'NEU', 'N', 'N', 'NONE', 'P', 'P', 'N', 'P', 'NONE', 'N+', 'NEU', 'N', 'NONE', 'P', 'N', 'NONE', 'N', 'N', 'NONE', 'N+', 'P', 'NEU', 'N+', 'NEU', 'P+', 'N', 'NONE', 'NEU', 'N+', 'N', 'N', 'P+', 'NONE', 'P', 'N', 'N', 'P+', 'N', 'N+', 'P', 'NONE', 'P', 'P+', 'P', 'P', 'N', 'N', 'N', 'NONE', 'P', 'P', 'N+', 'NONE', 'P', 'N', 'NEU', 'N', 'P', 'N', 'N', 'P', 'NONE', 'N+', 'P', 'P', 'NEU', 'N']
```

