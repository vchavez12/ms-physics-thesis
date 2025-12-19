# dnn_lambda-1405
 
 2024-12-22
>Started organizing these files for HADRON2025
>Generated datasets (1000 per label) with phase and without phase [check curriculum11 and 12]
> Haven't tried normalizing them. Noticed that the plan was not possible since for bb and tb ("unitary is not achieved").
> Trained using CNN2D model 80% for 0 and 60% for 1

Folders:
CNN2D-5_Adam-0_24-12-22: folder for training cnn for datasets with no phase and strength
CNN2D-5_Adam-1_24-12-22: folder for training cnn for datasets with random phase and strength

0: without phase
1: with phase

curriculum11_training: folder of datasets with no phase and strength (1000 per label)
curriculum12_training: folder of datasets with random phase and strength (1000 per label)

label   |   bt   |   bb   |   tb   |   mspp   |
00      |   0    |   0    |   0    |    0     |
01      |   1    |   0    |   0    |    0     |
02      |   0    |   1    |   0    |    0     |
03      |   0    |   0    |   1    |    0     |
04      |   2    |   0    |   0    |    0     | 
05      |   1    |   1    |   0    |    0     |
06      |   1    |   0    |   1    |    0     |
07      |   0    |   2    |   0    |    0     |
08      |   0    |   1    |   1    |    0     |
09      |   0    |   0    |   2    |    0     |
10      |   1    |   0    |   0    |    1     |
11      |   0    |   1    |   0    |    1     |
12      |   0    |   0    |   1    |    1     |
 
mspp - main-shadow pole pair

2025-01-15
Generating new datasets with correct strength and phase placements (based on physics[unitarity and optical theorem])
I'll try to train them again for HADRON2025 but all will be used for the full paper
Started 14:35 Ended: 14:53 (for 00, 01, 02, 03)
Started 14:55 Ended: 15:17 (for 04, 05, 06, 07)
Started 15:18 Ended: 15:33 (for 08, 09, 10, 11, 12)

The training for datasets with phase improved from 60 --> 70% accuracy.


Now, I'll be generating datsets for the full paper. Will be doing a curriculum training.
label   |   bt   |   bb   |   tb   |
#----------------------------------#
00      |   0    |   0    |   0    |
01      |   1    |   0    |   0    |
02      |   0    |   1    |   0    |
03      |   0    |   0    |   1    |
#----------------------------------#
04      |   2    |   0    |   0    |    ###
05      |   0    |   2    |   0    | 
06      |   0    |   0    |   2    |
#----------------------------------#
07      |   1    |   1    |   0    |    ###
08      |   1    |   0    |   1    |
09      |   0    |   1    |   1    |
#----------------------------------#
10      |   3    |   0    |   0    |
11      |   0    |   3    |   0    |
12      |   0    |   0    |   3    |
#----------------------------------#
13      |   2    |   1    |   0    |
14      |   2    |   0    |   1    |
15      |   1    |   2    |   0    |   ###
16      |   0    |   2    |   1    |
#----------------------------------#
17      |   1    |   0    |   2    |###
18      |   0    |   1    |   2    |
19      |   1    |   1    |   1    |###

Generatimg datasets
Start: 18:29; End: 19:29 (04,05,06,07)
Start: 20:02; End: 22:52 (08,09,10,11)
Start: 20:02; End: 22:52 (12,13,14,15)
Start: 22:52; End: 00:29  (16,17,18,19)

2025-01-16

For the full paper, we will do a curriculum training where the groups/curriculums are shown above.
300 epochs per curriculum

1st run:
curriculum | training accu | testing accu | training loss | testing loss |
    01     |     76.60%    |     75.39%   |     0.56      |      0.57    |  
    02     |     73.34%    |     72.18%   |     0.69      |      0.72    |  
    03     |     71.42%    |     70.27%   |     0.75      |      0.79    |
    04     |     70.63%    |     68.52%   |     0.77      |      0.83    |
    05     |     66.69%    |     65.39%   |     0.91      |      0.96    |
    06     |     63.94%    |     60.92%   |     1.00      |      1.10    |
    
