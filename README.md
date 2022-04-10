# big-mart-sales
 


Problem Statement-

    Build a predictive model and find out the sales of each product at a particular store.
    
    
    
    ![image](https://user-images.githubusercontent.com/76938173/162599667-1e3e6f62-c5f4-45fd-815a-c57a9dc73ebb.png)


Data Description-

    
     •	Item_Identifier: Unique product ID
     •	Item_Weight: Weight of product
     •	Item_Fat_Content: Whether the product is low fat or not
     •	Item_Visibility: The % of total display area of all products in a store allocated to the particular product
     •	Item_Type: The category to which the product belongs
     •	Item_MRP: Maximum Retail Price (list price) of the product
     •	Outlet_Identifier: Unique store ID
     •	Outlet_Establishment_Year: The year in which store was established
     •	Outlet_Size: The size of the store in terms of ground area covered
     •	Outlet_Location_Type: The type of city in which the store is located
     •	Outlet_Type: Whether the outlet is just a grocery store or some sort of supermarket
     •	Item_Outlet_Sales: Sales of the product in the particulat store. This is the outcome variable to be predicted.

      Apart from training files, we also require a "schema" file from the client, which contains all the relevant information about the training files such as:
        Name of the files, Length of Date value in FileName, Length of Time value in FileName, Number of Columns, Name of the Columns, and their datatype.

architecture
 ![image](https://user-images.githubusercontent.com/76938173/162375449-3775f7f4-7cac-4dfa-8c72-f271b62689e6.png)

      
 
Data Validation -
   
   In this step, we perform different sets of validation on the given set of training files.  
   
     1. Name Validation- We validate the name of the files based on the given name in the schema file. We have created a regex pattern as per the name given in the schema file to use for validation. After validating the pattern in the name, we check for the length of date in the file name as well as the length of time in the file name. If all the values are as per requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder."

     2. Number of Columns - We validate the number of columns present in the files, and if it doesn't match with the value given in the schema file, then the file is moved to "Bad_Data_Folder."


     3.	Name of Columns - The name of the columns is validated and should be the same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder".

     4. The datatype of columns - The datatype of columns is given in the schema file. This is validated when we insert the files into Database. If the datatype is wrong, then the file is moved to "Bad_Data_Folder".


     5. Null values in columns - If any of the columns in a file have all the values as NULL or missing, we discard such a file and move it to "Bad_Data_Folder".

Data Insertion in Database-

      1) Database Creation and connection - Create a database with the given name passed. If the database is already created, open the connection to the database. 
   
      2) Table creation in the database - Table with name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the table is already present, then the new table is not created and new files are inserted in the already present table as we want training to be done on new as well as old training files.   
   
      3) Insertion of files in the table - All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has invalid data type in any of the columns, the file is not loaded in the table and is moved to "Bad_Data_Folder".


Model Training -

    1) Data Export from Db - The data in a stored database is exported as a CSV file to be used for model training.
    
    2) Data Preprocessing   
      a) Drop columns not useful for training the model. Such columns were selected while doing the EDA.
      b) Replace the invalid values with numpy “nan” so we can use imputer on such values.
      d) Check for null values in the columns. If present, impute the null values 
      e) Scale the training and test data separately 
      
    3) Clustering - KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters is selected by plotting the elbow plot, and for the dynamic selection of the number of clusters, we are using "KneeLocator" function. The idea behind clustering is to implement different algorithms
       To train data in different clusters. The Kmeans model is trained over preprocessed data and the model is saved for further use in prediction.
       
    4) Model Selection - After clusters are created, we find the best model for each cluster. We are using two algorithms, "Random forest Regressor" and “Linear Regression”. For each cluster, both the algorithms are passed with the best parameters derived from GridSearch. We calculate the Rsquared scores for both models and select the model with the best score. Similarly, the model is selected for each cluster. All the models for every cluster are saved for use in prediction. 
 
 
    
