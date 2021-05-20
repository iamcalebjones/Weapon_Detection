# Weapon Detection with Computer Vision
## Using computer vision to detect weapons in video footage
#### by Caleb Jones
[LinkedIn](https://www.linkedin.com/in/calebsjones/) | [GitHub](https://github.com/iamcalebjones) | [Presentation Slides](https://www.beautiful.ai/player/-M_m0fACA3YtgjsssTaz)

<p align="center">
  <img src="https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/main_pic.png">
</p>

**Warning**: This writeup features images and videos of me handling a real firearm during testing of the model. The weapon was unloaded during the demonstrations, which is also captured in the video.

## Problem

Mass shootings, by definition in America in 2021, are gun-related incidents in which 4 or more people are injured or killed by a firearm. These have happened everywhere from grocery stores to house parties, but the ones we tend to hear about generally involve very public places, frequently schools, concerts, restaurants, shopping centers, and places of worship. The weapons used in these incidents are also widely variable, but the most commonly used weapons are pistols, with 9mm and .22 calibers leading the list. And after every event, the news circuits show headlines about how to deal with it, and politicians make promises about more legislation and tighter restrictions required to obtain firearms.

But the question I had was what role can technology play in fighting this problem? In a culture saturated with technology, surely it has a role in preventing these events. In exploring this question, I was brought to the field of computer vision.

## Computer Vision

Computer vision is generally split into 3 sub-categories.
* __Image Classification__: this technique assigns a class label to an image
* __Object Localization__: this technique involves drawing a bounding box around one or more objects in an image
* __Object Detection__: this technique combines these two tasks, drawing a bounding box around each object of interest in an image and giving it a class label

Research in the field of computer vision has seen massive advances in the last 7-8 years. The first models were able to detect an object in an image in about 20 seconds of processing. Now, the processing time is around ~20 milliseconds, allowing up to 50 frames per second of video to be processed, and lightweight versions of these kinds of models are processing videos at a rate of 150 frames per second.

In choosing to proceed with a computer vision task, I needed to pick a framework on which to build my model. Transfer learning would allow me to leverage the architecture and development that's been done before me. I decided to build on the YOLO framework, selecting the most recent release [YOLOv5](https://github.com/ultralytics/yolov5), since it is well known for extremely fast processing times in deployment, and is also very easy to use for training a new model. All I needed now was the data.

## The Data

I found a dataset that was built for exactly a problem like this. It is a collection of images of a host of weapons and accompanying annotations with bounding box data and class labels, which was put together for exactly this type of project. The data was provided by the [Andalusian Research Institute](https://dasci.es/) at the University of Grenada, and it contained several weapons classes as well as non-weapon classes for the purpose of variety in training. I focused on the dataset for [pistols](https://github.com/ari-dasci/OD-WeaponDetection/tree/master/Pistol%20detection) to get started and prove a concept. 

This dataset contains 3000 images of pistols in a variety of settings and orientations, which included being held, laying on a table, and even cartoon guns were represented. In my reading about the YOLOv5 platform, 100-500 images were recommended for training a model on a new object, so I arbitrarily chose to use a third of the pistol images for my model, because although up to 500 images were recommended, less isn't more, more is more. The train/test split that I worked with contained 692 images for training, 198 images for validation, and 99 images for testing.

## Training the Model

I used Google's Colaboratory cloud computing environment for my training runs due to the fact that it is free while still providing access to GPU services for much faster training. I also decided to train a few models on different versions of the data, described below.
* __Standard Model__: this version of the data was just the raw images with no augmentations or resizing applied in preprocessing
* __Resized Model__: this version of the data contained the images resized to 416x416 pixels, with white borders applied to the spaces in rectangular images
* __Augmented Model__: this version of the data contained the images resized and several augmentations as well: horizontal and vertical flipping, and +/- 30-degree horizontal and vertical shear

## Model Results

Out of the three models, the standard model did the best with a plain background, but the resized and augmented models did the best with a busy background. First, a few images for comparison.

<p align="center">
  <img src="https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/demo_1.png">
</p>

For this image, the standard model did not detect the pistol, and the resized and augmented models detected with only 59% and 61% confidence, respectively. After a few more tests, I realized that the busy background increased the difficulty of the models to see the weapon.

<p align="center">
  <img src="https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/demo_2.png">
</p>

Again in this image, the standard model did not see the pistol, but neither did the other two for that matter, detecting objects that were not pistols as pistols.

<p align="center">
  <img src="https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/demo_3.png">
</p>

For this third image, all three models detected the pistol, with the standard model having the highest confidence at 85%. The resized model falsely detected my wristwatch as a pistol, and the augmented model was only 67% confident in this detection.

Still images are fine, and a fast way to test out a model, but I wanted to see this in action on video. Below are some tests of the standard model on a few household items, namely a pasta spoon, a grilling spatula, a set of wire strippers, and a hammer. On the pistol demonstration, I show that the weapon is empty before proceeding, and **in part of my handling of the weapon, the pistol is pointed directly at the camera**, because I wanted to demonstrate the model's ability to detect all orientations of the weapon.

| Spoon | Spatula | Wire Strippers | Hammer |
| -- | -- | -- | -- |
![](https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/object_test_spoon.gif) | ![](https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/object_test_spatula.gif) | ![](https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/object_test_wire_strippers.gif) | ![](https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/object_test_hammer.gif) |

In the video clips, we can see that although there are a few false detections of the test objects, they are generally brief and low confidence. At one point, the wire strippers got up to 90% confidence for a moment, but the confidence levels jumped around a lot.

And now the demonstration with the pistol, again on the standard model.

<p align="center">
  <img src="https://github.com/iamcalebjones/Weapon_Detection/blob/main/demos/object_test_pistol.gif">
</p>

For the pistol the confidence levels were above 90% for the majority of the clip, and only dropped significantly when the pistol was turned to some less common orientations that were not strongly represented in the test data, like upside-down. This kind of performance is exactly what I had hoped to see from a detection model at the outset of this project.

## Application and Further Development

In society nowadays, security cameras are everywhere, although their use is generally intended to limit loss to businesses in the event of theft or accidents. By leveraging these advances in computer vision though, current security systems could be turned from passive into active systems, and incorporated as a part of an early warning system in public places where shootings have tended to occur. By sounding an alarm and alerting emergency personnel, this would prevent or limit the impact of the next attempted mass shooting as soon as a weapon is seen.

Touching on the model's performance for a moment, the security system rollout would need to include thresholds for triggering an alarm, such as minimum confidence levels and time durations of those confidence levels, to avoid triggering on false detections. And development of a model like this would need to be much more robust, with training datasets containing many more images than I included here.

Additionally, a deployable model would ideally be trained on a host of weapon types, not only pistols. It should include rifle-type weapons, as well as knives. The data is out there, it just needs to be collected and included on future development of this concept.

These steps for further development will also include training on more images in each class, which will hopefully help the final model be able to better distinguish weapons in noisy backgrounds like the still image examples above. Because in a real life scenario, there is a good chance that the image settings will not be ideally clean white backgrounds like I tested in my videos here.

## Conclusion

Joseph Redmon, one of the developers of the YOLO model architecture, said in his [TED Talk](https://www.youtube.com/watch?v=Cgxsv1riJhI&t=333s) that the model is "open source and in the public domain, free for anyone to use," and that "now we have a pretty powerful solution to this low level computer vision problem, and anyone can take it and build something with it, so now the rest is up to all of you." 

It is my hope that this use of technology is implemented into daily lives for the benefit of all people.

