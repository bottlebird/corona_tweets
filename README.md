# 코로나 관련 트윗 분석
1. 2020년 5월14일-16일 동안 생성된 '코로나19' 키워드를 가진 트윗 10,000개 수집(크롤링)
2. 전처리 (불용어 제거)
3. FastText로 워드임베딩 생성 (skipgram, minCount=10)
4. (ongoing) Term Frequency-Inverse Document Frequency (TF-IDF) 기반으로 Word2Vec 구현
5. Nearest neigbors (관련어) 출력
6. (ongoing) 벡터공간 시각화

## 1. 트윗 크롤링
- 가장 먼저 2020년 5월14일-16일 동안 생성된 '코로나19' 키워드를 가진 트윗을 크롤링하기 위해 GetOldTweet3 라이브러리를 사용<br />
- 크롤링 과정에서 HTTP 429 “Too Many Requests” 에러를 방지하기 위해 MaxTweets를 10,000개로 제한<br />

```bash
tweetCriteria = got.manager.TweetCriteria().setQuerySearch('코로나19')\
                                           .setSince("2020-05-14")\
                                           .setUntil("2020-05-16")\
                                           .setMaxTweets(10000)
tweet = got.manager.TweetManager.getTweets(tweetCriteria)

tweets=[]
for cnt, tw in enumerate(tweet):
  tweets.append(tw.text)
  if cnt%100==0:
    print(tw.text)
```

## 2. 한국어 전처리 (불용어제거)
- 한국어 전처리를 위해 코엔엘파이 라이브러리 사용
- 조사나 접속사와 같은 단어들뿐만 아니라 명사, 형용사와 같은 단어들 중에서 불용어로 제거하고 싶은 단어들이 생기기 마련이다. 이럴 경우, 사용자가 직접 불용어 사전을 만들어 제거할 수도 있음.

'-는', '-나',

!pip3 install konlpy    # Python 3.x. 트윗 크롤링
가장 먼저 2020년 5월14일-16일 동안 생성된 '코로나19' 키워드를 가진 트윗을 크롤링
이를 위해 GetOldTweet3 라이브러리를 사용하나 크롤링 과정에서 HTTP 429 “Too Many Requests” 에러를 방지하기 위해 MaxTweets를 10,000개로 제한.



<p align="center">
<img src="./img/1.a_1.png" width="400" align='middle'>
</p>
For the player's statistics during the season (lower-left box cluster), AtBat(the number of times at bat) and Hits (the number of hits) are highly correlated with Runs (the number of runs). For the player's statistics during the career (upper-right box cluster), all the predictors are highly correlated to each other. 

Considering the baseball's game rules, these observations are valid.

If we look at the predictors that are correlated to the salary, CRuns (Number of runs in the career) and CRBI (Number of runs enabled in the career) show relatively high correlation. However, it does not imply causation. There are several possible explanations: (a) A influences B; (b) B influences A; and (c) A and B are influenced by one or more additional variables.
<br /><br />
Now let's look at the p-values of the predictors to understand the relationships between the salary and other predictors.
