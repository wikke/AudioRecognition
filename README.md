# Audio Recognition

This is a audio classification neural network projet. Speech Command dataset is provided by Google and AIY.

**A BUG IS FOUND IN DATASET, file 'bird/3e7124ba\_nohash\_0.wav' has no sound, which cause 'Invalid value encountered in true_divide' Exception when training**

 **The accuracy achieves 0.7539 on validation dataset and categorical_crossentropy achieves 0.81352** with network architecture below,

```
Layer (type)                 Output Shape              Param #   
=================================================================
input_2 (InputLayer)         (None, 16000, 1)          0         
_________________________________________________________________
conv1d_3 (Conv1D)            (None, 320, 64)           3264      
_________________________________________________________________
conv1d_4 (Conv1D)            (None, 6, 64)             204864    
_________________________________________________________________
batch_normalization_2 (Batch (None, 6, 64)             256       
_________________________________________________________________
lstm_2 (LSTM)                (None, 32)                12416     
_________________________________________________________________
dropout_2 (Dropout)          (None, 32)                0         
_________________________________________________________________
dense_2 (Dense)              (None, 30)                990       
=================================================================
Total params: 221,790
Trainable params: 221,662
Non-trainable params: 128
_________________________________________________________________
```

## Trials and Expreience

- Single and double Conv Network is not enough for time series situation, like audio in this project. Only RNN(LSTM) is hard to train, since there are 16000 audio frames in one second. A combination of CNN & RNN works better.(单纯的CNN效果一般，RNN难以训练，因为有16000个ts；结合是更好的选择)
- Data augumentation(But make little difference, which is unexpected)
	- Add noise
	- Drop silence part of audio
(做了添加噪音和移除空白音的数据增强，但是效果变差)
- `dropout` and `recurrent_dropout` make training slow and performance worse.（dropoout和recurrent_dropout参数让训练变得缓慢而差劲）
- GRU perform worse than LSTM in this dataset.
- double LSTM(firse return_sequences=True, second false) works worse.(双层LSTM让效果很差)
- Combination of forward & backword LSTM makes little difference.(正反LSTM然后concatenate并没有让结果更好)