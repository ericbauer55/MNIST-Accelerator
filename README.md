![](../../workflows/gds/badge.svg) ![](../../workflows/docs/badge.svg) ![](../../workflows/test/badge.svg) ![](../../workflows/fpga/badge.svg)

# MNIST Neural Network Accelerator
This project is for a neural network accelerator ASIC for text recognition in MNIST handwritten digit dataset.
Due to constraints with the fab organization (Tiny Tapeout), the following goals have been outlined.

Goals:

* classify at least 2 of the 10 digits (Ex. '3' and '5')

* recall of at least 0.8

* increase performance (speed) to classify image (benchmark: python-based neural net on a raspberry pi.)

Diagram:

<pre>
___________________                                     __________                            _____
|                 |                                     |        |                           |  7  |
|  Raspberry Pi   | ---- Pre-processed MNIST Image ---> |  Chip  | --- 4-pin BCD number ---> | seg |
|                 |                                     |        |                           |disp |
|                 | <--- Returned Classification -----  |        |                           |     |
|_________________|                                     |________|                           |_____|
</pre>
The Raspberry Pi will hold all of the images from the test set. It will loop through each image, preprocess it (to reduce data size), and deliver it to the chip. the chip will then return the classification back. This process is repeated for all images in the test set.

The chip's logic is laid out into 4 main components:
<pre>
   _________________________________________________________________________
   |   _________         __________         __________         __________    |
   |   |       |         |        |         |        |         |        |    |
  \/   |  I/O  |         | Memory |         |   NN   |         | Output |    |
------>|       | ----->  |        | ----->  |        | ----->  |        | --->
       |       |         |        |         |        |         |        |
       |_______|         |________|         |________|         |________|
</pre>

## I/O
Wait to recieve image over I2C. Read 28x1 image into memory. Read weights and biases for each neuron into memory.

## Memory
stores the 28x1 image as well as the weights and biases for the neural network. Memory will only be able to hold one image at a time.

## Neutal Network
Configure neural net with weights and biases and begin processing image from memory. return the classification of the image as a single byte as well as the confidence.

## Output
Display the decoded digit on 7 segment display. This section should incorporate logic to decode a binary coded decimal (BCD) into 7 segment logic for external 7 segment display.
Additionally, the output layer should trigger an additional digital pin as a flag to signal the image has finished being processed and another image can be fed in over I2C


# Resources

- [FAQ](https://tinytapeout.com/faq/)
- [Digital design lessons](https://tinytapeout.com/digital_design/)
- [Learn how semiconductors work](https://tinytapeout.com/siliwiz/)
- [Join the community](https://tinytapeout.com/discord)
- [Build your design locally](https://docs.google.com/document/d/1aUUZ1jthRpg4QURIIyzlOaPWlmQzr-jBn3wZipVUPt4)

# Tools
 [OSS Suite](https://github.com/YosysHQ/oss-cad-suite-build)
