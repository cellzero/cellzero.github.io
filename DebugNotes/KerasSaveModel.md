输入形状同模型需求不匹配
============================

使用Keras保存模型结构的小陷阱
----------------------------

> 本文记录了使用Keras保存模型结构所遇到的一个小问题。  
> 即多输入模型，在某项输入同输出未发生联系时，保存再读取后使用引发异常的情况。  
> 以下，首先给出结论，其次列出出问题的代码及简要记录调试的过程。

### 结论

Keras在保存模型结构时，会自动过滤掉同输出无关的输入。

因此，若某模型的输入同输出不产生关系，model.to_json()所得json内容将不存在该输入。

### 错误范例

以下，简要记录会引发错误的情况，并介绍修改方式。

首先，列出会引发如题目所述问题的代码，以及得到的异常信息。

#### 代码、输出及异常信息

会引发异常的代码简化如下：

```python
import numpy as np
from keras.layers import Input, Concatenate, Dense
from keras.models import Model, model_from_json

# create model
input_1 = Input(shape=(1,))
input_2 = Input(shape=(2,))
output = Dense(10)(input_2)

model = Model(inputs=[input_1, input_2], outputs=output, name="sl")

# fake data
x_1 = np.random.rand(10, 1)
x_2 = np.random.rand(10, 2)

# before json
model_json = model.to_json()
print("input before:", model.input_shape)
print("output before:", model.predict([x_1, x_2]).shape)

# after json
model2 = model_from_json(model_json)
print("input after", model2.input_shape)
print("output after", model2.predict([x_1, x_2]).shape)
```

模型简单的接受input_1、input_2两个输入，然后input_2经过全连接层作为输出（注意input_1并未使用）。
随后分别用原始模型、经json保存后的模型进行预测，会发现后者抛出异常。
为方便观察，代码中输出了两个模型的输入层形状。

输出结果如下：

```
input before: [(None, 1), (None, 2)]
output before: (10, 10)
input after (None, 2)
```

可以看出，经json保存后的模型，其输入形状发生了变化：
只保留了同输出相关的输入（此处即input_2）。

异常信息如下：

```
ValueError: Error when checking model : 
the list of Numpy arrays that you are passing to your model is not the size the model expected. 
Expected to see 1 array(s), but instead got the following list of 2 arrays
```

#### 修改方式

只要在模型中使用input_1即可，如：

```python
# create model
input_1 = Input(shape=(1,))
input_2 = Input(shape=(2,))
hidden = Concatenate()([input_1, input_2])
output = Dense(10)(hidden)
```

此时即可正常使用json保存模型结构。

检查两者程序的json串，也可以看到类似的信息。

修改前的json串的输入层记录为：

```json
"input_layers": [["input_1", 0, 0]]
```

修改后的json串的输入层记录为：

```json
"input_layers": [["input_1", 0, 0], ["input_2", 0, 0]]
```

由此可见模型结构保存成功。

### 总结

一般来说，模型中不会出现未使用到的输入的情况（如果存在，此输入可删除）。
该例为正在编写模型结构中，尝试调试代码时偶然发现。
虽真实场景遇到可能极低，还是记录在此，以备查看。
