## 文件目录结构
我们需要建立如下目录并把对应的文件拷贝到相关目录：

```
Santander-2018
└───python
│   │  *.ipynb
│   │   *.py
└───input
│   │   train_v2.csv
│   │   test_v2.csv
│   │   ...
└───output
    │   submission.csv
    │   ...

feature_extract_v2.py
	用于将原始数据的json转换成csv。
	执行方式： copy到上述的python目录，在命令行执行:
		 python feature_extract_v2.py
	注意为了提高效率这里使用了多线程，而多线程在Windows下面必须在一个main()函数里才能被正确执行，所以只能通过命令行调用（jupyter 会报错）。

GoogleAnalytics_V2.ipynb
	这里的关键点是我们所要预测的revenue是一段时间之后的，也就是说y和X是不同时间段的。在这个notebook里我们参考最后的private leaderboard的设置构建了一份对应的训练数据并完成的end-to-end的feature engineering->tuning->stacking流程。这里的问题是还有大量训练数据没有被利用到，所以我们可以通过调整train_y_end_date来生成不同的训练数据（具有和测试数据相同的时间段，和相同的时间间隔）来训练模型，最后再完成总的ensemble。另外要注意的是，
1) public leaderboard由于使用的是和test X相同的时间段的revenue来验证，所以已经失去了参考的价值
2) test_v2.csv的数据实际上可以，也必须和train_v2.csv合并，作为真正的训练数据（想一想为什么？）