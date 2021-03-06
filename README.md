# Snack_Security

A Node.js app using [Express 4]. This repository contains face_recognition, an application developed during my internship as "Snack Security".

The barebones node.js app was originally forked from https://github.com/heroku/node-js-sample.

Information on Heroku Deployment, written from the original repository, is at the end of this ReadMe. 

## Snack Security- Derived from Face Recognition

Snack Security is a program forked from Adam Geitgey's Facial Recognition API. Built to protect snacks, the program plays an alarm and captures an image when an unknown face is detected in the frame. After capturing multiple images with the same unknown face, the program recognizes the face as "Unnamed" rather than "Unknown".

The file from the forked repository that I edited is "facerec_from_webcam_faster.py".

### Prerequisites

In addition to the installations required for Adam Geitgey's Face Recognition programs, the Pyglet framework is required to play sound in the example, "facerec_from_webcam_faster.py". Additionally, OpenCV 3+ is required for live webcam recognition. OpenCV installation is complicated-- I found that the following tutorial worked for me:

http://www.pyimagesearch.com/2016/12/05/macos-install-opencv-3-and-python-3-5/
The only difference between my installation and the tutorial is that I used Python3.6.1, so my cmake configuration was the following:

```python
cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D PYTHON3_EXECUTABLE=$(which python3) \
    -D PYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
    -D PYTHON3_LIBRARY=/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/lib/python3.6/config-3.6m-darwin/libpython3.6.dylib \
    -D PYTHON3_LIBRARIES=/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/bin \
    -D PYTHON3_INCLUDE_DIR=/usr/local/Cellar/python3/3.6.1/Frameworks/Python.framework/Versions/3.6/Headers \
    -D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
    -D INSTALL_C_EXAMPLES=OFF -D INSTALL_PYTHON_EXAMPLES=ON \
    -D BUILD_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules ..
```
    
A dockerfile is included for the installation of OpenCV3, Dlib, and Face_recognition on a linux machine. While it is currently unable to grab a reference to the machine webcam, the steps included in the file create a tutorial which can be used for any linux machine with python3.4.

### Notes

This program captures and saves images of unknown faces it detects in the frame. Please use with the permission and knowledge of anyone who may be seen on camera.

# Face Recognition

Recognize and manipulate faces from Python or from the command line with
the world's simplest face recognition library.

