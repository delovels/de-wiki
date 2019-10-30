# NGBoost
<!--<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script> -->
## Natural Gradient

梯度下降通过调整梯度方向上的参数，通过一小段距离，以训练我们的网络。在如何定义“小距离”时出现了一个小问题。在标准梯度下降法中，距离是指参数空间中的欧氏距离。
![两对高斯分布](https://github.com/delovels/de-wiki/raw/master/resource/picture/%E4%B8%A4%E5%AF%B9%E9%AB%98%E6%96%AF%E5%88%86%E5%B8%83.png)

**Key Point**——梯度测量的是改变参数对输出的影响程度。但是，必须在上下文中看到这对输出的影响:第一个分布中+2的移位比第二个分布中+2的移位意味着更多。自然梯度的作用是重新定义我们更新参数的“小距离”。并非所有参数都相等。与其平等地对待每个参数的变化，我们需要根据每个参数的变化对我们网络的整个输出分布的影响程度来衡量它。

##How to do
**当设定得分函数为MLE时才能这么推出以下结论**
>首先，我们定义一种新的距离形式，它对应于基于KL散度的距离，KL散度是一种衡量新分布和旧分布差异的度量。
我们通过定义一个度量矩阵来实现这一点，它允许我们根据一些定制的度量来计算向量的距离。    
对于一个有5个参数的网络，我们的度量矩阵是5x5。为了用metric计算参数delta变化的距离，我们使用下面的方法
```
totaldistance = 0  
    for i in xrange(5):  
        for j in xrange(5):
            totaldistance += delta[i] * delta[i] * metric[i][j]
```
>如果我们的度规矩阵是单位矩阵，这个距离和我们用欧几里得距离是一样的。
然而，大多数时候我们的度规不是单位矩阵。有了度量，我们对距离的测量就可以考虑各种参数之间的关系。
结果是，我们可以用费雪信息矩阵（费雪信息矩阵是KL散度的二阶导数）作为度规，它可以用KL散度来度量delta距离。
现在我们有了一个度量矩阵，它根据给定参数变化时的KL散度来度量距离。
这样，我们就可以计算出标准梯度应该如何缩放。
```
natural_grad = inverse(fisher) * standard_grad  
```

##给出定义
NGBoost算法是一种用于概率预测的有监督的学习方法，从预测条件概率分布y|x随x的函数的角度出发，通过这种方法达到提升（的目的）。即

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large \theta" style="border:none;">
$$\theta$$
$$ P_{\theta }\left (y|x \right ) = f_{\theta }\left ( x\right ) $$


##具体算法实现
![算法实现1](https://github.com/delovels/de-wiki/raw/master/resource/picture/%E7%AE%97%E6%B3%95%E5%AE%9E%E7%8E%B01.png)
![算法实现2](https://github.com/delovels/de-wiki/raw/master/resource/picture/%E7%AE%97%E6%B3%95%E5%AE%9E%E7%8E%B02.png)

##预测流程
![预测流程](https://github.com/delovels/de-wiki/raw/master/resource/picture/%E9%A2%84%E6%B5%8B%E6%B5%81%E7%A8%8B.png)

##Q&A
* Why fit？
