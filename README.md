# Homework 2 (Style-Transfer)

## Training MUNIT

For training, we use the default demo config:
```
bash scripts/demo_train_summer2winter_yosemite256.sh
```
In this assignment, we use the `summer2winter_yosemite256` dataset (not the HD version), which is introduced in the UNIT paper. Which contains summer and winter street images extracted from real-world driving videos.

We show the tensorboard logging of the overall discriminator loss and generator loss below as an evidence of our training process (540k iter.).

D-Loss | G-Loss |
---    | ---  |
<img src="logs/summer2winter_yosemite256_folder/loss_dis.png" alt="drawing" width="250"/> | <img src="logs/summer2winter_yosemite256_folder/loss_gen.png" alt="drawing" width="250"/> |


iter. | Summer → Winter|
---    | ---  |
10k   | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00010000.jpg" alt="drawing" /> |
20k   | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00020000.jpg" alt="drawing" /> |
30k   | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00030000.jpg" alt="drawing" /> |
110k  | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00110000.jpg" alt="drawing" /> |
120k  | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00120000.jpg" alt="drawing" /> |
130k  | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00130000.jpg" alt="drawing" /> |
310k  | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00310000.jpg" alt="drawing" /> |
320k  | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00320000.jpg" alt="drawing" /> |
330k  | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00330000.jpg" alt="drawing" /> |
540k  | <img src="outputs/summer2winter_yosemite256_folder/images/gen_a2b_test_00540000.jpg" alt="drawing" /> |



## Inference one image in multiple style

Input | Style-A | Style-B | Style-C | Style-D | Style-E | Style-F | Style-G |
---   | ---     | ---     | ---     | ---     | ---     | --- | --- |
<img src="results/edges2shoes/input.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output000.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output001.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output002.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output003.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output004.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output005.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output006.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output007.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output008.jpg" alt="drawing" width="50"/> | <img src="results/edges2shoes/output009.jpg" alt="drawing" width="50"/> |


## Compare with other methods

First we compare the result to one CVPR'17 paper, pix2pix. Note that Pix2Pix can only do one → one mapping.

Input | Style-A |
---   | ---     |
<img src="results/edges2shoes/input.jpg" alt="drawing" width="50"/> | <img src="inputs/pix.png" alt="drawing" width="50"/> |

Another paper BicycleGAN can solve this problem (one → one mapping). 
Just like the `cycle consistency` we have tried in CycleGAN before, we have to add constraints on the consistency of the shared domain (Which is domain `Y`). As the table shown below, BicycleGAN integrate the `xy_Backward` loss and `yz_Forward` loss together as our special `bi-cycle-loss`. So that shared domain `Y` keeps the transform between `X` to `Z` adversarial. 

| Separate Cycles | Joint Cycles |
|:-----:|:---:|
|<img src="./inputs/single-cycle.png" alt="drawing" width="250"/>|<img src="./inputs/bi-cycle-2.png" alt="drawing" width="250"/>|

For fair comparions, we use the same testing image among all the methods.

Input | Encoded | Style-B | Style-C | Style-D | Style-E | Style-F | Style-G |
---   | ---     | ---     | ---     | ---     | ---     | --- | --- |
<img src="results/edges2shoes/input.jpg" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_encoded.png" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample01.png" alt="drawing" width="50" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample02.png" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample03.png" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample04.png" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample05.png" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample06.png" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample07.png" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample08.png" alt="drawing" width="50"/> | <img src="outputs/bi/input_000_random_sample09.png" alt="drawing" width="50"/> |

The generated image quality from both MUNIT and Pix2Pix are good. However, as mentioned above, Pix2Pix only supports one to one mapping. Comparing to another one to many mapping method, BicycleGAN, the image quality generated by MUNIT clearly outperform the one generated by BicleGAN. Through recombining its `content code` with a random `style code` sampled from the style space of the target domain, MUNIT shows great advantage comparing to either Pix2Pix or BicycleGAN.