2025-01-18
Now, since there are no MSPP dataset here, we will include it now since in HADRON2025 paper, it's favored by the trained DNN.
Let's hope it is also favored here in the full paper.

Generating datasets
Start: 13:04; End: 14:12 (20, 21, 22)

label   |   bt   |   bb   |   tb   |  mspp  |
#-------------------------------------------#
20      |   1    |   0    |   0    |   1    |
21      |   0    |   1    |   0    |   1    |
22      |   0    |   0    |   1    |   1    |

I'm gonna repeat the training since the model has changed, from 20 output nodes to 23. 
I'll change it to 30 so I can add more without repeating the training.

The training is slow (dnn does not learn) [my bad.  the learning rate was 1e-4 (too low haha)]

2nd run:
curriculum | training accu | testing accu | training loss | testing loss |
    01     |     88.28%    |     86.85%   |     0.29      |      0.31    |  
#    02     |     73.34%    |     72.18%   |     0.69      |      0.72    |  
#    03     |     71.42%    |     70.27%   |     0.75      |      0.79    |
#    04     |     70.63%    |     68.52%   |     0.77      |      0.83    |
#    05     |     66.69%    |     65.39%   |     0.91      |      0.96    |
#    06     |     63.94%    |     60.92%   |     1.00      |      1.10    |
    
Some realizations in training, we need to have a good initial hyperparameters (learning rate, batch size, etc). When I was traning this dataset, the early epochs remain at 25% accuracy (4 labels so 1/4) then, it will remain at that at 500 epochs so there is no learning happening. I used 1e-3 as learning rate and 64 batch size. I hope it is consistent with the next trainings.

Okay, now, I am experiencing a challenge in training the DNN. Since we're changing from 20 to 30 output nodes, there will be an increased in complexity in the architectures. That is why we are not getting the same accuracies as the previous run. Now, I am going to change the CNN and try to just run the datasets in HADRON2025 paper.

3rd Run: 2025-01-22
curriculum | training accu | testing accu | training loss | testing loss |
    01     |     80.99%    |     80.95%   |     0.45      |      0.45    |  
    04     |     74.56%    |     70.94%   |     0.69      |      0.82    |

4th Run: 2025-01-23
I forgot that I trained the HADRON2025 paper prior to proper implementation of the phase and strength. Now, we will train the DNN using the new datasets that we have generated.  

label   | text
00      | no pole
01      | one pole
02      | ambiguous 
03      | two poles without mspp
04      | two poles with mspp 

epoch | training accu | testing accu | training loss | testing loss |
1000  |     72.23%    |    72.90%    |     0.69      |     0.70     |

2025-01-24

I think I did not mention that I added points in the experimental data to extend the input nodes to 60 nodes (originally 54).
Now, I revert it back to the original and let's see if there's something interesting here.
I am generating 1000 plots per label. I am using the labelling in our full paper.

Start: 19:57 End: (00,01,02,03)

2025-01-29

I will be generating datasets for the lambda paper. 3000 datasets 
Start: 19:38 End: 22:30 (00,01,02,03)
Start: 22:42 End: 01:46 (04,05,06,07)
Start: 01:46 End: overnight(slept during the run) (08,09,10,11)
Start: 08:37 End: 11:42 (12,13,14,15)
Start: 11:44 End: 14:50 (16,17,18,19)

2025-01-30
I will be training these datasets. Nice!
I am training the datasets and I see that the NN can easily learn how to distinguish the pole structures. However, it always plateus at a number (75%) then returns to the initial state (25%). I don't know about this but I am thinking of the NN searching for the global minimum of the loss function. Yes, it can already attain 75% as the maximum accuracy but there must be a global maximum accuracy for this case. I am thinking of another way of training that uses this concept. Maybe training multiple times and see the accuracies. I don't know but let's try it tomorrow. I'm going to sleep first haha 


2025-01-31
Now, I am training the CNN using what we call curriculum training. We adjust the curriculum based on the preliminary predictions of the DNN. From 0-9, the inference is 1 or 7 which is reasonable hahaha. Let's see the other predicitons. 

