# IntroNeuralNetworks in Python: A Template Project
## Getting Started

For those of you that do not want to learn about the construction of the model (although I highly suggest you to), clone and download the project, unzip it to your preferred folder and run the following code in your computer.

```bash
pip install -r requirements.txt
python LSTM_model.py
```
It's as simple as that!

## Requirements

For those who want a more details manual, this program is built in Python 3.6. If you are using an earlier version of Python, like Python 3.x, you will run into problems with syntax when it comes to f strings. I do suggest that you update to Python 3.6.

```bash
pip install -r requirements.txt
```

## Stock Price Data

Now we come to the most dreaded part of any machine learning project: data acquisiton and data preprocessing. As tedious and hard as it might be, it is vital to have high quality data to feed into your model. As the saying goes "Garbage in. Garbage out." This is most applicable to machine learning models, as your model is only as good as the data it is fed. Processing the data comes in two parts: downloading the data, and forming our datasets for the model. Thanks to Yahoo Finance API, downloading the stock price data is relatively simple (sadly I doubt not for long). 

To download the stock price data, we use `pandas_datareader` which after a while did not work. So we use this [fix](https://github.com/ranaroussi/fix-yahoo-finance) and use `fix_yahoo_finance`. If this fails (maybe in the near future), you can just download the stock data directly from Yahoo for free and save it as `stock_price.csv`.

## Preprocessing

Once we have the stock price data for the stocks we are going to predict, we now need to create the training and testing datasets.

### Preparing Train Dataset

The goal for our training dataset is to have rows of a given length (the number of prices used to predict) along with the correct prediction to evaluate our model against. I have given the user the option of choosing how much of the stock price data you want to use for your training data when calling the `Preprocessing` class. Generating the training data is done quite simply using `numpy.arrays` and a for loop. You can perform this by running:

```python
Preprocessing.get_train(seq_len)
```

### Preparing Test Dataset

The test dataset is prepared in precisely the same way as the training dataset, just that the length of the data is different. This is done with the following code:

```python
Preprocessing.get_test(seq_len)
```

## Neural Network Models

Since the main goal of this project is to get acquainted with machine learning and neural networks, I will explain what models I have used and why they may be efficient in predicting stock prices. If you want a more detailed explanation of neural networks, check out my blog.

### Multilayer Perceptron Model

A multilayer perceptron is the most basic of neural networks that uses backpropagation to learn from the training dataset. If you want more details about how the multilayer perceptron works, do read this [article](https://medium.com/engineer-quant/multilayer-perceptron-4453615c4337).

### LSTM Model

The benefit of using a Long Short Term Memory neural network is that there is an extra element of long term memory, where the neural network has data about the data in prior layers as a 'memory' which allows the model to find the relationships between the data itself and between the data and output. Again for more details, please read this [article](https://www.altumintelligence.com/articles/a/Time-Series-Prediction-Using-LSTM-Deep-Neural-Networks)

## Backtesting

My backtest system is simple in the sense that it only evaluates how well the model predicts the stock price. It does not actually consider how to trade based on these predictions (that is the topic of developing trading strategies using this model). To run just the backtesting, you will need to run

```python
back_test(strategy, seq_len, ticker, start_date, end_date, dim)
```
The `dim` variable is the dimensions of the data set you want and it is necessary to successfully train the models.

## Stock Predictions

Now that your model has been trained and backtested, we can use it to make stock price predictions. In order to make stock price predictions, you need to download the current data and use the `predict` method of `keras` module. Run the following code after training and backtesting the model:

```python
data = pdr.get_data_yahoo("AAPL", "2017-12-19", "2018-01-03")
stock = data["Adj Close"]
X_predict = np.array(stock).reshape((1, 10)) / 200
print(model.predict(X_predict)*200)
```

## Extensions

As mentioned before, this projected is highly extendable, and here some ideas for improving the project.

### Getting Data

Getting data is pretty standard using Yahoo Finance. However, you may want to look into clustering data in terms of trends of stocks (maybe by sector, or if you want to be really precise, use k-means clustering?).

### Neural Network Model

This neural network can be improved in many ways:
1. Tuning hyperparameters: find the optimal hyperparameters that gives the best prediction 
2. Backtesting: Make the backtesting system more robust (I have left certain important aspects out for you to figure). Maybe include buying and shorting?
3. Try different Neural Networks: There are plenty of options and see which works best for your stocks.

### Supporting Trade

As I said earlier, this model can be used to support trading by using this prediction in your trading strategy. Examples include:
1. Simple long short strategy: you buy if the prediction is higher, and vice versa
2. Intraday Trading: if you can get your hands on minute data or even tick data, you can use this predictor to trade.
3. Statistical Arbitrage: use can also use the predictions of various stock prices to find the correlation between stocks. 
