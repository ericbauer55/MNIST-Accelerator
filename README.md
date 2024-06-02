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

## Neural Network
Configure neural net with weights and biases and begin processing image from memory. return the classification of the image as a single byte as well as the confidence.

## Output
Display the decoded digit on 7 segment display. This section should incorporate logic to decode a binary coded decimal (BCD) into 7 segment logic for external 7 segment display.
Additionally, the output layer should trigger an additional digital pin as a flag to signal the image has finished being processed and another image can be fed in over I2C


# Tiny Tapeout Verilog Project Template

- [Read the documentation for project](docs/info.md)

## What is Tiny Tapeout?

Tiny Tapeout is an educational project that aims to make it easier and cheaper than ever to get your digital and analog designs manufactured on a real chip.

To learn more and get started, visit https://tinytapeout.com.

## Set up your Verilog project

1. Add your Verilog files to the `src` folder.
2. Edit the [info.yaml](info.yaml) and update information about your project, paying special attention to the `source_files` and `top_module` properties. If you are upgrading an existing Tiny Tapeout project, check out our [online info.yaml migration tool](https://tinytapeout.github.io/tt-yaml-upgrade-tool/).
3. Edit [docs/info.md](docs/info.md) and add a description of your project.
4. Adapt the testbench to your design. See [test/README.md](test/README.md) for more information.

The GitHub action will automatically build the ASIC files using [OpenLane](https://www.zerotoasiccourse.com/terminology/openlane/).

## Enable GitHub actions to build the results page

- [Enabling GitHub Pages](https://tinytapeout.com/faq/#my-github-action-is-failing-on-the-pages-part)

## Resources

- [FAQ](https://tinytapeout.com/faq/)
- [Digital design lessons](https://tinytapeout.com/digital_design/)
- [Learn how semiconductors work](https://tinytapeout.com/siliwiz/)
- [Join the community](https://tinytapeout.com/discord)
- [Build your design locally](https://docs.google.com/document/d/1aUUZ1jthRpg4QURIIyzlOaPWlmQzr-jBn3wZipVUPt4)

## What next?

- [Submit your design to the next shuttle](https://app.tinytapeout.com/).
- Edit [this README](README.md) and explain your design, how it works, and how to test it.
- Share your project on your social network of choice:
  - LinkedIn [#tinytapeout](https://www.linkedin.com/search/results/content/?keywords=%23tinytapeout) [@TinyTapeout](https://www.linkedin.com/company/100708654/)
  - Mastodon [#tinytapeout](https://chaos.social/tags/tinytapeout) [@matthewvenn](https://chaos.social/@matthewvenn)
  - X (formerly Twitter) [#tinytapeout](https://twitter.com/hashtag/tinytapeout) [@matthewvenn](https://twitter.com/matthewvenn)