Prediction Data Description- 

      Client will send the data in multiple set of files in batches at a given location. Data will contain predictors in 170 columns.
      Apart from prediction files, we also require a "schema" file from client which contains all the relevant information about the training files such as:
      Name of the files, Length of Date value in FileName, Length of Time value in FileName, Number of Columns, Name of the Columns and their datatype.

 Data Validation-
      
      In this step, we perform different sets of validation on the given set of training files.  
      
       1) Name Validation- We validate the name of the files on the basis of given Name in the schema file. We have created a regex pattern as per the name given in schema file, to use for validation. After validating the pattern in the name, we check for length of date in the file name as well as length of time in the file name. If all the values are as per requirement, we move such files to "Good_Data_Folder" else we move such files to "Bad_Data_Folder". 
       
       2) Number of Columns - We validate the number of columns present in the files, if it doesn't match with the value given in the schema file then the file is moved to "Bad_Data_Folder". 
       
       3) Name of Columns - The name of the columns is validated and should be same as given in the schema file. If not, then the file is moved to "Bad_Data_Folder". 
       
       4) Datatype of columns - The datatype of columns is given in the schema file. This is validated when we insert the files into Database. If dataype is wrong then the file is moved to "Bad_Data_Folder". 
       
       5) Null values in columns - If any of the columns in a file has all the values as NULL or missing, we discard such file and move it to "Bad_Data_Folder".  


Data Insertion in Database -

       1) Database Creation and connection - Create database with the given name passed. If the database is already created, open the connection to the database. 
       
       2) Table creation in the database - Table with name - "Good_Data", is created in the database for inserting the files in the "Good_Data_Folder" on the basis of given column names and datatype in the schema file. If table is already present then new table is not created, and new files are inserted the already present table as we want training to be done on new as well old training files.  
       
       3) Insertion of files in the table - All the files in the "Good_Data_Folder" are inserted in the above-created table. If any file has invalid data type in any of the columns, the file is not loaded in the table and is moved to "Bad_Data_Folder".

Prediction -

     1) Data Export from Db - The data in the stored database is exported as a CSV file to be used for prediction.
     
     2) Data Preprocessing   
        a) Drop columns not useful for training the model. Such columns were selected while doing the EDA.
        b) Replace the invalid values with numpy “nan” so we can use imputer on such values.
        c) Check for null values in the columns. If present, impute the null values.
        d) Scale the training data.
        
     3) Clustering - KMeans model created during training is loaded, and clusters for the preprocessed prediction data is predicted.
     
     4) Prediction - Based on the cluster number, the respective model is loaded and is used to predict the data for that cluster.
     
     5) Once the prediction is made for all the clusters, the predictions along with the original names before label encoder are saved in a CSV file at a given location and the location is returned to the client.


requirements.txt file consists of all the packages that you need to deploy the app in the cloud.

![image](https://user-images.githubusercontent.com/76938173/162376078-d0e91df8-e89c-4e18-9b88-ca03f7b58902.png)

main.py is the entry point of our application, where the flask server starts. 

![image](https://user-images.githubusercontent.com/76938173/162376590-cf1edb79-8763-474c-a5f7-797928f90d94.png)

This is the predictionFromModel.py file where the predictions take place based on the data we are giving input to the model.

![image](https://user-images.githubusercontent.com/76938173/162376665-6b1d18b9-fd54-4fff-98a0-ee026695eb33.png)

manifest.yml:- This file contains the instance configuration, app name, and build pack language.

![image](https://user-images.githubusercontent.com/76938173/162376782-a0361867-8b81-4a8e-a0f4-18cf86ae30d4.png)

Procfile :- It contains the entry point of the app.

![image](https://user-images.githubusercontent.com/76938173/162376837-db80f1e3-2d8e-4bd2-9ac9-23ad92f2ce35.png)

runtime.txt:- It contains the Python version number

![image](https://user-images.githubusercontent.com/76938173/162376872-37b6ae6d-94d7-4d4a-ba29-5c8822543bf4.png)






