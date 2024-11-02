# Data analysis 

Here are projects, which show different sides of data analytics applicating to engineering and otherwise fields. 

Project's list: 

###1. PredictionSolv.
    Time series + parsing
Prediction rate of bank. Collected data from the past, which was prepared for using timeline-learning.
The script collects user reviews data for "СовКомБанк" from the website www.sravni.ru, including:
- the review date,
- rating,
- bank response status,
- whether the problem was resolved. 
It preprocesses the data by converting ratings to numerical values, timestamps to datetime format, and aggregates the data to calculate daily averages and totals for analysis. 
Finally, it splits the dataset into training and testing sets to train a linear regression model for predicting future ratings based on historical trends.

###2. GraphTest.
    Graph DB (neo4j + python)
Analitics of complex social community by graph theory.
The task involves analyzing a dataset of events and participants, which is loaded into a graph database (Neo4j) to explore relationships and community structures. 
After transforming the data, the project imports participant and event nodes, establishes connections, and conducts community detection to visualize interactions and identify small groups within the larger network. 
Finally, a REST service is created in Python to query the graph database using a person's full name, returning the results in either GraphML or JSON format.

###3. Boats.
    Jupyter 
Classified various boat constructions using machine learning techniques. The dataset contains features such as:
- mass,
- velocity,
- number of masts,
- number of wings,
- several classification labels, including stability type and configuration.
Utilized multiple algorithms:
- Logistic Regression,
- Support Vector Classifier (SVC),
- Gradient Boosting Classifier,
- Random Forest Classifier,
After splitting the data into training and testing sets, evaluated the performance of each model based on accuracy metrics, ultimately identifying the best-fit classifiers for predicting different construction characteristics of boats based on their specifications.

###4. Bugers.
    Jupyter
Analysis involved classifying different types of bugs based on features:
- reflection,
- speed,
- brightness,
- volume.
Used a Random Forest Classifier, fine-tuned through Grid Search for optimal parameters.
After training on the original dataset, we assessed feature importance to identify key attributes influencing classification.
Applied the model to a new dataset, successfully predicting bug types and revealing the distribution of classes, demonstrating the effectiveness of Random Forest in this classification task.

###5. Set_of_propellers.
    Jupyter
To leverage machine learning techniques to identify the combination of design parameters that maximizes the thrust of a propeller, showcasing the ability of neural networks to learn from a dataset and generalize beyond it. Steps Involved:
- Dataset Creation. Define Parameters:
Establish the range for blade angle (10 to 80 degrees), number of blades (2 to 6), and radius of blade curvature (-50 mm to 30 mm).
Base Configuration:
Start with a baseline propeller configuration, including a blade angle of 40 degrees, 3 blades, a radius of 0 mm, and an associated thrust value.
Generate Thrust Values:
Use a mathematical function to generate thrust values based on variations in the parameters.
- Construct Parameter Lists.
    Number of Blades:
Created a list of configurations by varying the number of blades from 2 to 6. Adjust the radius and calculate the corresponding thrust based on the number of blades.
    Blade Angle:
Created a list of configurations by varying the blade angle from 10 to 80 degrees, adjusting the thrust values accordingly.
    Radius of Curvature:
Created a list of configurations by varying the radius from -50 mm to 30 mm, calculating the corresponding thrust values based on the new radius.
- Creating DataFrame. Combined the lists of configurations into a structured DataFrame to facilitate easier data manipulation and analysis.
- Data Visualization. Visualized the relationship between thrust values, blade angle, and radius using scatter plots or 3D plots. This step helps in understanding the relationships and identifying potential maxima in thrust values.
- Model Training. Prepared the feature set (blade angle, number of blades, radius) as independent variables and thrust as the dependent variable. Train a machine learning model, such as a linear regression model, on the dataset to learn the relationships.
- Making Predictions. Created a test set with specific parameter values to predict thrust using the trained model. This step evaluates how well the model can generalize to new data.
- Optimization of Parameters
Use optimization techniques to find the parameter configuration that maximizes thrust. This can involve defining an objective function that predicts thrust based on input features and applying an optimization algorithm to explore the parameter space.
- Review Results. Compare the initial parameter configuration with the optimized configuration. Analyze the predicted thrust values to assess improvements and validate the effectiveness of the machine learning model.
Conclusion:
Following these steps allows for an effective application of machine learning to model the relationship between propeller design parameters and thrust values. This systematic approach enables the identification of optimal propeller configurations, demonstrating the capability of machine learning in solving complex engineering challenges.

###6. iris
    Jupiter
The process involves using a neural network classifier, specifically a Decision Tree, to predict the species of flowers from the Iris dataset based on features:
- sepal length,
- sepal width,
- petal length,
- petal width.

First, the dataset is loaded, and the features (X) and species labels (y) are defined. 
The data is then split into training and testing sets, with the model being trained on the training data. 
After training, the model makes predictions on the test set, and the results are evaluated by comparing the predicted species to the actual species. 
Additionally, to generate new flower characteristics for a specific species, random feature values can be created and classified using the trained model, retaining those that match the desired species while discarding others.

###7. mushroom_example
    Jupiter 
This solution addresses a comprehensive task from lesson 3.5 on Stepik, focusing on identifying edible and non-edible mushrooms based on their characteristics. 
The dataset can be accessed at https://stepik.org/media/attachments/course/4852/training_mush.csv, and it includes features:
- cap shape,
- color,
- bruises,
- odor, 
- and more.
The necessary libraries, including pandas and NumPy, are imported, and the dataset is loaded for examination.
Random Forest Classifier from the scikit-learn library is employed for modeling, with hyperparameters tuned using GridSearchCV to optimize performance.
After training, the model's feature importance is visualized to assess which characteristics contribute most to the classification.
Predictions are made on a separate test dataset, and a confusion matrix is generated to compare predicted results against the actual outcomes, helping to quantify the number of edible mushrooms identified correctly.






