# DreamBooth with multiple instances
This variation of Dreambooth allows to train multiple subjects ("instances") at the same time. This could be different people, animals or any other kind of object. The instances can be from the same class, but do not have to. 

The following example assumes that I want to train the model on my three animals, to dogs Bob and Charlie, and my Cat Sue.

## Input data
All input data must placed in a single directory which with the following structure:

<pre>
|-- data_dir
    |-- instances.json
    |-- MyDogBob
        |-- Bob_1.jpg
        |-- Bob_2.jpg
        |-- Bob_3.jpg
        |-- ...
    |-- MyDogCharlie
        |-- Charlie_1.jpg
        |-- Charlie_2.jpg
        |-- Charlie_3.jpg
        |-- ...
    |-- Dog
        |-- Dog_1.jpg
        |-- Dog_2.jpg
        |-- Dog_3.jpg
        |-- ...
    |-- MyCatSue
        |-- myCat_1.jpg
        |-- myCat_2.jpg
        |-- myCat_3.jpg
        |-- ...
    |-- Cat
        |-- Cat_1.jpg
        |-- Cat_2.jpg
        |-- Cat_3.jpg
        |-- ...
    |-- ...
</pre>

Folder names and image file names are arbitrary, and each folder represents one class to be used for training. `instances.json` contains information about instance and class prompts, and where to find the corresponding images. 

## Schema for `instances.json` 
The main input file is `instances.json` which contains a list of objects with a schema like this:

<pre>
[
    {
        "instance_prompt": "Photo of a dog [MyDogBob]",
        "class_prompt": "Photo of a dog",
        "instance_folder": "MyDogBob",
        "class_folder": "Dog",
        "count": 50
    },
    {
        "instance_prompt": "Photo of a dog [MyDogCharlie]",
        "class_prompt": "Photo of a dog",
        "instance_folder": "MyDogCharlie",
        "class_folder": "Dog",
        "count": 40
    },
    {
        "instance_prompt": "Photo of a cat [MyCatSue]",
        "class_prompt": "Photo of a cat",
        "instance_folder": "MyCatSue",
        "class_folder": "Cat"
    }
]
</pre>
Here, each object has these fields:
* The `instance_prompt` should contain the unique token representing the instance to be learned (e.g. `[MyDogBob]`).
* The `class_prompt` should be a neutral prompt describing the instance's class without the unique token
* `instance_folder` is the name of the folder that contains the  training images for the instance (e.g. `MyDogBob`).
* `class_folder` is the name of the folder that contains the training images corresponding to the `class_prompt` (e.g. `Dog`).
* `count` (optional) is the number of training images for this instance. If missing, use the number of files in `instance_folder`.

Note that with this structure we can share images across instances. In this case the `Dog` folder contains class images for both `[MyDogBob]` and `[MyDogCharlie]`. 

Similarly, we can re-use the instance images to train each subject with multiple prompts. For example, to the above we could add
<pre>
    {
        "instance_prompt": "Photo of a brown labrador [MyDogBob]",
        "class_prompt": "Photo of a brown labrador",
        "instance_folder": "MyDogBob",
        "class_folder": "Labrador"
    }
</pre>

## Missing:
Currently there is no functionality to automatically create images class images from the provided prompts. 