Built using [dlib](http://dlib.net/)'s state-of-the-art face recognition
built with deep learning. The model has an accuracy of 99.38% on the
[Labeled Faces in the Wild](http://vis-www.cs.umass.edu/lfw/) benchmark.

This also provides a simple `face_recognition` command line tool that lets
you do face recognition on a folder of images from the command line!


[![PyPI](https://img.shields.io/pypi/v/face_recognition.svg)](https://pypi.python.org/pypi/face_recognition)
[![Build Status](https://travis-ci.org/ageitgey/face_recognition.svg?branch=master)](https://travis-ci.org/ageitgey/face_recognition)
[![Documentation Status](https://readthedocs.org/projects/face-recognition/badge/?version=latest)](http://face-recognition.readthedocs.io/en/latest/?badge=latest)

## Features

#### Find faces in pictures

Find all the faces that appear in a picture:

![](https://cloud.githubusercontent.com/assets/896692/23625227/42c65360-025d-11e7-94ea-b12f28cb34b4.png)

```python
import face_recognition
image = face_recognition.load_image_file("your_file.jpg")
face_locations = face_recognition.face_locations(image)
```

#### Find and manipulate facial features in pictures

Get the locations and outlines of each person's eyes, nose, mouth and chin.

![](https://cloud.githubusercontent.com/assets/896692/23625282/7f2d79dc-025d-11e7-8728-d8924596f8fa.png)

```python
import face_recognition
image = face_recognition.load_image_file("your_file.jpg")
face_landmarks_list = face_recognition.face_landmarks(image)
```

Finding facial features is super useful for lots of important stuff. But you can also use for really stupid stuff
like applying [digital make-up](https://github.com/ageitgey/face_recognition/blob/master/examples/digital_makeup.py) (think 'Meitu'):

![](https://cloud.githubusercontent.com/assets/896692/23625283/80638760-025d-11e7-80a2-1d2779f7ccab.png)

#### Identify faces in pictures

Recognize who appears in each photo.

![](https://cloud.githubusercontent.com/assets/896692/23625229/45e049b6-025d-11e7-89cc-8a71cf89e713.png)

```python
import face_recognition
known_image = face_recognition.load_image_file("biden.jpg")
unknown_image = face_recognition.load_image_file("unknown.jpg")

biden_encoding = face_recognition.face_encodings(known_image)[0]
unknown_encoding = face_recognition.face_encodings(unknown_image)[0]

results = face_recognition.compare_faces([biden_encoding], unknown_encoding)
```

You can even use this library with other Python libraries to do real-time face recognition:

![](https://cloud.githubusercontent.com/assets/896692/24430398/36f0e3f0-13cb-11e7-8258-4d0c9ce1e419.gif)

See [this example](https://github.com/ageitgey/face_recognition/blob/master/examples/facerec_from_webcam_faster.py) for the code.

## Installation

Requirements:
* Python 3+ or Python 2.7
* macOS or Linux (Windows untested)
* [Also can run on a Raspberry Pi 2+ (follow these specific instructions)](https://gist.github.com/ageitgey/1ac8dbe8572f3f533df6269dab35df65)
* A [pre-configured VM image](https://medium.com/@ageitgey/try-deep-learning-in-python-now-with-a-fully-pre-configured-vm-1d97d4c3e9b) is also available.

Install this module from pypi using `pip3` (or `pip2` for Python 2):

```bash
pip3 install face_recognition
```

IMPORTANT NOTE: It's very likely that you will run into problems when pip tries to compile
the `dlib` dependency. If that happens, check out this guide to installing
dlib from source (instead of from pip) to fix the error:

[How to install dlib from source](https://gist.github.com/ageitgey/629d75c1baac34dfa5ca2a1928a7aeaf)

After manually installing `dlib`, try running `pip3 install face_recognition`
again to complete your installation.

If you are still having trouble installing this, you can also try out this
[pre-configured VM](https://medium.com/@ageitgey/try-deep-learning-in-python-now-with-a-fully-pre-configured-vm-1d97d4c3e9b).

## Usage

#### Command-Line Interface

When you install `face_recognition`, you get a simple command-line program
called `face_recognition` that you can use to recognize faces in a
photograph or folder full for photographs.

First, you need to provide a folder with one picture of each person you
already know. There should be one image file for each person with the
files named according to who is in the picture:

![known](https://cloud.githubusercontent.com/assets/896692/23582466/8324810e-00df-11e7-82cf-41515eba704d.png)

Next, you need a second folder with the files you want to identify:

![unknown](https://cloud.githubusercontent.com/assets/896692/23582465/81f422f8-00df-11e7-8b0d-75364f641f58.png)

Then in you simply run the command `face_recognition`, passing in
the folder of known people and the folder (or single image) with unknown
people and it tells you who is in each image:

```bash
$ face_recognition ./pictures_of_people_i_know/ ./unknown_pictures/

/unknown_pictures/unknown.jpg,Barack Obama
/face_recognition_test/unknown_pictures/unknown.jpg,unknown_person
```

There's one line in the output for each face. The data is comma-separated
with the filename and the name of the person found.

An `unknown_person` is a face in the image that didn't match anyone in
your folder of known people.

If you simply want to know the names of the people in each photograph but don't
care about file names, you could do this:

```bash
$ face_recognition ./pictures_of_people_i_know/ ./unknown_pictures/ | cut -d ',' -f2

Barack Obama
unknown_person
```


#### Python Module

You can import the `face_recognition` module and then easily manipulate
faces with just a couple of lines of code. It's super easy!

API Docs: [https://face-recognition.readthedocs.io](https://face-recognition.readthedocs.io/en/latest/face_recognition.html).

##### Automatically find all the faces in an image

```python
import face_recognition

image = face_recognition.load_image_file("my_picture.jpg")
face_locations = face_recognition.face_locations(image)

# face_locations is now an array listing the co-ordinates of each face!
```

See [this example](https://github.com/ageitgey/face_recognition/blob/master/examples/find_faces_in_picture.py)
 to try it out.

##### Automatically locate the facial features of a person in an image

```python
import face_recognition

image = face_recognition.load_image_file("my_picture.jpg")
face_landmarks_list = face_recognition.face_landmarks(image)

# face_landmarks_list is now an array with the locations of each facial feature in each face.
# face_landmarks_list[0]['left_eye'] would be the location and outline of the first person's left eye.
```

See [this example](https://github.com/ageitgey/face_recognition/blob/master/examples/find_facial_features_in_picture.py)
 to try it out.

##### Recognize faces in images and identify who they are

```python
import face_recognition

picture_of_me = face_recognition.load_image_file("me.jpg")
my_face_encoding = face_recognition.face_encodings(picture_of_me)[0]

# my_face_encoding now contains a universal 'encoding' of my facial features that can be compared to any other picture of a face!

unknown_picture = face_recognition.load_image_file("unknown.jpg")
unknown_face_encoding = face_recognition.face_encodings(unknown_picture)[0]

# Now we can see the two face encodings are of the same person with `compare_faces`!

results = face_recognition.compare_faces([my_face_encoding], unknown_face_encoding)

if results[0] == True:
    print("It's a picture of me!")
else:
    print("It's not a picture of me!")
```

See [this example](https://github.com/ageitgey/face_recognition/blob/master/examples/recognize_faces_in_pictures.py)
 to try it out.

## Python Code Examples

All the examples are available [here](https://github.com/ageitgey/face_recognition/tree/master/examples).

* [Find faces in a photograph](https://github.com/ageitgey/face_recognition/blob/master/examples/find_faces_in_picture.py)
* [Identify specific facial features in a photograph](https://github.com/ageitgey/face_recognition/blob/master/examples/find_facial_features_in_picture.py)
* [Apply (horribly ugly) digital make-up](https://github.com/ageitgey/face_recognition/blob/master/examples/digital_makeup.py)
* [Find and recognize unknown faces in a photograph based on photographs of known people](https://github.com/ageitgey/face_recognition/blob/master/examples/recognize_faces_in_pictures.py)
* [Recognize faces in live video using your webcam - Simple / Slower Version (Requires OpenCV to be installed)](https://github.com/ageitgey/face_recognition/blob/master/examples/facerec_from_webcam.py)
* [Recognize faces in live video using your webcam - Faster Version (Requires OpenCV to be installed)](https://github.com/ageitgey/face_recognition/blob/master/examples/facerec_from_webcam_faster.py)
* [Recognize faces on a Raspberry Pi w/ camera](https://github.com/ageitgey/face_recognition/blob/master/examples/facerec_on_raspberry_pi.py)
* [Run a web service to recognize faces via HTTP (Requires Flask to be installed)](https://github.com/ageitgey/face_recognition/blob/master/examples/web_service_example.py)

## How Face Recognition Works

If you want to learn how face location and recognition work instead of
depending on a black box library, [read my article](https://medium.com/@ageitgey/machine-learning-is-fun-part-4-modern-face-recognition-with-deep-learning-c3cffc121d78).

## Caveats

* The face recognition model is trained on adults and does not work very well on children. It tends to mix
  up children quite easy using the default comparison threshold of 0.6.

## Deployment to Cloud Hosts (Heroku, AWS, etc)

Since `face_recognition` depends on `dlib` which is written in C++, it can be tricky to deploy an app
using it to a cloud hosting provider like Heroku or AWS.

To make things easier, there's an example Dockerfile in this repo that shows how to run an app built with
`face_recognition` in a [Docker](https://www.docker.com/) container. With that, you should be able to deploy
to any service that supports Docker images.

## Common Issues

Issue: `Illegal instruction (core dumped)` when using face_recognition or running examples.

Solution: `dlib` is compiled with SSE4 or AVX support, but your CPU is too old and doesn't support that.
You'll need to recompile `dlib` after [making the code change outlined here](https://github.com/ageitgey/face_recognition/issues/11#issuecomment-287398611).

Issue: `RuntimeError: Unsupported image type, must be 8bit gray or RGB image.` when running the webcam examples.

Solution: Your webcam probably isn't set up correctly with OpenCV. [Look here for more](https://github.com/ageitgey/face_recognition/issues/21#issuecomment-287779524).

Issue: `MemoryError` when running `pip2 install face_recognition`

Solution: The face_recognition_models file is too big for your available pip cache memory. Instead,
try `pip2 --no-cache-dir install face_recognition` to avoid the issue.

Issue: `AttributeError: 'module' object has no attribute 'face_recognition_model_v1'`

Solution: The version of `dlib` you have installed is too old. You need version 19.4 or newer. Upgrade `dlib`.

Issue: `TypeError: imread() got an unexpected keyword argument 'mode'`

Solution: The version of `scipy` you have installed is too old. You need version 0.17 or newer. Upgrade `scipy`.

## Thanks

* Many, many thanks to [Davis King](https://github.com/davisking) ([@nulhom](https://twitter.com/nulhom))
  for creating dlib and for providing the trained facial feature detection and face encoding models
  used in this library. For more information on the ResNet that powers the face encodings, check out
  his [blog post](http://blog.dlib.net/2017/02/high-quality-face-recognition-with-deep.html).
* Thanks to everyone who works on all the awesome Python data science libraries like numpy, scipy, scikit-image,
  pillow, etc, etc that makes this kind of stuff so easy and fun in Python.
* Thanks to [Cookiecutter](https://github.com/audreyr/cookiecutter) and the
  [audreyr/cookiecutter-pypackage](https://github.com/audreyr/cookiecutter-pypackage) project template
  for making Python project packaging way more tolerable.


# node-js-sample

## Running Locally

Make sure you have [Node.js](http://nodejs.org/) and the [Heroku Toolbelt](https://toolbelt.heroku.com/) installed.

```sh
git clone git@github.com:heroku/node-js-sample.git # or clone your own fork
cd node-js-sample
npm install
npm start
```

Your app should now be running on [localhost:5000](http://localhost:5000/).

## Deploying to Heroku

```
heroku create
git push heroku master
heroku open
```

Alternatively, you can deploy your own copy of the app using the web-based flow:

[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

## Documentation

For more information about using Node.js on Heroku, see these Dev Center articles:

- [10 Habits of a Happy Node Hacker](https://blog.heroku.com/archives/2014/3/11/node-habits)
- [Getting Started with Node.js on Heroku](https://devcenter.heroku.com/articles/getting-started-with-nodejs)
- [Heroku Node.js Support](https://devcenter.heroku.com/articles/nodejs-support)
- [Node.js on Heroku](https://devcenter.heroku.com/categories/nodejs)
- [Using WebSockets on Heroku with Node.js](https://devcenter.heroku.com/articles/node-websockets)
