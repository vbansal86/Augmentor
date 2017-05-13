![AugmentorLogo](https://github.com/mdbloice/AugmentorFiles/blob/master/Misc/AugmentorLogo.png)

[![Documentation Status](https://readthedocs.org/projects/augmentor/badge/?version=latest)](http://augmentor.readthedocs.io/en/latest/?badge=latest) [![License](http://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat)](LICENSE.md) [![Project Status: WIP - Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](http://www.repostatus.org/badges/latest/wip.svg)](http://www.repostatus.org/#wip) [![PyPI](https://img.shields.io/badge/pypi-v0.1.3-blue.svg?maxAge=2592000)](https://pypi.python.org/pypi/Augmentor) [![Supported Python Versions](https://img.shields.io/badge/python-2.7%2C%203.3--3.6-orange.svg)](https://pypi.python.org/pypi/Augmentor)

Image augmentation library in Python for machine learning. A Julia version of the package is also being developed as a sister project and is available [here](https://github.com/Evizero/Augmentor.jl).

## Installation

Install using `pip` from the command line:

```python
pip install Augmentor
```

See the documentation for building from source. To upgrade from a previous version, use `pip install Augmentor --upgrade`.

## Documentation

Complete documentation can be found on Read the Docs: <http://augmentor.readthedocs.io/>

## Quick Start Guide and Usage
The purpose of _Augmentor_ is to automate image augmentation (artificial data generation) in order to expand datasets as input for machine learning algorithms, especially neural networks and deep learning.

The package works by building an augmentation **pipeline** where you define a series of operations to perform on a set of images. Operations, such as rotations or transforms, are added one by one to create an augmentation pipeline: when complete, the pipeline can be executed and an augmented dataset is created.

To begin, instantiate a `Pipeline` object that points to a directory on your file system:

```python
import Augmentor
p = Augmentor.Pipeline("/path/to/images")
```

You can then add operations to the Pipeline object `p` as follows:

```python
p.rotate(probability=0.7, max_left=10, max_right=10)
p.zoom(probability=0.5, min_scale=1.1, max_scale=1.5)
```

Every function requires you to specify a probability, which is used to decide if an operation is applied to an image as it is passed through the augmentation pipeline.

Once you have created a pipeline, you can sample from it like so:

```python
p.sample(10000)
```

which will generate 10,000 augmented images based on your specifications. By default these will be written to the disk in a directory named `output` relative to the path specified when initialising the `p` pipeline object above.

## Main Features

### Elastic Distortions

Using elastic distortions, one image can be used to generate many images that are real-world feasible and label preserving:

| Input Image                                                                                                                       |   | Augmented Images                                                                                                        |
|-----------------------------------------------------------------------------------------------------------------------------------|---|-------------------------------------------------------------------------------------------------------------------------|
| ![eight_hand_drawn_border](https://cloud.githubusercontent.com/assets/16042756/23697279/79850d52-03e7-11e7-9445-475316b702a3.png) | → | ![eights_border](https://cloud.githubusercontent.com/assets/16042756/23697283/802698a6-03e7-11e7-94b7-f0b61977ef33.gif) |

The input image has a 1 pixel black border to emphasise that you are getting distortions without changing the size or aspect ratio of the original image, and without any black/transparent padding around the newly generated images.

The functionality can be more clearly seen here:

| Original Image<sup>[1]</sup>                                                                      | Random distortions applied                                                                            |
|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/orig.png) | ![Distorted](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/distort.gif) |

### Perspective Transforms

There are a total of 12 different types of perspective transform available. Four of the most common are shown below.

| Tilt Left                                                                                               | Tilt Right                                                                                               | Tilt Forward                                                                                               | Tilt Backward                                                                                               |
|---------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| ![TiltLeft](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/TiltLeft_s.png) | ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/TiltRight_s.png) | ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/TiltForward_s.png) | ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/TiltBackward_s.png) |

The remaining eight types of transform are as follows:

| Skew Type 0                                                                                         | Skew Type 1                                                                                         | Skew Type 2                                                                                         | Skew Type 3                                                                                         |
|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| ![Skew0](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/Corner0_s.png) | ![Skew1](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/Corner1_s.png) | ![Skew2](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/Corner2_s.png) | ![Skew3](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/Corner3_s.png) |

| Skew Type 4                                                                                         | Skew Type 5                                                                                         | Skew Type 6                                                                                         | Skew Type 7                                                                                         |
|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| ![Skew4](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/Corner4_s.png) | ![Skew5](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/Corner5_s.png) | ![Skew6](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/Corner6_s.png) | ![Skew7](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/Corner7_s.png) |

### Size Preserving Rotations

Rotations by default preserve the file size of the original images:

| Original Image                                                                                    | Rotated 10 degrees, automatically cropped                                                               |
|---------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/orig.png) | ![Rotate](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/rotate_aug_b.png) |

