--
layout: page
title: Concept
--

#Implementation and Approach

The immediate issue with training a neural network to extend the boundaries of an image is that it is impossible to feed different sized matrices, or images in our case, into most if not all conventional graph designs. This would mean that all image sizes would need a separate network trained and on top of this you would also need to train each image size with a variety of output sizes to accommodate different filter sizes. Luckily we have Fourier transforms.

Instead of inputting normal images into the neural network, we can instead perform a fourier transform on the image and input that into the neural network. We can Fourier transform all images into the same dimension matrix so that they can all be input into the same neural network. This does mean that quality of images will be capped at whatever sized Fourier domain is used. This is not that impactful to our use as we are mainly interested in the larger pattern of the edge directions. An additional thought was that since Fourier transforms can highlight edges in images with the use of a high pass filter that it may organize the information in a more useable way for the neural network. 

The training set for the neural network will be made by taking a random selection of images of a certain style, we chose landscape photographs for ours, and center cropping them to create input images. If the network is going to be general use then a range of cropping amounts can be used, but the network will likely be more accurate for set crop amount. We chose to use a crop amount of 24 pixels per edge which would allow for up to a 49x49 pixel convolutional filter to be used with this network. 

The cropped and uncropped image pairs were then filtered to be grayscale, they could also be separated into color channels and trained on separate networks for potential gains in accuracy for colored photos. Now as grayscale images, the cropped and uncropped images are Fourier transformed to 1000x1000 pixel Fourier domains. The Fourier Transform of the cropped image was saved as FInputXXXX.png and the Fourier Transform of the uncropped image was saved as FExpectedXXXX.png with the XXXX’s being the iterative image number and the same for each pair of images.

A sequential neural network was set up in JavaScript using TensorFlow.js to train on the Fourier Transformed image pairs. We then trained the network with ~2000 Fourier Transform image pairs of landscape scenery. Once trained, the network can be used on novel images by first Fourier Transforming them and inputting them into the neural network. The NN outputs a 1000x1000 Fourier Domain of the predicted larger image. In our networks case it will be a prediction for the input image extended by 24 pixels on all sides. This Fourier Transformed prediction is then transformed back to a normal image. Since all this manipulation likely messed up a lot of the original image’s pixels, we just trim off the new border pixels and fill them in around the original image to get a rough makeup of the nonexistent pixels. 

This image with grown pixels can now be used with a convolutional filter to hopefully get a more accurate filtered image than previous techniques allowed.

To somewhat evaluate the results of our neural network we will compare an average pixel prediction score for each technique across a variety of random images. 

