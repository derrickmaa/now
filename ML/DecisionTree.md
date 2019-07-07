```
import pandas as pd
import pydotplus
from sklearn.model_selection import train_test_split
from sklearn import tree
from sklearn.metrics import confusion_matrix
from sklearn import preprocessing
from IPython.display import Image
from sklearn.externals.six import StringIO


def DT(feature, target): # create the DT
    x_train, y_test, x_target, y_target = train_test_split(feature, target, train_size=0.8, random_state=1) # 80%train 20%test
    clf = tree.DecisionTreeClassifier(criterion='entropy')
    clf = clf.fit(x_train, x_target)
    predicted = clf.predict(y_test)
    result_matrix = confusion_matrix(y_target, predicted)
    result = pd.DataFrame(result_matrix)
    return result


def main():
    cars = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/car/car.data')
    cars = pd.DataFrame(cars)
    cars.columns = ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'results']
    for i in ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'results']:
    # encode the str into label to support sklearn's DT (onehot can not be easily used in DecisionTree)
        cars[i] = preprocessing.LabelEncoder().fit(cars[i]).transform(cars[i])
    feature = cars.loc[:, ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety']]
    target = cars.loc[:, ['results']]
    result = DT(feature, target)
    result.columns = ['acc', 'good', 'unacc', 'v-good']
    result.index = ['acc', 'good', 'unacc', 'v-good']
    print(result)


if __name__ == '__main__':
    main()


"""create the image of DT"""
dot_data = StringIO()
tree.export_graphviz(clf, out_file=dot_data)
graph = pydotplus.graph_from_dot_data(dot_data.getvalue())
Image(graph.create_png())

```