This time, I tried training per group (0-3, 4-7, 8-11, 12-15, 16-19)
Let's see if there are confusions. If there is, we will train again with different groups. Confused labels are switched into the other group with also confused labels. We will repeat this process until we get a satisfactory accuracy and loss.
After training, we will use all the trained DNN to get an inference.
Then, the inferred labels will be trained again to see the ultimate inference. ultimate???!!!
Let's see how do we analyze the results from that.

2025-02-02
I trained the DNN, the inference is 7. Will generate more dataset for validation. 

2025-02-07
I saw an error in the codes. The reason why generating datasets take too much time is the unnecessary for-loops in Nimag part. 
I corrected the code and 10k datasets only took less than 5 mins. Nice! I don't have to ask Sir to run the codes for generating datasets

2025-02-08
Will try to train the new datasets (I increased the range of the imaginary part to 100MeV to accomodate the second broad pole in the literatures.

01: 00, 01, 02, 03 (01 - 100%)
02: 01, 04, 05, 06 [1 4], [41.046 58.954]
03: 04, 07, 08, 09 [4 7 8], [47.682 52.018  0.3  ]
04: 07, 10, 11, 12 [7 10], [97.148  2.852]
05: 07, 13, 14, 15 [7 13 15], [9.832e+01 1.638e+00 4.200e-02]% 7 gets confused with 15
01b: 07 15 
06: 07, 16, 17, 18 [7 16 17 18], [8.7282e+01 3.2000e-02 1.1826e+01 8.6000e-01] 
07: 07, 01, 04, 19 [7 1 19], [19.522 4.9600 75.516]
02b: 07, 19 

2025-02-10
I am now training the $\Sigma^-\pi^+$ invariant mass distribution.
01: 00, 01, 02, 03: [1], [100.]%
02: 01, 04, 05, 06: 
1????

2025-02-12
We're going to average the KN threshold by using the PDG values for p, n, K+, K0. 
Then, repeat the training for Sigma^-pi^+.
01: 00, 01, 02, 03: [1], [100]%
02: 01, 04, 05, 06: [1, 4], [98.316  1.684]%
03: 01, 07, 08, 09: [1], [100]%
04: 01, 10, 11, 12: [1], [100.]%
05: 01, 13, 14, 15: [1, 13, 14], [92.202  7.678  0.12 ]%
06: 01, 16, 17, 18: [1], [100.]%
07: 01, 04, 13, 19: [1, 4], [ 1.792 98.208]%

wtf??? this does not make sense.

2025-02-17
Alright I repeated the training and apparently, I mislabelled curr02. So it does infer label 4.

2025-02-18
For this week, I will be repeating the training for Sigma\pi line shape. (average threshold).

\Sigma^+\pi^-
Curr:01
              precision    recall  f1-score   support

           0       0.96      1.00      0.98     10000
           1       1.00      1.00      1.00     10000
           2       0.89      0.91      0.90     10000
           3       0.93      0.87      0.90     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

[1], [100.]%

Curr:02
              precision    recall  f1-score   support

           0       0.99      1.00      0.99     10000
           1       1.00      0.99      0.99     10000
           2       0.98      0.96      0.97     10000
           3       0.96      0.97      0.97     10000

    accuracy                           0.98     40000
   macro avg       0.98      0.98      0.98     40000
weighted avg       0.98      0.98      0.98     40000

[1, 4], [83.09 16.91]%

Curr:03
              precision    recall  f1-score   support

           0       0.93      0.99      0.96     10000
           1       0.95      0.89      0.92     10000
           2       0.91      0.90      0.91     10000
           3       0.99      1.00      0.99     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

 [1, 7, 8], [97.346  2.492  0.162]%

Curr: 04
              precision    recall  f1-score   support

           0       1.00      1.00      1.00     10000
           1       1.00      1.00      1.00     10000
           2       1.00      1.00      1.00     10000
           3       1.00      0.99      1.00     10000

    accuracy                           1.00     40000
   macro avg       1.00      1.00      1.00     40000
weighted avg       1.00      1.00      1.00     40000

[1], [100.]%

Curr: 05 

              precision    recall  f1-score   support

           0       0.99      1.00      0.99     10000
           1       0.90      0.85      0.88     10000
           2       0.87      0.91      0.89     10000
           3       0.98      0.99      0.98     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

[1, 13, 14, 15], [9.9204e+01 2.8000e-02 2.0000e-03 7.6600e-01]%

Curr:06

              precision    recall  f1-score   support

           0       0.98      1.00      0.99     10000
           1       0.78      0.79      0.79     10000
           2       0.99      0.98      0.99     10000
           3       0.78      0.78      0.78     10000

    accuracy                           0.88     40000
   macro avg       0.88      0.88      0.88     40000
weighted avg       0.88      0.88      0.88     40000

[1, 17], [98.886  1.114]%

Curr: 07

              precision    recall  f1-score   support

           0       0.95      0.99      0.97     10000
           1       0.99      0.99      0.99     10000
           2       0.83      0.81      0.82     10000
           3       0.83      0.81      0.82     10000

    accuracy                           0.90     40000
   macro avg       0.90      0.90      0.90     40000
weighted avg       0.90      0.90      0.90     40000


[1, 4, 7, 19], [9.2092e+01 6.2000e-02 5.5340e+00 2.3120e+00]%



2025-02-19
              precision    recall  f1-score   support

           1       0.99      0.99      0.99     10000
           2       0.89      0.92      0.91     10000
           3       0.91      0.89      0.90     10000
           4       1.00      0.99      0.99     10000

    accuracy                           0.95     40000
   macro avg       0.95      0.95      0.95     40000
weighted avg       0.95      0.95      0.95     40000

[1, 4], [6.0000e-03 9.9994e+01]%

Curr: 02

              precision    recall  f1-score   support

           4       1.00      0.99      1.00     10000
           5       0.97      0.96      0.96     10000
           6       0.95      0.97      0.96     10000
           7       0.99      0.99      0.99     10000

    accuracy                           0.98     40000
   macro avg       0.98      0.98      0.98     40000
weighted avg       0.98      0.98      0.98     40000
[4], [100.]%

Curr: 03

              precision    recall  f1-score   support

           4       0.96      0.97      0.97     10000
           8       0.98      0.97      0.97     10000
           9       0.99      1.00      1.00     10000
           10       0.99      0.98      0.98     10000

    accuracy                           0.98     40000
   macro avg       0.98      0.98      0.98     40000
weighted avg       0.98      0.98      0.98     40000

[4, 8, 10], [9.996e+01 3.400e-02 6.000e-03]%

Curr: 04
              precision    recall  f1-score   support

           4        0.93      0.96      0.95     10000
           11       1.00      0.99      1.00     10000
           12       0.99      1.00      0.99     10000
           13       0.96      0.92      0.94     10000

    accuracy                           0.97     40000
   macro avg       0.97      0.97      0.97     40000
weighted avg       0.97      0.97      0.97     40000

[4], [100.]%

Curr: 05

              precision    recall  f1-score   support

           4       0.93      0.96      0.95     10000
           14       0.96      0.93      0.95     10000
           15       0.98      0.97      0.98     10000
           16       0.97      0.99      0.98     10000

    accuracy                           0.96     40000
   macro avg       0.96      0.96      0.96     40000
weighted avg       0.96      0.96      0.96     40000


4], [100.]%

