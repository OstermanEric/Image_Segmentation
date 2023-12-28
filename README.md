# K-means Clustering & Thresholding

K-means clustering and thresholding and the segment anything model (SAM) were used to segment the lungs and airways from the different slices from the CT-CASE 15 dataset. For both models, the necessary packages and imports were installed as well as the data set which was unzipped and loaded in slice by slice. The K-means model classifies patterns more similar to each other such as the lungs and airways vs the softer tissue using the image after checking and scaling pixels to Hounsfield units. After this step, the mask is generated using k-means clustering to identify a threshold for the mask. The results of generating the mask, eroding & dilating the mask, the final mask, and the mask applied to the original image are shown below: 


![k-means_mask](https://github.com/OstermanEric/Image_Segmentation/assets/96360406/e8ff4395-f895-4d46-be1f-80b88b707cb9)











These results show how well the k-means thresholding worked for generating a mask and applying it to the original image. 

# SAM
For the SAM method, the DICOM CT files had to first be converted to a PNG or JPEG file which was done with a custom function written in our colab. Once this was done, the image could be passed into a mask generator method which automatically segments different areas of the image. The results are below:



![SAM_everything](https://github.com/OstermanEric/Image_Segmentation/assets/96360406/693badc9-755e-488b-8fb3-10b557e0c506)




The segmented image on the right is the result of the SAM model automatically creating a mask, applying it, and segmenting the image itself with no other input. The model segments almost everything individually as shown by the plethora of colors. The next mask creation method we used with SAM was a user-determined bounding box which is shown below:

<img width="456" alt="Screenshot 2023-12-27 at 8 44 02 PM" src="https://github.com/OstermanEric/Image_Segmentation/assets/96360406/44f5bfe6-9786-4aab-8f0b-5dfa92e256d3">



This image shows an example of a bounding box drawn by the user. This box tells the SAM model which area to use when creating a mask and segmenting. It can be extremely accurate especially if the box is tightly drawn around the intended area as shown below:

![SAM_box](https://github.com/OstermanEric/Image_Segmentation/assets/96360406/107d444b-891d-4744-bb2d-90d7246fbd81)



The segmentation done (right side) is extremely accurate and essentially covers the entire lungs and airways. Remarkably, the model is accurate from only the box and image as the two user inputs. The masks themselves that the model generated are shown below: 

![SAM_mask](https://github.com/OstermanEric/Image_Segmentation/assets/96360406/4f2c533b-0292-45fd-ade5-9dad946fa0c0)


These masks are what were generated from the SAM model using the bounding box from the figure above. The model does a great job of separating the lungs and airways from the rest of the image. Lastly, we used two sets of points on the image (150, 300), and (350, 300) to guide the model on where to segment. The model generated 3 masks with varying scores and the results are shown below: 


![SAM_coords](https://github.com/OstermanEric/Image_Segmentation/assets/96360406/a77afdb1-9cde-40ec-8b86-21e50f17c192)






These masks were generated using two sets of coordinates from the original image. The model generated three masks with varying intersection over union (IoU) scores. The leftmost mask has a score almost equal to 1 which would indicate that it is a perfect match between the projected box and the ground truth box which is incredible. The other two boxes have a decent IoU score but as you can tell, have not segmented the image correctly. 

# Discussion
Both methods work exceptionally well in terms of thresholding the image. K-means is simple to use and implement, guarantees convergence, and is easily adaptable. The model responds well to incoming data and does not require much implementation. However, K-means does have some drawbacks such as having to choose K manually. This process is usually done through experimentation which can be time-consuming. Another drawback is that outliers can impact the results negatively. Due to how the clustering algorithm works, outliers can impact the implementation more so than other algorithms.

The SAM model also has its advantages and disadvantages. SAM is easy to use and promptable, making it extremely portable, easy to use, and offers a variety of methods or prompts to choose from when segmenting. These prompts include boundary boxes, passing in one or multiple points from the image, or asking it to segment everything. Although easy to use, SAM requires a ton of computing power and needs to be run on a GPU. This can take a while and cost money depending on the services or hardware one is working with. Another drawback is that this model is extremely new, coming out within the past 2 years. This means that there is minimal documentation and guides on how to troubleshoot the model and does not allow many many different types of mediums such as DICOM. 

