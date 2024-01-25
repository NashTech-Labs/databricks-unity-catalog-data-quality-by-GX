## DATA QUALITY BY GREAT EXPECTATIONS

Great Expectations is the leading tool for validating, documenting, and profiling your data to maintain quality and improve communication between teams.

### Steps to setup Great Expectations are:

**1.** **Install** the python library in local.   
```sh  
pip install great_expectations  
```   
```sh  
pip install sqlalchemy-databricks 
```  
**2. Creating Data Context:** A Data Context is the primary entry point for the Great Expectations deployment, with configurations and methods for all supporting components.
```sh  
great_expectations init  
```
**3. Connect to Datasource:** We are connecting to Databricks Unity Catalog Data:
```sh  
great_expectations datasource new
``` 
```sh  
connection_string = f"databricks://token:{token}@{host}:{port}/{database}?http_path={http_path}&catalog={catalog}&schema={schema}"
```
**4. Create Expectation** Suite of your Data
```sh  
great_expectations suite new
```  
**5.** If you want to edit or add new expectation run the below command
```sh  
great_expectations suite edit suite_name
```    
**6. Creating Checkpoint:** The next thing we need to do is to apply the expectations to the test data set to validate our new data set. To do that we need to create a checkpoint to execute the created Expectation Suite on the new data set. We run the code below in our terminal
```sh  
great_expectations checkpoint new checkpoint_name
```  
**7.** To Configure Data Hub With Great Expectations Tool, Add the below line of code to product_data_checkpoint.yaml file :    
```
- name: datahub_action  
      action:  
        module_name: datahub.integrations.great_expectations.action  
        class_name: DataHubValidationAction  
        server_url: http://35.120.124.334:8080  
        platform_alias: databricks  
        platform_instance_map: {   
          my_datasource: 5580193376168524.datamesh-2-metastore.main  
        }
        env: PROD 
```
**8.** To view the validation result on Datahub run the checkpoint command
```sh 
great_expectations checkpoint run product_data_checkpoint
```
**9.** On Data Hub Platform you can view the validation result
