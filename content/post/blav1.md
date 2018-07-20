+++
title = "blavaanやってみよう_1"

date = 2018-07-20T00:00:00
lastmod = 2018-07-20T00:00:00
draft = false

tags = ["R","日記"]
summary = "blavaanというパッケージを使ってみた話."

[header]
image = "headers/blavaan.JPG"
caption = "Image credit: [**Academic**](https://faculty.missouri.edu/~merklee/blavaan/)"


+++

  どうも。     
  
  今日は**blavaan**とよばれるベイズを用いた　
  共分散構造分析の可能なパッケージを紹介します。　
  解説記事にありがちですが、[本家](https://faculty.missouri.edu/~merklee/blavaan/)を確認するのがもっとも  
  学習の近道ですのであしからず。    
  
  もちろんベイズを用いたSEMはMplusでもできるわけです。　
  しかし、blavaanを用いると以下の利点があります。      
  1. blavaanの利点
    1. 情報量基準がいっぱい簡単に出せる
    2. Rでできる
    3．アルゴリズム的な問題をクリアできる→[詳細3pあたり](https://www.jstatsoft.org/article/view/v085i04)
    
  十分な利点を確認したところで、blavaanを使っていきましょう。　
  ちなみにblavaanでは[JAGS](http://mcmc-jags.sourceforge.net/)を用いてMCMCするので
  事前にJAGSをインストールしてrjagsもインストールしときましょう。
  
  
  
  
## インストール

まず、インストール。

```r:install
install.packages("blavaan", dependencies = TRUE)
```

おわり。　　



## CFA

今回はシンプルなCFAについて紹介します。  
  ここではblavaanの先輩、[lavaan](http://lavaan.ugent.be/)の[チュートリアル](http://lavaan.ugent.be/tutorial/cfa.html)を用います。  
  使うのはHolzingerSwineford1939というデータセットです。    
    
  2種類の学校による3因子の能力を測定したデータです。項目は9種類です。  
  詳しくは[ここ](https://www.rdocumentation.org/packages/lavaan/versions/0.6-1.1240/topics/HolzingerSwineford1939)かヘルプを確認。  
  lavaanのチュートリアルで**cfa()**と書いてある部分を**bcfa()**に変えるだけ。
  

```r:bcfa
library(blavaan)

# 3つの因子でモデル特定
HS.model <- ' visual  =~ x1 + x2 + x3      
              textual =~ x4 + x5 + x6
              speed   =~ x7 + x8 + x9 '

# 推定
fit <- bcfa(HS.model, data=HolzingerSwineford1939)

# 結果を見る
summary(fit, fit.measures=TRUE)
fitMeasures(fit)

```

  そんで出した結果が以下の通り。
  
  
```r:result
summary(fit, fit.measures=TRUE)

## blavaan (0.3-2) results of 10000 samples after 5000 adapt/burnin iterations
## 
##   Number of observations                           301
## 
##   Number of missing patterns                         1
## 
##   Statistic                                 MargLogLik         PPP
##   Value                                      -3874.312       0.000
## 
## Parameter Estimates:
## 
## 
## Latent Variables:
##                    Estimate  Post.SD  HPD.025  HPD.975     PSRF
##   visual =~                                                    
##     x1                1.000                                    
##     x2                0.590    0.119    0.354    0.821    1.003
##     x3                0.779    0.131    0.525    1.036    1.004
##   textual =~                                                   
##     x4                1.000                                    
##     x5                1.130    0.069    0.996    1.263    1.002
##     x6                0.941    0.060    0.824    1.058    1.001
##   speed =~                                                     
##     x7                1.000                                    
##     x8                1.232    0.165    0.921    1.557    1.002
##     x9                1.177    0.221    0.789    1.628    1.010
##     Prior     
##               
##               
##  dnorm(0,1e-2)
##  dnorm(0,1e-2)
##               
##               
##  dnorm(0,1e-2)
##  dnorm(0,1e-2)
##               
##               
##  dnorm(0,1e-2)
##  dnorm(0,1e-2)
## 
## Covariances:
##                    Estimate  Post.SD  HPD.025  HPD.975     PSRF
##   visual ~~                                                    
##     textual           0.386    0.081    0.232    0.546    1.003
##     speed             0.245    0.053    0.143    0.351    1.001
##   textual ~~                                                   
##     speed             0.164    0.048    0.076    0.261    1.000
##     Prior     
##               
##  dwish(iden,4)
##  dwish(iden,4)
##               
##  dwish(iden,4)
## 
## Intercepts:
##                    Estimate  Post.SD  HPD.025  HPD.975     PSRF
##    .x1                4.936    0.067    4.803    5.066    1.001
##    .x2                6.088    0.068    5.955    6.222    1.001
##    .x3                2.251    0.065     2.12    2.375    1.000
##    .x4                3.062    0.067    2.931    3.192    1.001
##    .x5                4.342    0.075    4.196    4.488    1.001
##    .x6                2.187    0.064    2.064    2.312    1.001
##    .x7                4.187    0.063    4.066    4.313    1.000
##    .x8                5.528    0.059    5.413    5.641    1.000
##    .x9                5.375    0.058    5.262     5.49    1.000
##     visual            0.000                                    
##     textual           0.000                                    
##     speed             0.000                                    
##     Prior     
##  dnorm(0,1e-3)
##  dnorm(0,1e-3)
##  dnorm(0,1e-3)
##  dnorm(0,1e-3)
##  dnorm(0,1e-3)
##  dnorm(0,1e-3)
##  dnorm(0,1e-3)
##  dnorm(0,1e-3)
##  dnorm(0,1e-3)
##               
##               
##               
## 
## Variances:
##                    Estimate  Post.SD  HPD.025  HPD.975     PSRF
##    .x1                0.594    0.121    0.358    0.831    1.006
##    .x2                1.136    0.106    0.936    1.345    1.001
##    .x3                0.836    0.099    0.644    1.032    1.001
##    .x4                0.384    0.050    0.288    0.482    1.001
##    .x5                0.453    0.060    0.337     0.57    1.000
##    .x6                0.362    0.046    0.276    0.455    1.000
##    .x7                0.829    0.090     0.66     1.01    1.001
##    .x8                0.509    0.095    0.324    0.696    1.002
##    .x9                0.554    0.094    0.365    0.738    1.006
##     visual            0.760    0.151    0.473    1.059    1.006
##     textual           0.963    0.114    0.754    1.195    1.001
##     speed             0.357    0.085    0.198    0.526    1.007
##     Prior     
##   dgamma(1,.5)
##   dgamma(1,.5)
##   dgamma(1,.5)
##   dgamma(1,.5)
##   dgamma(1,.5)
##   dgamma(1,.5)
##   dgamma(1,.5)
##   dgamma(1,.5)
##   dgamma(1,.5)
##  dwish(iden,4)
##  dwish(iden,4)
##  dwish(iden,4)


fitMeasures(fit)

##       npar       logl        ppp        bic        dic      p_dic 
##     30.000  -3738.244      0.000   7647.601   7535.522     29.517 
##       waic     p_waic      looic      p_loo margloglik 
##   7539.155     32.534   7539.212     32.562  -3874.312
```

おわり。　　



## 事前分布の変更

MCMCの長さにくじけたので今回は次の事前分布の変更だけして終わります。  
  事前分布は**dp**によって変更可能です。
  デフォルトの設定は**dpriors()**で確認するか  
  [こちら](https://faculty.missouri.edu/~merklee/blavaan/prior.html)で確認してください。 
  
    
  ぼくはHolzingerSwineford1939というデータセットについて一切知りませんが、  
  今回測定した項目の最大値が異なる場合，正規性の仮定が脅かされることがあるらしいです。  
  今回は最大値が異なるということにしましょう。  
  さてこれは困りました。    
    
  というわけでその点について以下のようにモデルに情報を加えましょう。
  

```r:modified

# 事前分布を操作して最大値を調整
modHS.model <- c(HS.model, 
            ' x1  ~~ prior("dunif(0,4.5)[sd]")*x1
              x2  ~~ prior("dunif(0,5)[sd]")*x2
              x3  ~~ prior("dunif(0,2.5)[sd]")*x3
              x4  ~~ prior("dunif(0,3.5)[sd]")*x4
              x5  ~~ prior("dunif(0,3.5)[sd]")*x5
              x6  ~~ prior("dunif(0,3)[sd]")*x6
              x7  ~~ prior("dunif(0,4)[sd]")*x7
              x8  ~~ prior("dunif(0,5)[sd]")*x8
              x9  ~~ prior("dunif(0,4.5)[sd]")*x9')

# 推定
modfit <- bcfa(modHS.model, data=HolzingerSwineford1939)

# 結果を比較
summary(modfit, fit.measures=TRUE)
blavCompare(fit,modfit)

```

ここでは, 項目の最大値に関する情報を項目ごとに与えました。  
  最大値の半分までの範囲を持つ一様分布を各項目に設定しました。  
  面倒なんでこのコードは自分で確認してませんが、そんな劇的に結果は変わらないと思います。  
  
    
  事前分布を色々変えることで色々なことがblavaanではできます。
  今日はここまで。