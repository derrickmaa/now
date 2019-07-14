```
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn import svm
from sklearn.metrics import confusion_matrix
from mlxtend.plotting import plot_decision_regions


def main():
    wine = np.loadtxt('https://archive.ics.uci.edu/ml/machine-learning-databases/wine/wine.data', delimiter=',')
    feature = wine[:, 1:3]
    target = wine[:, :1]
    target = target.astype(np.int64)
    x_train, x_test, y_train, y_test = train_test_split(feature, target, train_size=0.8, random_state=1)
    clf_s = svm.SVC(kernel='linear')
    clf_s.fit(x_train, y_train.ravel())
    predicted = clf_s.predict(x_test)
    confusion_matrix(y_test, predicted)
    feature_combined = np.vstack((x_train, x_test))
    target_combined = np.vstack((y_train, y_test))
    target_combined = np.reshape(target_combined, (-1))
    plot_decision_regions(feature_combined, target_combined, clf=clf_s)
    plt.xlabel('Alcohol')
    plt.ylabel('Malic acid')
    plt.title('SVM on wines')
    plt.show()


if __name__ == '__main__':
    main()

```
