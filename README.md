# kaggle_CycleGan_Monet
This project is to use Cycle GAN to transform regular photos into Monet style .
STEP1 The Problem we are trying to solve is to convert 7k to 10k realistic photos into monet style images.  We have 300 monet style images and 7038 regular photos to convert. They are all 256 x 256 (x3) images. The goal is to use a CycleGan to feed the photos to a genrater to transform the regular photo into a monet sytle photo and then run it through a discrimnator and find the loss(in this case the loss is scored on MiFID) and improve the generator with better outputs to also improve the discrimator.

Step 2 EDA it was very challenging to learn how to imput the the tfrec images once i did and decoding jpegs I made sure they were all 256*256 and scaled them to -1,1 for the tanh used in cycle gan. Since there was heavy imbalance with the data set where we only have 300 monet photos and 7k+ regular i experemented with different batches and shuffling the data.

I printed a number of monet and photo to make sure the they came through clean and not with error.

I also looked at the average pixel intensity and learned that the monet photos are a bit darker then the photos and also a bit higher standard deviation in intensity then the photos.

Step 3 model building: I decided to do a traditional cycle gan. with an encoder/downsampling a resiual block to try and maintain the key features/styles of the monet and then a decoder/upscaling. I chained these together to get 2 Generators to crate image from my generator builds(monet to photo m2p and photo to monet p2m)

I also then built my two discrimators using the ecoders to downsample and compare the accuracy to the photos and monet inputs.

I ran into a lot of issues where the model would return specific colors ie all my images were blue, then gold, so i figured out I had to fix my identity loss function.
i also experiemented and added some smoothing with discrimator loss of monet phots by softening it (*.9) strength to reduce overfitting.

I also tried training with different batch sizes and learning rates. ie 1e4 and 2e4 sizes 64,16,8,4.


Step 4 Results I ran a number of scenarios via kaggle notebook and my jupyter notebook. I scored a 125 on the leaderboard which I attribute to not being able to handle many epochs because of how long it took to run.  My notebook where i ran 20 epochs my training converaged fairly stably and kept making incrimental improvements on the generator loss. it would have been beneficial to keep running more epochs as the improvement did not completely flatten out.

Step 5 Conclusion smaller batch sizes lead to longer epoch runs.  I could have been a little more agressive on learning rate and converaged the model faster without so many epochs and run times. I also could have reduced how many residual blocks and limited the number of steps in an epoch. Since we only had 300 monet images I could have augmented both with some rotations and flips to help generalize the data.
