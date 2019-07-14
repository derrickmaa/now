regression
```
from sklearn import svm
import pandas as pd
from sklearn import preprocessing
from sklearn.model_selection import train_test_split


fire = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/forest-fires/forestfires.csv')
fire = pd.DataFrame(fire)
for i in ['month', 'day']:
    # encode the str into label to support sklearn's DT
    fire[i] = preprocessing.LabelEncoder().fit(fire[i]).transform(fire[i])
n_fire = fire.values
feature = n_fire[:, :12]
target = n_fire[:, 12:]
target = target.ravel()
x_train, x_test, y_train, y_test = train_test_split(feature, target, train_size=0.8, random_state=2)
clf = svm.SVR()
clf.fit(x_train, y_train)
predicted = clf.predict(x_test)
v = predicted - y_test
v = pd.DataFrame(v)
print(v)

```
