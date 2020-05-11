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

Тем не менее, такие показатели не являются высокими, мы все еще можем наблюдать неуверенность сети на некоторых данных из in-distribution и ошибочно высокие confidence для данных из out-of-distribution. Второе обуславливается тем, что некоторые классы в датасетах пересекаются.
 
Для лучших результатов при обучении сети следует добавить дополнительную аугментацию изображений и взять больше слоев в нейронной сети. 
