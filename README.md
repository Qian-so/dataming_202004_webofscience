# dataming_202004_webofscience

The development of the issues that people study in nearly five years in the area of IOT on web of science.

## 导入数据及简单探索

```R
data<-read.csv("test2.csv", header = TRUE)##导入从web of science导出的近五年研究方向论文统计数据
library("ggplot2")
##对于数据进行简单的探索，了解大致分布
boxplot(data$quantity)
plot(data$quantity, data$year)
```

## 数据预处理

```R
##对数据进行处理，选取论文数量大于30以及剔除2020年的数据
newdata <- data[which(data$quantity >"30"&data$year<"2020"),]
##绘制折线图，了解各个研究方向近五年的发展情况
 ggplot(data=newdata, mapping=aes(x=year, y=quantity, group=orientation)) +
  geom_line(aes(color=orientation))+
  geom_point(aes(color=orientation))+
  theme(legend.position="top")

```

## 词频及词云

```R
##统计词频，制作词云
install.packages("wordcloud2")
library("wordcloud2")
mydata<-readLines(con <- file("C:/Users/lenovo/Desktop/2020head.txt",encoding="GB2312"))
engine_new_word<-worker()
##制作用户字典
new_user_word(engine_new_word, c("物联网","结合"))
txt1<-segment(mydata,engine_new_word)
##去除字数小于2的词语
txt2 <- subset(txt1, nchar(txt1)>1)
length(txt2)
##去除停用词
stopwords<-readLines(con <- file("stopwords.txt", encoding = "GB2312"))
for(j in 1:length(stopwords)){txt2 <- subset(txt2,txt2!=stopwords[j])}
length(txt2)
txt3<-table(txt2)
txt3<-txt3[!grepl('[0-9]+',names(txt3))] 
result<-txt3[order(txt3, decreasing = TRUE)]
front <-result[1:100]
front##查看排在前面部分的词频
##绘制词云
wordcloud2(front,color=c(rep(rev(brewer.pal(9, "Blues"))[1:6],each=3),rep("skyblue",40)),
  shape='cardioid',size=0.4,minSize = 0, gridSize =  0,fontFamily = 'Segoe UI', fontWeight = 'bold',
  backgroundColor = "white",minRotation = -pi/4, maxRotation = pi/4, shuffle = TRUE,
  rotateRatio = 0.6, ellipticity = 0.65)
```
