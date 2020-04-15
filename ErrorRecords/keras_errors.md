# Keras错误
## 1. ValueError: expected dense_2 to have shape (2,) but got array with shape (1,)
对keras模型进行编译，当loss是categorical_crossentropy的时候，
```
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])
```
需要将label标签转换成one_hot编码然后进行fit在keras下可以作如下操作：
```
y_train = keras.utils.to_categorical(y_train, num_classes)
model.fit(x=x_train, y=y_train, epochs=ml.epoch, shuffle=True,
          callbacks=[csv_logger, checkpointer, learn_rate, early_stopping],
          validation_split=ml.split, verbose=2))
```
