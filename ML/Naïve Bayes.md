```
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import GaussianNB
from sklearn.metrics import confusion_matrix


"""
features:fixed acidity, volatile acidity, citric acid, residual sugar, chlorides, free sulfur dioxide, 
total sulfur dioxide, density, pH, sulphates, alcohol
results:quality (score between 0 and 10)"""
def main():
    white_wine = np.loadtxt('https://archive.ics.uci.edu/ml/machine-learning-databases/wine-quality/winequality-white.csv', delimiter=';', skiprows=1)
    # import data in to numpy.
    # row[0] is row of the names of features. skip it.
    feature = white_wine[:, :11]  # features
    target = white_wine[:, 11:]  # result
    target = target.astype(np.int64)
    quality = np.unique(target)  # get the all qualities appeared
    x_train, y_test, x_target, y_target = train_test_split(feature, target, train_size=0.85, random_state=3)
    gnb = GaussianNB()
    gnb = gnb.fit(x_train, x_target.ravel())
    predicted = gnb.predict(y_test)
    result = confusion_matrix(y_target, predicted)
    result = pd.DataFrame(result, index=quality, columns=quality)
    print(result)


if __name__ == "__main__":
    main()


```