Curr: 06 

              precision    recall  f1-score   support

           4       0.98      0.98      0.98     10000
           17       0.81      0.83      0.82     10000
           18       0.98      0.98      0.98     10000
           19       0.83      0.80      0.81     10000

    accuracy                           0.90     40000
   macro avg       0.90      0.90      0.90     40000
weighted avg       0.90      0.90      0.90     40000

[4], [100.]%


2025-03-05
Will be generating \Sigma^0\pi^0 dataset. This completes the result of the paper.

Curr: 01 

              precision    recall  f1-score   support

           1       0.99      1.00      0.99     10000
           2       0.85      0.92      0.89     10000
           3       0.91      0.84      0.87     10000
           4       1.00      0.99      1.00     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

[1, 4], [ 0.268 99.732]%

Curr: 02

              precision    recall  f1-score   support

           4       1.00      0.99      0.99     10000
           5       0.97      0.97      0.97     10000
           6       0.96      0.97      0.97     10000
           7       0.99      0.99      0.99     10000

    accuracy                           0.98     40000
   macro avg       0.98      0.98      0.98     40000
weighted avg       0.98      0.98      0.98     40000
[4], [100.]%

Curr: 03

              precision    recall  f1-score   support

           4       0.95      0.97      0.96     10000
           8       0.97      0.98      0.98     10000
           9       1.00      1.00      1.00     10000
           10       0.99      0.97      0.98     10000

    accuracy                           0.98     40000
   macro avg       0.98      0.98      0.98     40000
