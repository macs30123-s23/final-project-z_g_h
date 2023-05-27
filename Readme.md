# Introduction

In this project, we leveraged different machine learning models to predict the review stars of Amazon products from customers' reviews and the category of the product. The data is collected from the Multilingual Amazon Reviews Corpus (https://registry.opendata.aws/amazon-reviews-ml/) which is freely accessible from the AWS Open Data Registry. We conducted our analysis on English reviews and French reviews and successfully obtained a prediction accuracy of almost 90% on Freanch and English review stars by using a multiclass logistic classifier. 
# Problem

Customer reviews are a crucial indicator of a product's quality and could affect the future demand for a product. Sellers improve their products and services according to the review; buyers decide which product to purchase based on past reviews of a product. The problem of fake and misleading comments could cause many negative impacts on both consumers and businesses. Fake comments might lead customers to buy products that do not meet their expectations. When consumers encounter fake reviews, it becomes difficult to differentiate between genuine and fabricated feedback. This loss of trust undermines the credibility of review platforms and makes it harder for consumers to make informed decisions. Moreover, fake and misleading comment creates a disadvantage for small businesses with limited resources may struggle to compete against larger companies that can afford to manipulate reviews. Fake or misleading reviews can disproportionately affect smaller businesses that heavily rely on positive online reviews to build their customer base and establish trust. Thus, companies such as Amazon that provide online shopping platforms often put lots of resources into identifying and removing fake and misleading comments to protect their platform's reputation. By training a model that could predict a customer's review star, we could speed up and automate the process of reading and evaluating every review on a platform like Amazon. And it could definitely help identify potentially fake or misleading reviews. If a review comment strongly suggests a positive or negative sentiment, but the predicted star rating does not align, it may indicate suspicious activity or manipulation. Furthermore, our trained model could be applied to platforms where customers do not explicitly give a star or rating for the product they review, the predicted review stars could be used to categorize and analyze those reviews efficiently.

# Data and Method

The Multilingual Amazon Reviews Corpus contains reviews in English, Japanese, German, French, Chinese, and Spanish, collected between November 1, 2015 and November 1, 2019. Each observation contains the title and content of a review, the given star rating, the product ID and the product category. Reviews in each language include 210000 observations. To find the optimal classification models, we try to train a multiclass logistic classifier, random forests, and a multinomial naive Bayes classifier and apply cross-validation to determine the optimal parameter for each model. The process of grid search could be extremely time-consuming, so it is essential to use tools from large-scale computing to optimize our computation to reduce our computing time. We set up our work environment in an AWS EMR cluster and use the Pyspark framework to parallelize our computing. To construct our features, we built a pipeline that we first applied RegexTokenizer to the review title and review content then use CountVectorizer to vectorize them. In addition to that, inside the pipeline, we used StringIndexer to categorize the product category and the labels so that they could be fitted into the machine learning model. Furthermore, we implemented TextBlob to calculate the sentiment score of the review body and review title and used the sentiment score as another feature to train our machine learning models.
When using PySpark, the framework will automatically parallelize our computation. More specifically, we used PySpark's machine learning library, MLlib. When we used the pipeline we build to chain multiple transformers to construct our features, operations such as tokenizing text, and converting categorical variables to numeric are performed on each partition of the data in parallel. Similarly, during model training, the algorithm processes partitions of the data across multiple nodes. Moreover, PySpark also parallelizes model selection and hyperparameter tuning processes such as grid search and cross-validation. For each combination of hyperparameters, PySpark can train a model on a different partition of data. Also, for cross-validation, each fold can be evaluated on a different node of the cluster. This makes the hyperparameter tuning and cross-validation processes more efficient.

# Result


