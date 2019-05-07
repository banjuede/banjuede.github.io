---


---

<pre><code>from sklearn.datasets import load_iris  
import pandas as pd  
import featuretools as ft  
from featuretools import variable_types  
from featuretools import selection  
  
  
# iris = load_iris()  
# df = pd.DataFrame(iris.data, columns=iris.feature_names)  
# df['species'] = iris.target  
# df['species'] = df['species'].map({0: 'setosa', 1: 'versicolor', 2: 'virginica'})  
  
df = pd.read_csv("C:\\Users\\lumin\\Desktop\\bank_data.csv")  
  
es = ft.EntitySet(id='bank_data')  
variable_type = {  
    "age": variable_types.Numeric,  
    "day": variable_types.Numeric,  
    "job": variable_types.Id,  
    "marital": variable_types.Categorical,  
    "housing": variable_types.Boolean,  
    "loan": variable_types.Boolean,  
    "time1": variable_types.Datetime,  
    "time2": variable_types.Datetime  
}  
es.entity_from_dataframe(entity_id='data', dataframe=df, variable_types=variable_type, make_index=True, index='index')  
print(es.entity_dict)  
# DFS  
feature_matrix, feature_defs = ft.dfs(entityset=es, target_entity="data", agg_primitives=['mode'],  
                                      ignore_variables={"data": []}, return_variable_types="all",  
                                      trans_primitives=[ft.primitives.SubtractNumeric(False), "diff"],  
                                      max_depth=1, features_only=False, max_features=-1)  
ft.primitives.AggregationPrimitive  
print(feature_matrix.shape)  
feature_matrix.to_csv("result.csv")  
print(feature_matrix.columns)  
print(feature_defs)  
  
# ft.save_features(feature_defs, 'features.txt')  
#  
# feature_defs = ft.load_features('features.txt')  
#  
# # Calculate the feature matrix and save  
# feature_matrix = ft.calculate_feature_matrix(feature_defs,  
#                                              entityset=es,  
#                                              n_jobs=1,  
#                                              verbose=0)  
#  
# feature_matrix.to_csv("feature_matrix.csv")  
# print(feature_matrix.shape)  
# print(feature_matrix.head(2))  
  
selection.remove_low_information_features()
</code></pre>

