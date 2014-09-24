---
layout: post
title: "RCurl Course 03"
description: "Course on Dataguru"
category: Course
tags: [R, RCurl, Course]
---
{% include JB/setup %}

# 目标

利用正则表达式匹配的方式，从网页中抓取信息。

# 实践

从[新浪财经](http://finance.sina.com.cn/)中找到某股票的历史交易信息。  
(举例：万科A 代码：000002)

```
##导入相应的packages
library(XML)  
library(RCurl)  
library(plyr)  

##使用正则匹配的方法提取网页信息  
##获取某一股票（600157）1998年至今的交易信息  
##使用公式提取匹配记录中的数字  
getNum <- function(x) {  
    pos <- regexpr("[0-9.]+", x)  
    y <- substr(x, pos, (pos + attributes(pos)$match.length - 1))  
    y  
}  

##分四个季度获取该股票1999-2014年的交易记录
year <- 1999:2014  
season <- 1:4  
StockSheet <- data.frame(date=c(), open=c(), High=c(), Close=c(), 
						Low=c(), Volumn=c(), Cat=c())  

##获取网页链接  
urlPre <- "http://vip.stock.finance.sina.com.cn/corp/go.php/vMS_MarketHistory/stockid/000002.phtml?"  
for(y in year) {  
    for(s in season) {  
        url <- paste(urlPre, "year=", y, "&jidu=", s, sep="")  
        if(url.exists(url)) {  
            doc <- getURL(url)  
            con <- strsplit(doc, "\r\n")[[1]]  
            col1 <- con[grep("a target='_blank'", con)+1]  
            ##分别提取日期，开盘等数据，并提取其中数字  
            date <- gsub("\\t|</a>", "", col1)         
            open <- getNum(con[grep("a target='_blank'", con)+3])  
            Hig <- getNum(con[grep("a target='_blank'", con)+4])  
            Cls <- getNum(con[grep("a target='_blank'", con)+5])  
            Low <- getNum(con[grep("a target='_blank'", con)+6])  
            Vol <- getNum(con[grep("a target='_blank'", con)+7])  
            Cat <- getNum(con[grep("a target='_blank'", con)+8])  
            ##将提取结果储存到data.frame  
            sheet <- data.frame(date=date, open=open, High=Hig, Close=Cls,
								Low=Low, Volumn=Vol, Cat=Cat)  
            StockSheet <- rbind.data.frame(StockSheet, sheet)  
        }
    }
}

##将结果按照日期 新-旧的顺序排列
StockSheet <- arrange(StockSheet, desc(date))

```

# 输出部分结果如下
```
head(StockSheet)

        date  open  High Close   Low   Volumn       Cat
1 2014-09-02 9.190 9.250 9.250 9.110 61047052 560099392
2 2014-09-01 9.120 9.190 9.170 9.100 43650716 398975840
3 2014-08-29 9.050 9.150 9.120 9.020 31654038 287416192
4 2014-08-28 9.160 9.180 9.020 9.010 51021812 463319168
5 2014-08-27 9.160 9.260 9.160 9.110 33304074 305748064
6 2014-08-26 9.100 9.220 9.160 9.080 34970732 320373984

```