Compared to rotations by other software:

| Original Image                                                                                    | Rotated 10 degrees                                                                                |
|---------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/orig.png) | ![Rotate](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/rotate.png) |

### Size Preserving Shearing

Shearing will also automatically crop the correct area from the sheared image, so that you have an image with no black space or padding.

| Original image                                                                                    | Shear (x-axis) 20 degrees                                                                              | Shear (y-axis) 20 degrees                                                                              |
|---------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/orig.png) | ![ShearX](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/shear_x_aug.png) | ![ShearY](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/shear_y_aug.png) |

Compare this to how this is normally done:

| Original image                                                                                    | Shear (x-axis) 20 degrees                                                                          | Shear (y-axis) 20 degrees                                                                          |
|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/orig.png) | ![ShearX](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/shear_x.png) | ![ShearY](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/shear_y.png) |

### Cropping

Cropping can also be handled in a manner more suitable for machine learning image augmentation:

| Original image                                                                                    | Random crops + resize operation                                                                          |
|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/orig.png) | ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/crop_resize.gif) |

### Chaining Operations in a Pipeline

With only a few operations, a single image can be augmented to produce large numbers of new, label-preserving samples:

| Original image                                                                                           | Distortions + mirroring                                                                                          |
|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------|
| ![Original](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/eight_200px.png) | ![DistortFlipFlop](https://raw.githubusercontent.com/mdbloice/AugmentorFiles/master/UsageGuide/flip_distort.gif) |

In the example above, we have applied three operations: first we randomly distort the image, then we flip it horizontally with a probability of 0.5 and then vertically with a probability of 0.5. We then sample from this pipeline 100 times to create 100 new data.

```python
p.random_distortion(probability=1, grid_width=4, grid_height=4, magnitude=8)
p.flip_left_right(probability=0.5)
p.flip_top_bottom(probability=0.5)
p.sample(100)
```

## Complete Example

Let's perform an augmentation task on a single image, demonstrating the pipeline and several features of Augmentor.

First import the package and initialise a Pipeline object by pointing it to a directory containing your images:

```python
import Augmentor

p = Augmentor.Pipeline("/home/user/augmentor_data_tests")
```

Now you can begin adding operations to the pipeline object:

```python
p.rotate90(probability=0.5)
p.rotate270(probability=0.5)
p.flip_left_right(probability=0.8)
p.flip_top_bottom(probability=0.3)
p.crop_random(probability=1, percentage_area=0.5)
p.resize(probability=1.0, width=120, height=120)
```

Once you have added the operations you require, you can sample images from this pipeline:

```python
p.sample(100)
```

Some sample output:

| Input Image<sup>[2]</sup>                                                                                          |   | Augmented Images                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------|---|---------------------------------------------------------------------------------------------------------------------|
| ![Original](https://cloud.githubusercontent.com/assets/16042756/23019262/b696e3a6-f441-11e6-958d-17f18f2cd35e.jpg) | → | ![Augmented](https://cloud.githubusercontent.com/assets/16042756/23018832/cda6967e-f43f-11e6-9082-765c291f1fd6.gif) |

The augmented images may be useful for a boundary detection task, for example.

## Licence and Acknowledgements

Augmentor is made available under the terms of the MIT Licence. See [`Licence.md`](https://github.com/mdbloice/Augmentor/blob/master/LICENSE.md).

[1] Checkerboard image obtained from Wikimedia Commons and is in the public domain: <https://commons.wikimedia.org/wiki/File:Checkerboard_pattern.svg>

[2] Skin lesion image obtained from the ISIC Archive:

- Image id = 5436e3abbae478396759f0cf
- Download: <https://isic-archive.com:443/api/v1/image/5436e3abbae478396759f0cf/download>

You can use `urllib` to obtain the skin lesion image in order to reproduce the augmented images above:

```python
>>> from urllib import urlretrieve
>>> im_url = "https://isic-archive.com:443/api/v1/image/5436e3abbae478396759f0cf/download"
>>> urlretrieve(im_url, "ISIC_0000000.jpg")
('ISIC_0000000.jpg', <httplib.HTTPMessage instance at 0x7f7bd949a950>)
```

Note: For Python 3, use `from urllib.request import urlretrieve`.

## Asciicast

Click the preview below to view a video demonstration of Augmentor in use:

[![asciicast](https://asciinema.org/a/105368.png)](https://asciinema.org/a/105368?autoplay=1&speed=3)
