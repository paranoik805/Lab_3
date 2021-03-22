Лабораторная работа 3.  
====
# Немного информации о цели лабараторной работы
Обучить нейронную сеть с использованием техники обучения Transfer Learning. В данной работе использовалась нейронная сеть EfficientNet-B0.Данная неронная сеть уже предобученна  на базе изображений ImageNet, так как техника Transfer Learning предполагает использование предварительно обученной нейронной сети. В первой части работы использовался фиксированный темп обучения (0.1, 0.01, 0.001, 0.0001). Во второй части работы использовались следующие политики изменения темпа обучения: пошаговое затухание и экспоненциальное затухание.

# 2.С использованием техники обучения Transfer Learning обучить нейронную сеть EfficientNet-B0 (предварительно обученную на базе изображений imagenet) для решения задачи классификации изображений Oregon WildLife с использованием фиксированных темпов обучения 0.1, 0.01, 0.001, 0.0001
* **Описание архитектуры:**   
 
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

 ***Пояснение***
<img src="./graph/com_v1.jpg">


### Анализ результатов:
Исходя из графика метрики точности и графика функции потерь видно, что лучшее значение достигается при темпе обучения 0.001

Из графиков видно, что наилучшее значение на фиксированном темпе обучения - 0.001, на валидационном наборе данных метрика точности = 89.13%, ошибки при этом = 0.2657.
При использовании шага обучения 0.001 достигается наилучшее качество на валидации равное 89,53%

# 3. С использованием техники обучения Transfer Learning. Обучить нейронную сеть EfficientNet-B0 (предобученную на базе изображений imagenet) для решения задачи классификации изображений Oregon WildLife

* **Описание структуры** 

* Использование нейронной сети EfficientNet-B0 с параметрами: include_top=False, input_tensor=inputs, pooling='avg', weights='imagenet'. Параметр include_top=False - использование слоев нейронной сети EfficientNet-B0 без классификации. Параметр input_tensor=inputs - подает входное изображение. Параметр pooling='avg' - применение глобального среднего объединения к выходным данным последнего сверточного слоя. Параметр weights=imagenet - использование предобученного начального приближения на базе imagenet.

```
x = EfficientNetB0(include_top=False, input_tensor=inputs, pooling='avg', weights='imagenet')(inputs) 
```

* "заморозка" сверточной части, так как мы используем уже обученную нейроную сеть:
```
x.trainable = False
```

* Преобразование многомерного тензора в одномерный
```
 x = tf.keras.layers.Flatten()(x.output)
```

* Полносвязный Dense слой с 20 выходами и функцией активации softmax, которая определяет к какой категории и с какой вероятностью относится поданное на вход изображение:  
```
outputs = tf.keras.layers.Dense(NUM_CLASSES, activation=tf.keras.activations.softmax)(x)
```

* Также был изменен формат данных с float32 на unit8, так как для коректной работы Imagenet необходим форма unit8.
```
example['image'] = tf.image.convert_image_dtype(example['image'], dtype=tf.uint8)
```

* В коде была убрана нормализация, так как она включена в модель. 

* Изменение темпов обучения от 0.1 до 0.0001
```
#optimizer=tf.optimizers.Adam(lr=0.1)
#optimizer=tf.optimizers.Adam(lr=0.01)
#optimizer=tf.optimizers.Adam(lr=0.001)
#optimizer=tf.optimizers.Adam(lr=0.0001)
```


 ### Графики обучения для нейронной сети предобученной на базе изображений imagenet:
 
Синяя линия - на валидации  
Оранжевая линия - на обучении  

 ***График метрики точности:*** 
<img src="./epoch_categorical_accuracy v2.svg">

 ***График функции потерь::*** 
 
<img src="./epoch_loss v2.svg">

### Анализ результатов:

Исходя из графика метрики точности и графика функции потерь видно, что сеть обучается, так как с каждой эпохой метрика точности увеличивается, а потери уменьшаются. Так же из графика метрики точности видно, что точность довольно высокая, достигает максимум около 90 процентов. Из чего можно сказать, что результаты предобученной нейронной сети на базе imagenet с использованием техники обучения Transfer Learning лучше, чем нейронной сети со случайным приближением.
