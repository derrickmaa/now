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
    result = confusion_matrix(y_test, predicted)
    result = pd.DataFrame(result)
    result.columns = ['wine1', 'wine2', 'wine3']
    result.index = ['wine1', 'wine2', 'wine3']
    print(result)
    feature_combined = np.vstack((x_train, x_test))
    target_combined = np.vstack((y_train, y_test))
    target_combined = np.reshape(target_combined, (-1))
    fig = plt.figure(figsize=(16, 9))
    plot_decision_regions(feature_combined, target_combined, clf=clf_s)
    plt.xlabel('Alcohol')
    plt.ylabel('Malic acid')
    plt.title('SVM on wines')
    plt.show()


if __name__ == '__main__':
    main()

```
result
       wine1  wine2  wine3
wine1     12      0      2
wine2      0     13      0
wine3      2      2      5

データの特徴は13個あるが、イメージはその中のAlcoholとMalic acidを選んでする。データ数は178で、トレーニングとテストの比例は8:2。汎化性能は83%。今回のデータ数と選んだ特徴数は少ないから、汎化性能が悪くなると思ったが、データがよいから、あまり悪くなさそうだ。しかし、テストデータ数が少ないので、これは完全に汎化性能とすることができないと思う。もしデータ数がもっと多かったらほうがいいと考える。特徴数が増えたら、汎化性能が上がったが、可視化するため、こちらは2個のみを選んだ。
特徴：
1) Alcohol 
2) Malic acid 
3) Ash 
4) Alcalinity of ash 
5) Magnesium 
6) Total phenols 
7) Flavanoids 
8) Nonflavanoid phenols 
9) Proanthocyanins 
10)Color intensity 
11)Hue 
12)OD280/OD315 of diluted wines 
13)Proline