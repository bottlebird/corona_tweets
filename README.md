# 코로나 관련 트윗 분석
1. 2020년 5월14일-16일 동안 생성된 '코로나19' 키워드를 가진 트윗 10,000개 수집(크롤링)
2. 전처리 (불용어 제거)
3. FastText로 워드임베딩 생성 (skipgram, minCount=15)
4. (ongoing) 추가로 Term Frequency-Inverse Document Frequency (TF-IDF) 기반으로 Word2Vec 구현
5. Nearest neigbors (관련어) 출력
6. (ongoing) 벡터공간 시각화

## 1. 트윗 크롤링
- 가장 먼저 2020년 5월14일-16일 동안 생성된 '코로나19' 키워드를 가진 트윗을 크롤링하기 위해 GetOldTweet3 라이브러리를 사용
- 크롤링 과정에서 HTTP 429 “Too Many Requests” 에러를 방지하기 위해 MaxTweets를 10,000개로 제한

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
- 조사나 접속사와 같은 단어들뿐만 아니라 명사, 형용사와 같은 단어들 중에서 불용어로 제거하고 싶을 경우, 직접 불용어 사전을 만들어 제거

```bash
!pip3 install konlpy    # Python 3.x
from konlpy.tag import Kkma
from konlpy.utils import pprint
kkma = Kkma()
pprint(kkma.nouns(total[3]))
for i in range(len(total)):
  total[i]=" ".join(kkma.nouns(total[i]))

print(total[3])
```

## 3. FastText로 워드임베딩 생성 (skipgram, minCount=10)
- <a href="https://github.com/facebookresearch/fastText" target="_blank">FastText</a>는 Facebook에서 제공하는 라이브러리로 손쉽게 Word Embedding을 생성
- 중심단어로 주변단어(관련단어)를 구할지 또는 주변단어로 중심단어를 예측할지에 따라 Skipgram vs. Cbow 모델을 택일
- 여기서는 Skip-gram 모델을 사용하고, 자주 언급되는 단어 (언급 빈도가 높을 수록 중요도가 높다는 가정하에) 위주로 구하기 위해 minCount (최소언급 횟수)를 15로 설정

```bash
#3. Fasttext로 워드임베딩 생성 (model='skipgram', minCount=10)
!pip install fasttext
import fasttext

# Skipgram model :
model = fasttext.train_unsupervised('/content/drive/My Drive/Colab Notebooks/NLP/4.tweet_analytics/data/output_twcorpus_pp.csv', model='skipgram', minCount=15)

# or, cbow model :
#model = fasttext.train_unsupervised('', model='cbow')
```

## 5. Nearest neigbors (관련어) 출력
- .get_nearest_neighbors() 함수를 통해 근접단어 10개를 출력

```bash
model.get_nearest_neighbors('한국')
```

# 개선사항
- 크롤링 과정에서 HTTP 429 “Too Many Requests” 'Too Many Request 에러 회피방법을 모색해 더 많은 트윗양을 수집/분석
- 전처리 과정에서 불용어 사전을 제작해 무의미한 단어들을 최대한 제거

# 
- 크롤링 과정에서 HTTP 429 “Too Many Requests” 'Too Many Request 에러 회피방법을 모색해 더 많은 트윗양을 수집/분석
- 전처리 과정에서 불용어 사전을 제작해 무의미한 단어들을 최대한 제거
