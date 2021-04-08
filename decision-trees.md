# Decision trees

## Libraries

Creates the decision tree

```python
from sklearn.tree import DecisionTreeClassifier
```

These measure the quality of the decision tree

```python
from sklearn.metrics import f1_score, confusion_matrix
```

This plots a given tree in a Jupyter Notebook.

```python
from sklearn.tree import plot_tree
```

## Complete example

```python
# indep_vars: list of columns to be used as independent variables
# dep_vars: list of columns to be used as targets
# data: pandas.DataFrame which contains the necessary data
# random_score: optional. Integer that locks the randomness so the results won't change every time
def decision_tree_full(indep_vars, dep_vars, data, random_score=None):
    df_indep = data[indep_vars]
    df_dep = data[dep_vars]

    tree = DecisionTreeClassifier(random_state=random_score)
    test_size = 0.30  # 30 percent of the data used for testing

    # Split given data in to train- and test data, 70/30
    x_train, x_test, y_train, y_test = train_test_split(
        df_indep,
        df_dep,
        test_size=test_size,
        random_state=random_score
    )

    # Train the model
    tree.fit(x_train, y_train)

    result = tree.predict(x_test)

    # Quality analysis
    f1_result = f1_score(y_test, result, pos_label="above")
    cm_result = confusion_matrix(y_test, result, labels=["above", "below"])

    # Show the tree plot 
    plt.figure(figsize=(15, 8))
    plt.show(
        plot_tree(
            tree,
            feature_names=indep_vars,
            max_depth=6,
            filled=True
        )
    )

    return (
        indep_vars,
        tree,
        f1_result,
        cm_result
    )
```