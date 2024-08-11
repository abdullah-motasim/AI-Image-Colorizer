# APS360 Image Colorizer AI Model
This is an AI model developed as part of the final project for an Applied Fundamentals of Deep Learning course. The purpose of the model is to colorize greyscale images. 

![image](https://github.com/user-attachments/assets/68c485aa-082d-4639-a66f-05b44c6faede)

# Methedolgy
Our final model was a conditional generative adversarial network (GAN) with a UNet Generator. The genreator and discriminator were implemented with ConvBlock and ConvTBlock modules.

**ConvBlock:**

• Convolutional Layer with 3x3 kernel, stride of 1, padding of 1

• Batch normalization

• Leaky ReLU with slope of 0.2

• Max Pool with 2x2 filter, stride of 2


**ConvTBlock:**

• Transposed Convolutional Layer with 3x3 kernel, stride of 1, padding of 1

• Batch normalization

• ReLU

• Upsample with a scaling factor of 2

# Model Generator Parameters
Our generator takes an input of (batch size, 1, 256, 256), which is the greyscale image, and outputs
a (batch size, 2, 256, 256) image which is the a* and b* channels of the recolourized image.

• ConvBlock Layers:

![image](https://github.com/user-attachments/assets/bb08263d-b33c-4ae3-9a1b-ba98e73de014)

• ConvTBlock Layers

![image](https://github.com/user-attachments/assets/0320095d-395e-4f3b-9999-f2b280489e7b)

• Convolutional Layer with 3x3 kernel, stride of 1, padding of 1

• Tanh activation function

Besides the structural part of our model, an important part lies during the runtime. A Unet model
implements skip connections in all the decoder layers from the corresponding encoder layers. For
example the first ConvTBlock layer input is concatenated with the output of the last ConvBlock
layer’s output in the channel dimension. That is why the ConvTBlocks have double the previous
layer’s output channels

# Model Discriminator Parameters

Our discriminator takes a (batch size, 3, 256, 256) image, and ouputs a (batch size, 1, 16, 16) tensor.
The reasoning behind why the output is not a single channel like a normal classifier is that each cell
in the output is responsible for determining if a 16x16 patch of the image is real. This leads to better
fine tuning and convergence.

• ConvBlock Layers:

![image](https://github.com/user-attachments/assets/35d88c9e-73aa-42d3-9888-9b2aa659ab0b)

• Convolutional Layer with 3x3 kernel, stride of 1, padding of 1

# Quantitative Results

To measure the quantitative results the team decided to use the L1 loss and GAN loss. Our results for
the losses on the final model are listed in the table below for the validation and testing dataset as well
as a graph of the losses with respect to each epoch. The GAN loss is calculated with Binary Cross
Entropy of the discriminator plus 100 times L1 loss of real image with generated image. L1 loss
is not that important since our goal is to generate real images and not perfectly recoloured images
since that is an impossible task due to the fact that many different coloured images map to the same
grayscale. L1 loss was used to maintain the image structure, ensuring that the colourized results
remained accurate to the original grayscale images, while the discriminator’s Binary Cross Entropy
loss allowed for more generalization and improved the overall quality of the colourized outputs.

![image](https://github.com/user-attachments/assets/b7b70ca4-301c-4d27-b992-1ed1821d1231)   ![image](https://github.com/user-attachments/assets/4f9410ef-f8ca-4a08-a237-c8b2e8e44d66)

# Qualitative Results
To measure the qualitative results the team decided to output 7 random samples after each epoch.
The samples include the greyscale image at the top, our model’s recolourized image in the middle
and the original coloured image at the bottom. These samples were used to assess and provide a
brief look into the model’s performance on a variety of images.

![image](https://github.com/user-attachments/assets/4a4e7fd4-f292-40f3-80bc-a5d44beb1255)

# Video Detailing Model
Check out this video presentation describing the process of gathering/augmenting the data, model architecture, qualitative/quantitative results, and a live demo of the model:

https://youtu.be/h9520l85-2Y?si=v-tj-VOh0jqfc9CX