weighted avg       0.98      0.98      0.98     40000

[4, 10], [88.972 11.028]%

Curr: 04
              precision    recall  f1-score   support

           4       0.91      0.96      0.94     10000
           11       1.00      1.00      1.00     10000
           12       0.99      1.00      0.99     10000
           13       0.96      0.91      0.93     10000

    accuracy                           0.97     40000
   macro avg       0.97      0.97      0.97     40000
weighted avg       0.97      0.97      0.97     40000

[4, 13], [9.995e+01 5.000e-02]%

Curr: 05

              precision    recall  f1-score   support

           04       0.93      0.95      0.94     10000
           14       0.95      0.92      0.93     10000
           15       0.98      0.97      0.98     10000
           16       0.98      0.99      0.99     10000

    accuracy                           0.96     40000
   macro avg       0.96      0.96      0.96     40000
weighted avg       0.96      0.96      0.96     40000

[4, 14], [9.9996e+01 4.0000e-03]%

Curr: 06

              precision    recall  f1-score   support

           04       0.98      0.98      0.98     10000
           17       0.81      0.83      0.82     10000
           18       0.99      0.99      0.99     10000
           19       0.82      0.81      0.82     10000

    accuracy                           0.90     40000
   macro avg       0.90      0.90      0.90     40000
weighted avg       0.90      0.90      0.90     40000

[4, 17, 19], [8.9466e+01 1.0512e+01 2.2000e-02]%

2025-03-12

Curr: 01

              precision    recall  f1-score   support

           0       0.99      0.99      0.99     10000
           1       0.86      0.94      0.90     10000
           2       0.92      0.84      0.88     10000
           3       1.00      0.99      1.00     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

[1, 4], [99.77  0.23]%

Curr: 02

              precision    recall  f1-score   support

           1       0.96      0.99      0.98     10000
           5       0.98      0.96      0.97     10000
           6       0.96      0.97      0.96     10000
           7       0.99      0.96      0.97     10000

    accuracy                           0.97     40000
   macro avg       0.97      0.97      0.97     40000
weighted avg       0.97      0.97      0.97     40000

[1, 7], [80.524 19.476]%

Curr: 03

              precision    recall  f1-score   support

           1       0.93      0.98      0.96     10000
           8       0.97      0.92      0.95     10000
           9       1.00      1.00      1.00     10000
          10       1.00      0.99      0.99     10000

    accuracy                           0.97     40000
   macro avg       0.97      0.97      0.97     40000
weighted avg       0.97      0.97      0.97     40000

[1, 8], [51.39 48.61]%

Curr: 04

              precision    recall  f1-score   support

           1       0.99      1.00      1.00     10000
          11       0.99      0.99      0.99     10000
          12       0.98      0.99      0.99     10000
          13       1.00      0.99      1.00     10000

    accuracy                           0.99     40000
   macro avg       0.99      0.99      0.99     40000
weighted avg       0.99      0.99      0.99     40000

[1, 13], [98.47  1.53]%

