Лабораторная работа 3.  
====
# Немного информации о цели лабараторной работы
Обучить нейронную сеть с использованием техники обучения Transfer Learning. В данной работе использовалась нейронная сеть EfficientNet-B0.Данная неронная сеть уже предобученна  на базе изображений ImageNet, так как техника Transfer Learning предполагает использование предварительно обученной нейронной сети. В первой части работы использовался фиксированный темп обучения (0.1, 0.01, 0.001, 0.0001). Во второй части работы использовались следующие политики изменения темпа обучения: пошаговое затухание и экспоненциальное затухание.

# 2.С использованием техники обучения Transfer Learning обучить нейронную сеть EfficientNet-B0 (предварительно обученную на базе изображений imagenet) для решения задачи классификации изображений Oregon WildLife с использованием фиксированных темпов обучения 0.1, 0.01, 0.001, 0.0001 
 
* Изменение темпов обучения от 0.1 до 0.0001
```
#optimizer=tf.optimizers.Adam(lr=0.1)
#optimizer=tf.optimizers.Adam(lr=0.01)
#optimizer=tf.optimizers.Adam(lr=0.001)
#optimizer=tf.optimizers.Adam(lr=0.0001)
```


 ### Графики обучения для нейронной сети со с фиксированным темпом обучения 0.1, 0.01, 0.001, 0.0001:
  
 ***График метрики точности:*** 
<img src="./graph/epoch_categorical_accuracy v1.svg">

 ***График функции потерь:*** 
 
<img src="./graph/epoch_loss v1.svg">

 ***Пояснение:*** 
<img src="./graph/com_v1.jpg">


### Анализ результатов:
Смотря на график метрики точности видно, что лучшее значения, достигаются при использовании темпов обучения 0.001, равное 87.92%, и 0.1, равное 87.32% . Смотря на график функции потерь видно, что минимальные потери были при темпах обучения 0.001 и 0.0001. Исходя из данных результатов можно сказать, что темп обучения 0.001 является оптимальным.


# Реализовать и применить в обучении следующие политики изменения темпаобучения, а также определить оптимальные параметры для каждой политики: Пошаговое затухание (Step Decay) и Экспоненциальное затухание (Exponential Decay)

* **Описание структуры** 




 ### Графики обучения для нейронной сети предобученной на базе изображений imagenet:
 

 ***График метрики точности:*** 
<img src="./epoch_categorical_accuracy v2.svg">

 ***График функции потерь::*** 
 
<img src="./epoch_loss v2.svg">

### Анализ результатов:

Исходя из графика метрики точности и графика функции потерь видно, что сеть обучается, так как с каждой эпохой метрика точности увеличивается, а потери уменьшаются. Так же из графика метрики точности видно, что точность довольно высокая, достигает максимум около 90 процентов. Из чего можно сказать, что результаты предобученной нейронной сети на базе imagenet с использованием техники обучения Transfer Learning лучше, чем нейронной сети со случайным приближением.
