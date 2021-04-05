# K-Means


## Libraries

```python
from sklearn.cluster import KMeans
```

## Implementation

### Creating and fitting using [sklearn.cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) and [KMeans.fit()](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html#sklearn.cluster.KMeans.fit)

```python
# k: the amount of clusters
# data: pandas.DataFrame containing your data
def fit(k, data):
    model = KMeans(n_clusters=k)
    model.fit(data)
    return model
```

### Getting the [sklearn.cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) and [KMeans.fit()](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html#sklearn.cluster.KMeans.fit)

```python
# k: the amount of clusters
# data: pandas.DataFrame containing your data
def fit(k, data):
    model = KMeans(n_clusters=k)
    model.fit(data)
    return model
```

### Calculating a model's inertia 

Docs: [sklearn.cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)

```python
# k: the amount of clusters
# data: pandas.DataFrame containing your data
def calculate_inertia(k, data):
    model = KMeans(n_clusters=k)
    model.fit(data)
    return model.inertia_
```

### Calculating the inertias for models with k = 1 through N 

Docs: [sklearn.cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)

```python
# max_k: the amount of models to calculate inertia for
# data: pandas.DataFrame containing your data
def calculate_inertias(max_k, data):
    inertias = [] # This could be cleaner using yield 
    for k in range(1, max_k + 1):
        model = KMeans(n_clusters=k)
        model.fit(data)
        inertias.append(model.inertia_)
    return inertias # inertias now contains a list of max_k items, containing their respective inertias
```


### Elbow plot

```python
# ks: list (or iterable) of all k-values (in order)
# inertias: list of all inertias (in order)
def render_elbow(ks, inertias): 
    plt.plot(ks, inertias, 'bx-')
    plt.xlabel('k')
    plt.ylabel('Inertia')
    plt.title('The Elbow Method showing the inertias for each k') 
```

### Getting the model's labels

Docs: [sklearn.cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)

```python
# model: sklearn.cluster.KMeans whose labels to get
def get_labels(model):
    return list(set(model.labels_))
```

### Labelling the data

Docs: [sklearn.cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)

```python
# model: sklearn.cluster.KMeans whose labels to apply
# data: pandas.DataFrame to add the labels to
def label_data(model, data):
    data["kmeans_label"] = model.labels_
    return data
```

### Creating a crosstab between the k-means-clusters and some other column

Docs: [pandas.crosstab](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.crosstab.html)

```python
# data: pandas.DataFrame whose data to crosstab
# kmeans_column: name of the column with the k-means labels
# other_column: name of the column with which to compare the k-means
def create_crosstab(data, kmeans_column, other_column):
    return pd.crosstab(
        data[kmeans_column], 
        data[other_column]
    )
```

### Predicting the labels for new data

```python
# model: KMeans-model (already fit)
# new_data: data you want the labels for
def predict_clusters(model, new_data):
    return model.predict(new_data) # returns a list of labels.
```