Curr: 05

              precision    recall  f1-score   support

           1       0.99      1.00      1.00     10000
          14       1.00      0.99      0.99     10000
          15       0.98      0.97      0.97     10000
          16       0.98      0.99      0.98     10000

    accuracy                           0.99     40000
   macro avg       0.99      0.99      0.99     40000
weighted avg       0.99      0.99      0.99     40000

[1, 14, 15], [68.614 18.44  12.946]%

Curr: 06

              precision    recall  f1-score   support

           1       0.97      1.00      0.98     10000
          17       0.82      0.83      0.82     10000
          18       0.98      0.99      0.99     10000
          19       0.82      0.79      0.80     10000

    accuracy                           0.90     40000
   macro avg       0.90      0.90      0.90     40000
weighted avg       0.90      0.90      0.90     40000

[1, 17, 19], [31.786 22.932 45.282]%

2025-05-06
+- 00 -+
CURR:01
              precision    recall  f1-score   support

           11       0.85      0.81      0.83     10000
           09       0.66      0.74      0.70     10000
           03       0.86      0.79      0.83     10000
           14       1.00      1.00      1.00     10000

    accuracy                           0.84     40000
   macro avg       0.84      0.84      0.84     40000
weighted avg       0.84      0.84      0.84     40000

['2 bt, 0 bb, 1 tb'], [100.]%

CURR:02
              precision    recall  f1-score   support

           14       0.99      0.99      0.99     10000
           17       0.99      0.99      0.99     10000
           05       0.96      0.95      0.96     10000
           06       0.95      0.97      0.96     10000

    accuracy                           0.97     40000
   macro avg       0.97      0.97      0.97     40000
weighted avg       0.97      0.97      0.97     40000

['1 bt, 0 bb, 2 tb'], [100.]%

CURR:03
              precision    recall  f1-score   support

           17       0.73      0.79      0.76     10000
           13       0.97      0.96      0.97     10000
           16       1.00      1.00      1.00     10000
           19       0.77      0.72      0.74     10000

    accuracy                           0.87     40000
   macro avg       0.87      0.87      0.87     40000
weighted avg       0.87      0.87      0.87     40000

['1 bt, 0 bb, 2 tb'], [100.]%

CURR:04
              precision    recall  f1-score   support

           18       0.92      0.84      0.88     10000
           08       0.94      0.97      0.95     10000
           17       0.97      0.94      0.95     10000
           12       0.85      0.93      0.89     10000

    accuracy                           0.92     40000
   macro avg       0.92      0.92      0.92     40000
weighted avg       0.92      0.92      0.92     40000

['1 bt, 0 bb, 2 tb'], [100.]%

CURR:05
              precision    recall  f1-score   support

           10       1.00      1.00      1.00     10000
           02       1.00      1.00      1.00     10000
           07       0.89      0.88      0.88     10000
           17       0.88      0.90      0.89     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

['1 bt, 0 bb, 2 tb'], [100.]%

CURR:06
              precision    recall  f1-score   support

           17       0.95      0.94      0.94     10000
           15       0.97      0.95      0.96     10000
           01       0.95      0.98      0.97     10000
           04       1.00      0.99      0.99     10000

    accuracy                           0.96     40000
   macro avg       0.96      0.96      0.96     40000
weighted avg       0.96      0.96      0.96     40000

['1 bt, 0 bb, 2 tb', '1 bt, 0 bb, 0 tb'], [ 1.322 98.678]%


00 -+ +-
CURR:01
              precision    recall  f1-score   support

           06       0.99      1.00      1.00     10000
           17       0.91      0.90      0.91     10000
           15       0.92      0.90      0.91     10000
           10       0.98      0.99      0.99     10000

    accuracy                           0.95     40000
   macro avg       0.95      0.95      0.95     40000
weighted avg       0.95      0.95      0.95     40000

['1 bt, 0 bb, 2 tb'], [100.]%

CURR:02
              precision    recall  f1-score   support

           17       0.86      0.89      0.88     10000
           04       0.99      0.98      0.98     10000
           07       0.89      0.87      0.88     10000
           02       1.00      1.00      1.00     10000

    accuracy                           0.93     40000
   macro avg       0.93      0.93      0.93     40000
