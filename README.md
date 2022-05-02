# Cement Strength Prediction

## Problem Statement
    
    To build a regression model to predict the concrete compressive strength based on the different features in the training data.
    
## Data Description
    
    Given is the variable name, variable type, the measurement unit and a brief description. 
    The concrete compressive strength is the regression problem.

| Name                             | Data Type    | Measurement        | Description                                                                                                                                                                                                                                                                                                                                                                         |
|----------------------------------|--------------|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cement (component 1)             | quantitative | kg in a m3 mixture | Input Variable                                                                                                                                                                                                                                                                                                                                                                      |
| Blast Furnace Slag (component 2) | quantitative | kg in a m3 mixture | Input Variable-- Blast furnace slag is a nonmetallic coproduct produced in the process. It consists primarily of silicates, aluminosilicates, and calcium-alumina-silicates                                                                                                                                                                                                         |
| Fly Ash (component 3)            | quantitative | kg in a m3 mixture | Input Variable- it is a coal combustion product that is composed of the particulates (fine particles of burned fuel) that are driven out of coal-fired boilers together with the flue gases.                                                                                                                                                                                        |
|                                  |
| Water (component 4)              | quantitative | kg in a m3 mixture | Input Variable                                                                                                                                                                                                                                                                                                                                                                      |
| Superplasticizer (component 5)   | quantitative | kg in a m3 mixture | Input Variable--Superplasticizers (SP's), also known as high range water reducers, are additives used in making high strength concrete. Their addition to concrete or mortar allows the reduction of the water to cement ratio without negatively affecting the workability of the mixture, and enables the production of self-consolidating concrete and high performance concrete |
| Coarse Aggregate (component 6)   | quantitative | kg in a m3 mixture | Input Variable-- construction aggregate, or simply "aggregate", is a broad category of coarse to medium grained particulate material used in construction, including sand, gravel, crushed stone, slag, recycled concrete and geosynthetic aggregates                                                                                                                               |
| Fine Aggregate (component 7)     | quantitative | kg in a m3 mixture | Input Variable—Similar to coarse aggregate, the constitution is much finer.                                                                                                                                                                                                                                                                                                         |
| Age                              | quantitative | Day (1~365)        | Input Variable                                                                                                                                                                                                                                                                                                                                                                      |
| Concrete compressive strength    | quantitative | MPa                | Output Variable                                                                                                                                                                                                                                                                                                                                                                     |

    
## Data Validation
    
    In This step, we perform different sets of validation on the given set of training files.
    
    Name Validation: We validate the name of the files based on the given name in the schema file. We have 
    created a regex patterg as per the name given in the schema fileto use for validation. After validating 
    the pattern in the name, we check for the length of the date in the file name as well as the length of time 
    in the file name. If all the values are as per requirements, we move such files to "Good_Data_Folder" else
    we move such files to "Bad_Data_Folder."
    
    Number of Columns: We validate the number of columns present in the files, and if it doesn't match with the
    value given in the schema file, then the file id moves to "Bad_Data_Folder."
    
    Name of Columns: The name of the columns is validated and should be the same as given in the schema file. 
    If not, then the file is moved to "Bad_Data_Folder".
    
    The datatype of columns: The datatype of columns is given in the schema file. This is validated when we insert
    the files into Database. If the datatype is wrong, then the file is moved to "Bad_Data_Folder."
    
    Null values in columns: If any of the columns in a file have all the values as NULL or missing, we discard such
    a file and move it to "Bad_Data_Folder".
    
## Data Insertion in Database
     
     Database Creation and Connection: Create a database with the given name passed. If the database is already created,
     open the connection to the database.
     
     Table creation in the database: Table with name - "Good_Data", is created in the database for inserting the files 
     in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the table is already
     present, then the new table is not created and new files are inserted in the already present table as we want 
     training to be done on new as well as old training files.
     
     Insertion of file in the table: All the files in the "Good_Data_Folder" are inserted in the above-created table. If
     any file has invalid data type in any of the columns, the file is not loaded in the table and is moved to 
     "Bad_Data_Folder".
     
## Model Training
    
     Data Export from Db: The data in a stored database is exported as a CSV file to be used for model training.
     
     Data Preprocessing: 
        Check for null values in the columns. If present, impute the null values using the KNN imputer.
        
        Check if any column has zero standard deviation, remove such columns as they don't give any information during 
        model training.
        
     Clustering: KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters 
     is selected

# Local Development Setup

## Clone this Git.
```
git clone https://github.com/Samm-G/Cement-Strength-Prediction.git
```

## Create Conda Environment.
(Git Bash in project folder)
```
conda create -p <path_of_new_conda-venv> python==3.6.9 -y
```
(Open CMD in the main git-repo folder)
```
conda activate <path_of_new_conda-venv>
```
```
pip install -r requirements.txt
```

## To update requirements.txt
```buildoutcfg
pip freeze > requirements.txt
```

## Initialize Your Own Git Repo
```
rm -rf .git

git init
git add .

git commit -m "first commit"
git branch -M main

git remote add origin <github_url>
git push -u origin main
```

## Add to .gitignore
```
echo "**/__pycache__/" >> .gitignore
```

## To update your Modifications
```
git add .
git commit -m "proper message"
git push 
```

## Remove Py-Cache:
```
find . | grep -E "(__pycache__|\.pyc|\.pyo$)" | xargs rm -rf
```

## Application Entry Point:
```
python main.py
```


# Build Local Docker Image:

## Docker Login:
```
docker login -u $DOCKERHUB_USER -p $DOCKER_HUB_PASSWORD_USER docker.io
```

## Create a file "Dockerfile" with below content

```
FROM python:3.7
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
ENTRYPOINT [ "python" ]
CMD [ "main.py" ]
```

## Build Docker Image:
```
docker build -t <docker_image_name>:latest .
```

## See Docker Images:
```
docker images
```

## Run Image on local.:
```
docker run -p 5000:5000 <docker_image_name>
```


# Deployment:

## GCloud Deployment: (TODO)

    
    Create G Cloud Console Project:
        cloud.google.com > Console > IAM & Admin > Manage Resources > Create Project
        Give Project Name > Create

    Dashboard:
        Go to Menu > App Engine > Dashboard
        Select current project in top bar.
        Start Tutorial > Python
        Follow the Tutorial.. Select correct Project Name, and enter given commands.

    Download GCloud CLI:
        Follow Instructions:
            https://cloud.google.com/sdk/docs/install

    Open cmd:
        cd <project_root>
        
        Create new Config and set :
            gloud init
            2
            3
            <Give New account email to to create config>
            <Set Current project in choice to this one.>
        
        gcloud app deploy app.yaml --project <project_name>
        <Select any region to deploy>
        y

        Configure Access:
            Copy Access URL and launch
            Enable API (Enable Billing)
            Add Billing Info.
        
        gcloud app deploy app.yaml --project <project_name>
        <Select any region to deploy>
        y

        <Open Given URL and Access from Browser>


        
    





    
    


