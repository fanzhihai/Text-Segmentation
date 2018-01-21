# Text-Segmentation
1、读取数据，并展示数据信息<br>
------
```
# get the data
df = pd.read_csv('F:\Python\data\words_data\house_news.csv',encoding='utf-8')
df.head()
``` 
2、对数据进行预处理，将缺失值删除，将内容转化为列表格式<br>
-------
```
# drop the nan rows
df = df.dropna()
# pick up the content
content = df.content.values.tolist()
```
3、对数据内容进行分词<br>
--------
```
# split words
words=[]
for line in content:
    try:
        word = jieba.lcut(line)
        for wd in word:
            if len(wd) > 1 and wd != '\r\n':
                words.append(wd)
    except:
        print(line)
        continue
```
4、分词结束，对分词片段进行统计<br>
--------
```
# calculate words
words_state = words_df.groupby(by=['words'])['words'].agg({'count':np.size})
words_state = words_state.reset_index().sort_values(by=['count'],ascending=False)
words_state.head(10)
```
5、将分词片段在某图片做背景，以词云形式展示<br>
---------
```
# use a picture as background
from scipy.misc import imread
from wordcloud import ImageColorGenerator
bing = imread('F:\Python\data\words_data\house_fenghuang.jpg')
wordcloud = WordCloud(background_color='white',mask=bing,
                      font_path='F:\Python\data\words_data\simhei.ttf',
                     max_font_size=200)
word_count = {x[0]:x[1] for x in words_state.head(1000).values}
wordcloud = wordcloud.fit_words(word_count)
bingColor = ImageColorGenerator(bing)
plt.axis('off')
plt.imshow(wordcloud.recolor(color_func=bingColor))
import matplotlib
matplotlib.rcParams['figure.figsize'] = (20.0, 10.0)
```
6、最终结果<br>
---------
![]()