weighted avg       0.93      0.93      0.93     40000

['1 bt, 0 bb, 2 tb'], [100.]%

CURR:03
              precision    recall  f1-score   support

           19       0.79      0.70      0.74     10000
           01       0.90      0.97      0.94     10000
           17       0.74      0.77      0.76     10000
           03       1.00      1.00      1.00     10000

    accuracy                           0.86     40000
   macro avg       0.86      0.86      0.86     40000
weighted avg       0.86      0.86      0.86     40000

['1 bt, 0 bb, 0 tb', '1 bt, 0 bb, 2 tb'], [91.846  8.154]%

CURR:04

              precision    recall  f1-score   support

           01       0.87      0.93      0.90     10000
           08       0.93      0.86      0.89     10000
           16       0.78      0.59      0.67     10000
           11       0.67      0.83      0.74     10000

    accuracy                           0.80     40000
   macro avg       0.81      0.80      0.80     40000
weighted avg       0.81      0.80      0.80     40000

['1 bt, 0 bb, 0 tb'], [100.]%

CURR:05
              precision    recall  f1-score   support

           12       1.00      1.00      1.00     10000
           13       0.91      0.82      0.86     10000
           01       0.99      1.00      1.00     10000
           14       0.83      0.91      0.87     10000

    accuracy                           0.93     40000
   macro avg       0.93      0.93      0.93     40000
weighted avg       0.93      0.93      0.93     40000

['2 bt, 1 bb, 0 tb', '1 bt, 0 bb, 0 tb', '2 bt, 0 bb, 1 tb'], [3.0540e+00 9.6932e+01 1.4000e-02]%

CURR:06
              precision    recall  f1-score   support

           09       0.52      0.39      0.44     10000
           05       0.59      0.65      0.62     10000
           18       0.60      0.70      0.65     10000
           01       1.00      1.00      1.00     10000

    accuracy                           0.68     40000
   macro avg       0.68      0.68      0.68     40000
weighted avg       0.68      0.68      0.68     40000

['1 bt, 0 bb, 0 tb'], [100.]%


-+ +- 00
CURR:01
              precision    recall  f1-score   support

           12       0.96      0.96      0.96     10000
           04       0.97      0.99      0.98     10000
           05       0.96      0.96      0.96     10000
           10       0.99      0.97      0.98     10000

    accuracy                           0.97     40000
   macro avg       0.97      0.97      0.97     40000
weighted avg       0.97      0.97      0.97     40000

['2 bt, 0 bb, 0 tb'], [100.]%

CURR:02
              precision    recall  f1-score   support

           04       0.99      0.99      0.99     10000
           03       0.87      0.93      0.90     10000
           19       0.99      0.99      0.99     10000
           02       0.92      0.87      0.89     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

['1 bt, 1 bb, 1 tb'], [100.]%

CURR:03
              precision    recall  f1-score   support

           19       0.83      0.75      0.79     10000
           17       0.77      0.85      0.81     10000
           13       0.98      0.98      0.98     10000
           16       1.00      1.00      1.00     10000

    accuracy                           0.89     40000
   macro avg       0.90      0.89      0.89     40000
weighted avg       0.90      0.89      0.89     40000

['1 bt, 1 bb, 1 tb', '1 bt, 0 bb, 2 tb'], [ 0.192 99.808]%

CURR:04
              precision    recall  f1-score   support

           17       0.89      0.91      0.90     10000
           14       0.95      0.96      0.95     10000
           11       1.00      1.00      1.00     10000
           15       0.93      0.89      0.91     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

['1 bt, 0 bb, 2 tb'], [100.]%

CURR:05
              precision    recall  f1-score   support

           18       0.73      0.74      0.74     10000
           01       0.95      0.98      0.97     10000
           17       0.98      0.95      0.96     10000
           09       0.74      0.73      0.73     10000

    accuracy                           0.85     40000
   macro avg       0.85      0.85      0.85     40000
weighted avg       0.85      0.85      0.85     40000

