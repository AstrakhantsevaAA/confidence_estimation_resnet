# Learning Confidence for Out-of-Distribution Detection in Neural Networks
# ResNet

В работе представлена нейронная сеть ResNet18, обученная на датасете CIFAR-10. К архитектуре ResNet была добавлена дополнительная ветвь confidence branch, которая позволяет выявлять данные (out-of-distribution) не принадлежащие к датасету на которых обучалась сеть (in-distribution). Результатом работы данной ветви является число от 0 до 1, которое отображает уверенность сети в том, что данные принадлежат in-distribution. 
 
На графике можно увидеть, что без confidence branch уверенность сети для обоих датасетов находится в области от 0.4 до 0.6 с выборочным средним 0.5 и модой 0.4, это говорит о том, что сеть не уверена к какому распределению отнести данные.
 
![plot_baseline]

[plot_baseline]: https://github.com/AstrakhantsevaAA/confidence_estimation_resnet/blob/master/plots/resnet_0.0.pth_confidence_hist_confidence.png
 
 
На графике с confidence branch видно, что сеть хорошо научилась определять данные из in-distribution и не плохо определяет данные из out-of-distribution, их выборочные средние равны 0.78 и 0.4, а моды 0.99 и 0.1, соответственно. 
 
![plot_confidence]

[plot_confidence]: 
 https://github.com/AstrakhantsevaAA/confidence_estimation_resnet/blob/master/plots/resnet_0.3.pth_confidence_hist_confidence.png
 
Также, для сравнения, посмотрим на метрики. По всем метрикам сеть с confidence branch превосходит сеть без нее.

| metric                            | baseline      | Confidence branch |
| ----------------------------------|:-------------:| -----:|
| TPR95 (lower is better)           | 0.9640401606425676 | 0.7911086021505364 |
| Detection error (lower is better) | 0.494249      | 0.2405 |
| AUROC (higher is better)          | 0.48557408      | 0.81650198|
| AUPR_IN (higher is better)        | 0.5002453961255687     | 0.8318190388701076|
| AUPR_OUT (higher is better)       | 0.486528327815796     | 0.7696301616704235|

Тем не менее, такие показатели не являются высокими, мы все еще можем наблюдать неуверенность сети на некоторых данных из in-distribution и высокие confidence для данных из out-of-distribution. Второе обуславливается тем, что некоторые классы в датасетах пересекаются.
 
Для лучших результатов при обучении сети следует добавить дополнительную аугментацию изображений и взять больше слоев в нейронной сети. 

Возьмём случайные картинки из интернета и проверим качество работы нашей сети. Результаты не плохие, первые четыре картинки были взяты не из in-distribution, последняя картинка была взята специально максимально похожей на картинки из in-distribution, сеть с высокой уверенностью узнала картинку и отнесла ее в in-distribution. 

<img width="300" alt="pic1" src="https://github.com/AstrakhantsevaAA/confidence_estimation_resnet/blob/master/internet_pics_result/result_pic1.png">

<img width="300" alt="pic2" src="https://github.com/AstrakhantsevaAA/confidence_estimation_resnet/blob/master/internet_pics_result/result_pic2.png">

<img width="300" alt="pic3" src="https://github.com/AstrakhantsevaAA/confidence_estimation_resnet/blob/master/internet_pics_result/result_pic3.png">

<img width="300" alt="pic4" src="https://github.com/AstrakhantsevaAA/confidence_estimation_resnet/blob/master/internet_pics_result/result_pic4.png">

<img width="300" alt="pic5" src="https://github.com/AstrakhantsevaAA/confidence_estimation_resnet/blob/master/internet_pics_result/result_pic5.png">

Для остальных картинок она тоже дала верные confidence, разве что в 4-й картинке со Стичем она завысила confidence до 0.66, возможно приняла его за собаку. Но с threshold = 0.85 картинка верно отправляется в out-of-distribution.
