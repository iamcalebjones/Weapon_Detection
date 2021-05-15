# Weapon Detection with Computer Vision
## Using technology to detect weapons in video footage
#### by Caleb Jones
[LinkedIn](https://www.linkedin.com/in/calebsjones/) | [GitHub](https://github.com/iamcalebjones) | [Presentation Slides](https://www.beautiful.ai/player/-M_m0fACA3YtgjsssTaz)

<p align="center">
  <img src="https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/main_pic.png">
</p>

**Warning**: this writeup features images and videos of myself handling a real firearm during testing the model. The weapon was unloaded, and this is also demonstrated in the video.

## Problem

Mass shootings, by definition in America in 2021, are gun-related incidents in which 4 or more people are injured or killed by a firearm. These have happened everywhere from grocery stores to house parties, but the ones we tend to hear about generally involve very public places, frequently schools, concerts, restaurants, shopping centers, and places of worship. The weapons used in these incidents are also widely variable, but the most commonly used weapons are pistols, with 9mm and .22 calibers leading the list. And after every event, the news circuits show headlines about how to deal with it, and politicians make promises about more legislation and tighter restrictions required to obtain a firearm. 

But the question I had was how can technology help? In a culture saturated with technology, surely there must be a use of technology to prevent or limit the impact of these events. In exploring this question, I was brought to the field of computer vision.

## Computer Vision

Computer vision is generally split into 3 sub-categories.
* __Image Classification__: this technique assigns a class label to an image
* __Object Localization__: this technique involves drawing a bounding box around one or more objects in an image
* __Object Detection__: this technique combines these two tasks, drawing a bounding box around each object of interest in an image and giving it a class label

Research in the field of computer vision has seen massive advances in the last 7-8 years. The first models were able to detect an object in an image in about 20 seconds of processing. Now, the processing time is around ~20 milliseconds, allowing up to 50 frames per second of video to be processed, and lightweight versions of these kinds of models are processing videos at a rate of 150 frames per second.

In choosing to proceed with a computer vision task, I needed to pick a framework on which to build my model. Transfer learning would allow me to leverage the architecture and development that's been done before me. I decided to build on the YOLO framework, selecting the most recent release [YOLOv5](https://github.com/ultralytics/yolov5), since it is well known for extremely fast processing times in deployment, and is also very easy to use for training a new model. All I needed now was the data.

## The Data

I found a dataset that was built for exactly a problem like this. It is a collection of images and accompanying annotations with bounding box data and class labels put together for exactly this type of project. The data was provided by the [Andalusian Research Institute](https://dasci.es/) at the University of Grenada, and it contained several weapons classes as well as non-weapon classes for the purpose of variety in training. I focused on the dataset for [pistol dataset](https://github.com/ari-dasci/OD-WeaponDetection/tree/master/Pistol%20detection) to get started and prove concept. 

This dataset contains 3000 images of pistols in a variety of settings and orientations, being held, laying on a table, even cartoon guns were represented. In my reading about the YOLOv5 platform, 100-500 images were recommended for training a model on a new object, so I arbitrarily chose to use a third of the pistol images for my model, because although up to 500 were recommended, less isn't more, more is more. The train/test split that I worked with contained 692 images for training, 198 images for validation, and 99 images for testing.
