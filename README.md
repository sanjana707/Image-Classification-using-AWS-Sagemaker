# Image Classification using AWS SageMaker

This is a Dog Breed Classification Model. I have used AWS Sagemaker to train a pretrained model that can perform image classification by using the Sagemaker profiling, debugger, hyperparameter tuning and other good ML engineering practices. I have used the provided dog breed classification data set for this project.

## Project Set Up and Installation
Enter AWS through the gateway in the course and open SageMaker Studio. 
Download the starter files.
Download/Make the dataset available. 

## Dataset
The provided dataset is the dogbreed classification dataset which can be found in the classroom.
The project is designed to be dataset independent so if there is a dataset that is more interesting or relevant to your work, you are welcome to use it to complete the project.

### Access
Upload the data to an S3 bucket through the AWS Gateway so that SageMaker has access to the data. 

## Hyperparameter Tuning
I have chosen the dogbreed classification dataset because the data is rich with information that may be helpful to train the model. I have used the following hyperparameter ranges :
    1. "learning_rate": ContinuousParameter(0.001, 0.1),
    2. "batch_size": CategoricalParameter([32, 64, 128, 256, 512])

## Debugging and Profiling
When the model was trained for the first time, no debugging or profiling was done on it. So, it was difficult to know if the training was done well. Inorder to acheive this, we performed debugging and profiling. 

Training jobs:
![Training-jobs.PNG](Training-jobs.PNG)

For debugging, I created a SMDebug hook and registered it to the model. The rules for the debugger hook configuration included vanishing_gradient, overfit, overtraining, and poor_weight_initialization. 

For profiling, I have used the following rules: LowGPUUtilization, ProfilerReport and the connfiguration: system_monitor_interval_millis=500.

### Results

**Debugging Output**

![Debugging-output.PNG](Debugging-output.PNG)

Profiling and debugging helped in providing the insights of the model training. It helped in keeping tracking of the training process. When the estimator was fit aginst the profiler and debugger configurations it provided all the necessary insights like :

    Uploading - Uploading generated training model
    Completed - Training job completed
    VanishingGradient: NoIssuesFound
    Overfit: NoIssuesFound
    Overtraining: NoIssuesFound
    PoorWeightInitialization: InProgress
    LossNotDecreasing: NoIssuesFound
    Training seconds
    Billable seconds



## Model Deployment

Model was deployed on the ml.g4dn.xlarge instance. The model I was trying to deploy threw some errors. So, I tried to add a separate script (endpoint.py) to solve these errors referencing (https://github.com/htinaunglu/DogBreed-Classification-With-Amazon-Sagemaker)

This is the working endpoint:
![Endpoint.PNG](Endpoint.PNG)

Querying the endpoint:
![Query_results.PNG](Query_results.PNG)

To query the endpoint, just add the path of the image of the dog whose breed to be predicted.

<code>
  with open("dogImages/test/004.Akita/Akita_00244.jpg", "rb") as f:
    payload = f.read()  
</code>


Here, replace <code>dogImages/test/004.Akita/Akita_00244.jpg</code> with the required image path. This payload will then be passed to the predictor and the output will be produced accordingly.
