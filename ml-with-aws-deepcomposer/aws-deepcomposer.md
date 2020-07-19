## Machine Learning with AWS DeepComposer

### AWS DeepComposer

AWS Deep Composer uses Generative AI, or specifically Generative Adversarial Networks (GANs), to generate music. GANs pit 2 networks, a generator and a discriminator, against each other to generate new content.

![aws deepcomposer](https://video.udacity-data.com/topher/2020/May/5eb1e389_aws-mle-orchestra-metaphor/aws-mle-orchestra-metaphor.jpg)

### How DeepComposer works 

Uses a Gan that have a generator and a discriminator trained for thousands of iterations or epochs.
Loss funtions measure how close or accurate the model comes to a desired output

![aws deepcomposer how works](https://video.udacity-data.com/topher/2020/May/5eb23921_aws-mle-gan-model/aws-mle-gan-model.jpg)

#### DeepComposer aws architecture

+ Input melody captured on the AWS DeepComposer console
+ Console makes a backend call to AWS DeepComposer APIs that triggers an execution Lambda.
+ Book-keeping is recorded in Dynamo DB.
+ The execution Lambda performs an inference query to SageMaker which hosts the model and the training inference container.
+ The query is run on the Generative AI model.
+ The model generates a composition.
+ The generated composition is returned.
+ The user can hear the composition in the console.
+ The user can share the composition to SoundCloud.

![aws deepcomposer](https://video.udacity-data.com/topher/2020/May/5ebed725_aws-mle-under-hood-v2/aws-mle-under-hood-v2.jpg)

#### How to measure the quality of the music we’re generating:

+ We can monitor the loss function to make sure the model is converging
+ We can check the similarity index to see how close is the model to mimicking the style of the data. When the graph of the similarity index smoothes out and becomes less spikey, we can be confident that the model is converging
+ We can listen to the music created by the generated model to see if it's doing a good job. The musical quality of the model should improve as the number of training epochs increases.
w
#### Training architecture

+ User launch a training job from the AWS DeepComposer console by selecting hyperparameters and data set filtering tags
+ The backend consists of an API Layer (API gateway and lambda) write request to DynamoDB
+ Triggers a lambda function that starts the training workflow
+ It then uses AWS Step Funcitons to launch the training job on Amazon SageMaker
+ Status is continually monitored and updated to DynamoDB
+ The console continues to poll the backend for the status of the training job and update the results live so users can see how the model is learning

![aws deepcomposer](https://video.udacity-data.com/topher/2020/May/5eb8249e_aws-mle-train-arch/aws-mle-train-arch.png)

##### Challenges with GANs

+ Clean datasets are hard to obtain
+ Not all melodies sound good in all genres
+ Convergence in GAN is tricky – it can be fleeting rather than being a stable state
+ Complexity in defining meaningful quantitive metrics to measure the quality of music created

##### Generative AI

Generative AI has been described as one of the most promising advances in AI in the past decade by the MIT Technology Review.

Generative AI enables computers to learn the underlying pattern associated with a provided input (image, music, or text), and then they can use that input to generate new content. Examples of Generative AI techniques include Generative Adversarial Networks (GANs), Variational Autoencoders, and Transformers

##### What are GANs?

GANs, a generative AI technique, pit 2 networks against each other to generate new content. The algorithm consists of two competing networks: a generator and a discriminator.

+ A generator is a convolutional neural network (CNN) that learns to create new data resembling the source data it was
 trained on.
+ The discriminator is another convolutional neural network (CNN) that is trained to differentiate between real and
 synthetic data

The generator and the discriminator are trained in alternating cycles such that the generator learns to produce more and more realistic data while the discriminator iteratively gets better at learning to differentiate real data from the newly created data.

![aws deepcomposer](https://video.udacity-data.com/topher/2020/May/5eac7812_aws-deepcomoserr-gan-schema/aws-deepcomoserr-gan-schema.png)

Generator and discriminator are occurring asynchronously, the generator is learning to produce more realistitc data
and the discriminator is learning to differentiate real data from the newly created data.

#### Model Architecture
##### Generator

The generator network used in AWS DeepComposer is adapted from the U-Net architecture, a popular convolutional neural network that is used extensively in the computer vision domain.
The network consists of an “encoder” that maps the single track music data (represented as piano roll images) to a relatively lower dimensional “latent space“ and a ”decoder“ that maps the latent space back to multi-track music data.

Here are the inputs provided to the generator:

+ Single-track piano roll: A single melody track is provided as the input to the generator.
+ Noise vector: A latent noise vector is also passed in as an input and this is responsible for ensuring that there is
 a flavor to each output generated by the generator, even when the same input is provided.

![aws deepcomposer](https://video.udacity-data.com/topher/2020/May/5eada1b9_u-net/u-net.png)

##### Discriminator

The goal of the discriminator is to provide feedback to the generator about how realistic the generated piano rolls are, so that the generator can learn to produce more realistic data. The discriminator provides this feedback by outputting a scalar value that represents how “real” or “fake” a piano roll is.
Since the discriminator tries to classify data as “real” or “fake”, it is not very different from commonly used binary classifiers. We use a simple architecture for the critic, composed of four convolutional layers and a dense layer at the end.

![aws deepcomposer](https://video.udacity-data.com/topher/2020/May/5eada228_discriminator/discriminator.png)

Order steps in U-Net architecture : 
+ Input
+ Encoder
+ Latent Space
+ Decoder
+ Output

#### Training Methodology

During training, the generator and discriminator work in a tight loop as following:

##### Generator

+ The generator takes in a batch of single-track piano rolls (melody) as the input and generates a batch of multi
-track piano rolls as the output by adding accompaniments to each of the input music tracks.
+ The discriminator then takes these generated music tracks and predicts how far it deviates from the real data
 present in your training dataset.
 
##### Discriminator
 
+ This feedback from the discriminator is used by the generator to update its weights. As the generator gets better
 at creating music accompaniments, it begins fooling the discriminator. So, the discriminator needs to be retrained as well.
+ Beginning with the discriminator on the first iteration, we alternate between training these two networks until we
 reach some stop condition (ex: the algorithm has seen the entire dataset a certain number of times).

#### Finer control of AWS DeepComposer with hyperparameters

##### Number of epochs

When the training loop has passed through the entire training dataset once, we call that one epoch. Training for a higher number of epochs will mean your model will take longer to complete its training task, but it may produce better output if it has not yet converged. You will learn how to determine when a model has completed most of its training in the next section.

##### Learning Rate

The learning rate controls how rapidly the weights and biases of each network are updated during training. A higher learning rate might allow the network to explore a wider set of model weights, but might pass over more optimal weights.

##### Update ratio

A ratio of the number of times the discriminator is updated per generator training epoch. Updating the discriminator multiple times per generator training epoch is useful because it can improve the discriminators accuracy.

#### Evaluation

Typically when training any sort of model, it is a standard practice to monitor the value of the loss function throughout the duration of the training. The discriminator loss has been found to correlate well with sample quality. You should expect the discriminator loss to converge to zero and the generator loss to converge to some number which need not be zero. When the loss function plateaus, it is an indicator that the model is no longer learning. 

##### Sample output quality improves with more training

After 400 epochs of training, discriminator loss approaches near zero and the generator converges to a steady-state value. Loss is useful as an evaluation metric since the model will not improve as much or stop improving entirely when the loss plateaus.

![aws deepcomposer](https://video.udacity-data.com/topher/2020/May/5eada7ce_qualty-graph/qualty-graph.png)

#### Inference

The final process for music generation then is as follows:

+ Transform single-track music input into piano roll format.
+ Create a series of random numbers to represent the random noise vector.
+ Pass these as input to our trained generator model, producing a series of output piano rolls. Each output piano roll
 represents some instrument in the composition.
+ Transform the series of piano rolls back into a common music format (MIDI), assigning an instrument for each track.

#### Bonus : 

+ [Create compositions using sample models in music studio](https://console.aws.amazon.com/deepcomposer/home?region=us-east-1#musicStudio)
+ [Inspect the training of existing sample models](https://console.aws.amazon.com/deepcomposer/home?region=us-east-1#modelList)
+ [Train your own model within the AWS DeepComposer console](https://console.aws.amazon.com/deepcomposer/home?region=us-east-1#trainModel)
+ [Build your own GAN model](https://github.com/aws-samples/aws-deepcomposer-samples)
