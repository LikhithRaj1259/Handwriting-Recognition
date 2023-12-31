# Handwriting Recognition


## Introduction
In this repository I used the [NIST Special Database 19](https://www.nist.gov/srd/nist-special-database-19) and [Tensorflow](https://www.tensorflow.org/) to create a convolutional neural network, which recognizes handwritten digits. It differentiates between 47 classes: All uppercase letters, all numbers and a few lower case letters.I was able to achieve an accuracy of about 87%. If you have any questions, please feel free to open an issue, I will respond to all issues and do my best to help.

## Preprocessing the data
I downloaded the database [by_merge.zip](https://s3.amazonaws.com/nist-srd/SD19/by_merge.zip) then used a small python script to put them all in the same folder and renamed them in the format `class_[class]_index_[index].jpeg`. I then used opencv to convert these images into 2 arrays. One array contains the images, rescaled to 32x32 and greyscale with the shape (num_images, 32, 32, 1). The other array contains the label using one hot encoding, so the array has the shape (num_labels, 47).
The script to convert the images can be found in data_handler.main(). After conversion, the arrays are saved as `nist_labels_32x32.npy` and `nist_images_32x32.npy`. The data is now preprocessed and can be fed into the neural network. 

## Data Helper
I have also defined a class `NIST_Handler` which holds the train and test data and returns batches of this data with the functions `get_batch(batch_size)` and `test_batch(batch_size)`. 
It simply loops through the arrays and returns the next `batch size` images and labels.  Before I supply the Class with the images , I shuffled them with a unison shuffle.

## Training
As  loss function I used cross entropy and I chose the Adam Optimizer. I used a learning rate of 0.002, but I did not experiment much, so other values might yield better results. I then trained for 10,000-20,000 epochs, but the accuracy didn't increase much after 10,000 anymore. 
Every 200 epochs, a test batch with size 200 is used to evalute accuracy. These values are arbitrary and can be changed to preference. If you want to train as fast as possible, only evaluate accuracy very rarely and with small batches, but
if you want to get a feel for how your AI is doing and don't mind slightly longer training times, evaluate accuracy often and use big batches.
When training is completed, the model is saved. 

## Predictions
First I reconstruct the same graph that I used for training, then load the saved Variables. The predictions is one-hot encoded, thus
I made a dictionary that simply has the letter or number as value for each label. Refer to predict.py for more information.
