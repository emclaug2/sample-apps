# PyTorch CPU CIFAR-10 Tutorial 

In this hands-on tutorial, we'll go through how to host a pre-trained PyTorch model and the associated data on Algorithmia, learn how to deploy that model into production, and then call the algorithm once it's been deployed to make inferences.

The model itself is trained on CIFAR-10 images using a simple CNN and is based off of the [PyTorch CIFAR-10 Blitz tutorial](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html), but adjusted to save the model using Jit Trace. This way we can save the entire model architecture for inference on Algorithmia. We will be using some sample code that is already created for our algorithm, along with a pre-trained serialized model that we'll be hosting in Algorithmia's data collections. Note, for your own algorithms you can host your model and data anywhere, whether your data is already stored in an Amazon S3 bucket, or in a database, you can connect your algorithm to your data source. For this algorithm, we'll be passing in a single image, however for your algorithms you can use batch processing if that makes more sense.

## Prerequisites 

Clone [this repository](https://github.com/algorithmiaio/sample-apps/) so you have access to the code and data files. 

Alternatively you can simply download the repository as a .zip and get the data and code from `sample-apps/algo-dev-demo/pytorch-jit-cpu`

While we will use the Algorithmia Web IDE in this demo, note that for your own development workflow, you can use the [CLI](https://algorithmia.com/developers/clients/cli/) to deploy your model instead once you've created your algorithm and cloned your repository. By using [management API keys](https://algorithmia.com/developers/algorithm-development/algorithm-management-api/), you can also create an algorithm and do all the deployment from the CLI.


## Upload Your Data To Data Collections

In this demo, we are going to host our data on the Algorithmia platform in [Data Collections](https://algorithmia.com/developers/data/hosted/). 

You'll want to create a data collection to host your saved model and your test data: 


1. Log in to your Algorithmia account and click your avatar which will show a dropdown of choices. Click **"Manage Data"**

2. Then in the left panel on the page of data collection options, go ahead and click **"My Hosted Data"**

3. Click on **“New Collection”** under the “My Hosted Data” section on your data collections page. Let's name ours "pytorch_cifar_model"

4. After you create your collection you can set the read and write access on your data collection. We are going to select **"Private"** since only you will be calling your algorithm in this instance. 

5. Now, let's upload our model into the newly created data collection. You can either drag and drop the files from the folder: [pytorch-cifar-cpu/model](https://github.com/algorithmiaio/sample-apps/raw/master/algo-dev-demo/pytorch-cifar-cpu/model) or you can click **"Drop files here to upload"** from where you stored the repo on your computer.

6. Make another data collection called: "pytorch_cifar_data" and upload the data found in [pytorch-cifar-cpu/data](https://github.com/algorithmiaio/sample-apps/raw/master/algo-dev-demo/pytorch-cifar-cpu/data). This is simply an image that we'll pass in to our algorithm to test it out, and create a runnable example for our algorithm's description page.

## Create Your Algorithm

Now we are ready to deploy our model.

### First Create an Algorithm
1. Click the **"Plus"** icon at the top right of the navbar
2. Let's go through the form together to create our algorithm
3. Click on the purple "Create Algorithm"
4. Give it a name, pick "Python 3", and be sure to select "Standard Execution Env" (all other settings are optional)

Now that you have created your algorithm, you'll get a modal with information about using the CLI and Git. Every algorithm has a Git repo behind it so you can experiment with different I/O in development mode by calling the hash version.

### Add Code Samples
1. Click on the tab **"Source"** and you'll notice boilerplate code.
2. Let's delete that code, and copy and paste the code from the file [demo.py](https://github.com/algorithmiaio/sample-apps/blob/master/algo-dev-demo/pytorch-cifar-cpu/demo.py)
3. Recall our data collection is called "pytorch_cifar_model" and you'll need
to change "YOUR_USERNAME" to your own username: `file_path =
'data://YOUR_USERNAME/pytorch_cifar_model/demo_model.t7'` on line 41.

### Add Dependencies
1. Click the **"Dependencies"** button in the grey navbar.
2. Add Dependencies to the requirements.txt file under the ones that already exist, adding:
```
torch
torchvision
```
Because this is a Python algorithm, the dependencies are loaded into the algorithm via PyPi and pip install. Think of it as a requirements.txt file.

### Code Description
This will be a brief description of the code example including where to load the model. 

Note you always want to initialize the model outside of the apply() function. This way, after the model is initially loaded, subsequent calls will be much faster within that session.

## Test your Model

### Compile Code
1. Click the **"Compile"** button in the top right of the grey navbar
2. Now test your code in the console by passing in the data file we stored in our data collection.

### Developer Center
While the first commit and compile is occuring, this is a good opportunity to introduce where to find the documentation. Welcome to the [Developer Center](https://algorithmia.com/developers/) where you can find documentation on [Python algorithm development](https://algorithmia.com/developers/algorithm-development/languages/python/) and the full documentation on [PyTorch](https://algorithmia.com/developers/model-deployment/pytorch/).

### Pass in user Input
In the web console, paste in: `data://USERNAME/pytorch_cifar_data/sample_image_plane2.jpg`

## Deploy Your Model
We'll cover adding your sample I/O, versioning, release notes, and best practices of creating your algorithms.

## Documentation

- [Python Client Guides](https://algorithmia.com/developers/algorithm-development/languages/python/)
- [Documentation for PyTorch](https://algorithmia.com/developers/model-deployment/pytorch/)
- [Documentation for Scikit-Learn](https://algorithmia.com/developers/model-deployment/scikit/)
- [Working with Data](https://algorithmia.com/developers/data/)

