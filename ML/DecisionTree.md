```
import pandas as pd
import pydotplus
from sklearn.model_selection import train_test_split
from sklearn import tree
from sklearn.metrics import confusion_matrix
from sklearn import preprocessing
from IPython.display import Image
from sklearn.externals.six import StringIO


cars = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data')
cars = pd.DataFrame(cars)
cars.columns = ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'results']
for i in ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'results']:
  # encode the str into label to support sklearn's DT(try to use onehot but faild)
  cars[i] = preprocessing.LabelEncoder().fit(cars[i]).transform(cars[i])
feature = cars.loc[:, ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety']]
target = cars.loc[:, ['results']]
x_train, y_test, x_target, y_target = train_test_split(feature, target, train_size=0.8, random_state=1)
clf = tree.DecisionTreeClassifier(criterion='entropy')
clf = clf.fit(x_train, x_target)
predicted = clf.predict(y_test)
result_matrix = confusion_matrix(y_target, predicted)
result = pd.DataFrame(result_matrix)
result.columns = ['acc', 'good', 'unacc', 'v-good']
result.index = ['acc', 'good', 'unacc', 'v-good']
print(result)


dot_data = StringIO()
tree.export_graphviz(clf, out_file=dot_data)
graph = pydotplus.graph_from_dot_data(dot_data.getvalue())
Image(graph.create_png())

***********************************************************************************************

result:
        acc  good  unacc  v-good
acc      71     0      2       0
good      1    12      0       0
unacc     3     0    236       0
v-good    1     1      0      19


```
