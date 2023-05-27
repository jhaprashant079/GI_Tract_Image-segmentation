# GI_Tract_Image-segmentation
https://www.kaggle.com/competitions/uw-madison-gi-tract-image-segmentation
In this task we have to segment MRI image of Gastro Instestinal Tract.The classes to segment are of stomach, large instentine and small instestine.
## Necessity:
There are large number of people suffering from a cancer of the gastro-intestinal tract.Radiation oncologists try to deliver high doses of radiation using X-ray beams pointed to tumors while avoiding the stomach and intestines.They manually outline  position of stomach and intestines in MRI scans of cancer region which is quite time consuming and labor intensive which also makes it difficult for patient to tolerate.
This competition attempts to automate that process of segmentation and increase the accuracy while decreasing time consumption for higher radiation doses by oncologists.

### Backbone Architecture
Used several EfficientNet variants 

### Segmentation Model
Segmentation Model used was 2D-Unet

### Loss Function
2D-Unet with efficientNet-b4 backbone was trained with mixed loss of "Binary Cross Entropy Loss" and "Tversky Loss".Binary cross entropy Loss is one of the common losses proved to be good in segmentation tasks.Tversky loss ensures robust performance in skewed dataset.

## Dataset preparation
* Method 1)Given Dataset has **images** in grayscale so for input in the unet model which need 3 channels, the grayscale was replicated 3 times in RGB channels.
**Masks** for trainig were given in rle encoded format so this was decoded to pass in dataloader.
* Method 2) ***Novelty:*** as the MRI scans were sequentially given over time, there was a need to make use of time data.So a simple but powerful novel approach of making trainig images by putting sequential images in RGB channels was used.With as stride of 2 current image is built by stacking previous image ,current image and next image in sequence.
**Mask** Mask remains as of the current image signifying current image information is not just the currnet image but some more image in sequence.And as the sequmce was slow enough,keeping current mask was ok.

## Results:
**Metric:** Evaluation metrics used *mean Dice coefficient* and *3D Hausdorff distance*. The two metrics are combined, with a weight of 0.4 for the Dice metric and 0.6 for the Hausdorff distance.
* With above configurations and methods the naive pipeline showed a Private Leaderboard Score of 0.82472 and Public Score of 0.83609.
* The novel pipeline outperformed tha naive one giving a Private Leaderboard Score of 0.84796 and Public Score 0.85735.
