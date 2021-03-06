Keras-RCNN
==========

.. image:: https://travis-ci.org/broadinstitute/keras-rcnn.svg?branch=master
    :target: https://travis-ci.org/broadinstitute/keras-rcnn

.. image:: https://codecov.io/gh/broadinstitute/keras-rcnn/branch/master/graph/badge.svg
    :target: https://codecov.io/gh/broadinstitute/keras-rcnn

keras-rcnn is *the* Keras package for region-based convolutional
neural networks.

Getting Started
---------------

Let’s read and inspect some data:

.. code:: python

    training_dictionary, test_dictionary = keras_rcnn.datasets.shape.load_data()

    categories = {"circle": 1, "rectangle": 2, "triangle": 3}

    generator = keras_rcnn.preprocessing.ObjectDetectionGenerator()

    generator = generator.flow_from_dictionary(
        dictionary=training_dictionary,
        categories=categories,
        target_size=(224, 224)
    )

    validation_data = keras_rcnn.preprocessing.ObjectDetectionGenerator()

    validation_data = validation_data.flow_from_dictionary(
        dictionary=test_dictionary,
        categories=categories,
        target_size=(224, 224)
    )

    target, _ = generator.next()
    
    target_bounding_boxes, target_categories, target_images, target_masks, target_metadata = target

    target_bounding_boxes = numpy.squeeze(target_bounding_boxes)

    target_images = numpy.squeeze(target_images)

    target_categories = numpy.argmax(target_categories, -1)

    target_categories = numpy.squeeze(target_categories)

    keras_rcnn.util.show_bounding_boxes(target_image, target_bounding_boxes, target_categories)


Let’s create an RCNN instance:

.. code:: python

    model = keras_rcnn.models.RCNN((224, 224, 3), ["circle", "rectangle", "triangle"])

and pass our preferred optimizer to the `compile` method:

.. code:: python

    optimizer = keras.optimizers.Adam()

    model.compile(optimizer)

Finally, let’s use the `fit_generator` method to train our network:

.. code:: python

    model.fit_generator(    
        epochs=10,
        generator=generator,
        validation_data=validation_data
    )

Backbone
--------

A backbone is the convolutional neural network (CNN) that’s responsible for 
extracting the features that’re used by the RCNN. Keras-RCNN provides a number 
of popular backbones like ResNet and VGG. You can use a backbone by passing a 
backbone function to the RCNN constructor:

.. code:: python

    backbone = keras_rcnn.models.backbone.VGG16

    model = keras_rcnn.models.RCNN((224, 224, 3), ["circle", "rectangle", "triangle"], backbone)

Slack
-----

We’ve been meeting in the #keras-rcnn channel on the keras.io Slack
server. 

You can join the server by inviting yourself from the following website:

https://keras-slack-autojoin.herokuapp.com/