['1 bt, 0 bb, 0 tb', '1 bt, 0 bb, 2 tb'], [83.934 16.066]%

CURR:06
              precision    recall  f1-score   support

           07       0.93      0.87      0.90     10000
           06       1.00      1.00      1.00     10000
           08       0.89      0.81      0.85     10000
           01       0.82      0.94      0.87     10000

    accuracy                           0.90     40000
   macro avg       0.91      0.90      0.90     40000
weighted avg       0.91      0.90      0.90     40000

['1 bt, 0 bb, 0 tb'], [100.]%

2025-05-19 and 23
I ran a training using the feed forward nn. The results are a bit disappointing as the it only attained 60% accuracy for LModel1 and 40% accuracy for LModel3. Let's discuss a bit about this in the paper but include a longer discussion in the thesis.

2025-06-18
I ran the ordered 3-conv training. -+ +- 00

CURR:01
              precision    recall  f1-score   support

           0       1.00      1.00      1.00     10000
           1       0.92      0.89      0.91     10000
           2       0.90      0.92      0.91     10000
           3       1.00      1.00      1.00     10000

    accuracy                           0.95     40000
   macro avg       0.95      0.95      0.95     40000
weighted avg       0.95      0.95      0.95     40000

['1 bt, 0 bb, 0 tb', '2 bt, 0 bb, 0 tb'], [9.9998e+01 2.0000e-03]%

CURR:02
              precision    recall  f1-score   support

           0       0.93      0.97      0.95     10000
           1       0.94      0.93      0.94     10000
           2       0.93      0.95      0.94     10000
           3       0.97      0.93      0.95     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

['1 bt, 0 bb, 0 tb'], [100.]%

CURR:03
              precision    recall  f1-score   support

           0       0.85      0.94      0.89     10000
           1       0.93      0.83      0.88     10000
           2       1.00      1.00      1.00     10000
           3       1.00      1.00      1.00     10000

    accuracy                           0.94     40000
   macro avg       0.94      0.94      0.94     40000
weighted avg       0.94      0.94      0.94     40000

['1 bt, 0 bb, 0 tb'], [100.]%

CURR:04

              precision    recall  f1-score   support

           0       0.99      1.00      1.00     10000
           1       1.00      1.00      1.00     10000
           2       1.00      1.00      1.00     10000
           3       1.00      0.99      1.00     10000

    accuracy                           1.00     40000
   macro avg       1.00      1.00      1.00     40000
weighted avg       1.00      1.00      1.00     40000

['1 bt, 0 bb, 0 tb', '2 bt, 1 bb, 0 tb'], [9.9998e+01 2.0000e-03]%

CURR:05
              precision    recall  f1-score   support

           0       0.95      0.98      0.96     10000
           1       0.98      0.98      0.98     10000
           2       0.97      0.95      0.96     10000
           3       0.99      1.00      0.99     10000

    accuracy                           0.97     40000
   macro avg       0.97      0.97      0.97     40000
weighted avg       0.97      0.97      0.97     40000

['1 bt, 0 bb, 0 tb'], [100.]%

CURR:06
              precision    recall  f1-score   support

           0       0.91      0.99      0.95     10000
           1       0.77      0.74      0.75     10000
           2       1.00      1.00      1.00     10000
           3       0.77      0.73      0.75     10000

    accuracy                           0.86     40000
   macro avg       0.86      0.86      0.86     40000
weighted avg       0.86      0.86      0.86     40000

['1 bt, 0 bb, 0 tb', '1 bt, 0 bb, 2 tb'], [97.57  2.43]%


2025-09-27
curriculum | training accu | testing accu | training loss | testing loss |
    01     |     76.60%    |     75.39%   |     0.56      |      0.57    |  
    02     |     73.34%    |     72.18%   |     0.69      |      0.72    |  
    03     |     71.42%    |     70.27%   |     0.75      |      0.79    |
    04     |     70.63%    |     68.52%   |     0.77      |      0.83    |
    05     |     66.69%    |     65.39%   |     0.91      |      0.96    |
    06     |     63.94%    |     60.92%   |     1.00      |      1.10  

