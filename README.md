# idfy_task :: Assignment - Machine Learning
## Objective
The objective of this assignment is to build an OCR solution forthe provided dataset. This specific
dataset is normal and HDR readings of license plates.

### System Configuration
- Processor : Intel® Core™ i3-8130U
- RAM : 16 GB

### Module Version
- python version 	= 3.6
- numpy  	 	= 1.19.4
- pandas 	 	= 1.1.4
- opencv 	 	= 3.4.2
- matplotlib 		= 3.3.3
- tqdm              	= 4.54.1
- tensorflow 		= 1.4.0
- requests		= 2.25.1

### Tools Used
- Neural network : [Yolo V2](https://github.com/thtrieu/darkflow)
- [LabelImg](https://github.com/tzutalin/labelImg)

### Why I am using YOLO ?
- There are many conventional options to perform OCR and after visualizing the dataset, it can be done way more easily than using big architecture like YOLO.
- I wanted to try this out for a very long time and now I have a reason for it.
- In my previous company, I made changes in this architecture in medical domain and from then only I wanted to try this on this type of task.

### Steps
	
1. ISSUE 1 : jupyter auto complete is not working.  <br /> 
fix: https://stackoverflow.com/questions/40536560/ipython-and-jupyter-autocomplete-not-working   <br /> 
https://github.com/ipython/ipython/issues/10493
	
2. took first look of dataset.csv

3. mapped each IMAGE in dataset to respective ANNOTATION

4. started visualizing dataset  <br /> 
	4.1. extracted type (HDR/normal), shape (height,width,channel) information for each image in dataset.  <br /> 
		4.1.1. saved image with above details as new images for proper visualization.  <br /> 
		4.1.2. saved each image data in csv file for later use.  <br /> 
		
	4.2. Only 282 images were stored in for visualization in first run. <br /> 
  `REASON`: We are having repeated filenames in both HDR and NORMAL images, and I was saving images using their quality as prefix and original image name as next part (eg: HDR_I00001.png OR NORMAL_100001.png) so, images were overwritten.
  ![alt text](https://github.com/abhishek795jha/idfy_task/blob/main/readme_images/fig_1.png?raw=true)
  `SOLUTION`: I stored each using the number plate reading they contain as suffix part. (eg: HDR_9B52145.png OR NORMAL_9B52145.png) <br /> 
  4.3. In second run, 604 images were stored. <br /> 
  `REASON`: There are images which contains the same number plate but are saved using different names.
  TOTAL 48 IMAGES which contain image of same number plate. <br /> 
  ![alt text](https://github.com/abhishek795jha/idfy_task/blob/main/readme_images/fig_2.png?raw=true)
  `SOLUTION`: Manually visualized the repeated images and removed those which were having least visible plates among them. <br /> 
 
  5. Starting to annotate 604 images using LabelImg tool. <br /> 
    5.1. Annotated 299 HDR images. <br /> 
	  5.2. Found some images which are not visible at all, so i removed them from both directory. (HDR and NORMAL images) <br /> 
	  5.3. Cross-referenced HDR images with NORMAL images. <br />
		`REASON`: If images containing same number plate had same dimension in bot directories then annotation will be same. <br /> 
	  5.4. Generated annotations for NORMAL images. <br /> 
	  5.5. Merged both folders. (HDR and NORMAL image and their annotations) <br /> 
	  5.6. Total = 598 images and annotations. <br /> 
	
6. Split data in the ration on 80:20 (train:test) <br /> 
	6.1. **Train data : 479** <br /> <t/ >**Test data  : 119** <br /> 
	6.2. Instance of each class present in training data. <br /> 
	
		:: ALPHABETS ::
		A : 28
		B : 384
		C : 23
		D : 6
		E : 12
		F : 3
		G : 2
		H : 4
		I : 9
		J : 10
		K : 1
		L : 9
		M : 17
		N : 4
		O : 0
		P : 3
		Q : 0
		R : 1
		S : 7
		T : 17
		U : 0
		V : 2
		W : 0 
		X : 3
		Y : 0
		Z : 42
		
		:: NUMBERS ::
		0 : 239
		1 : 271
		2 : 259
		3 : 243
		4 : 277
		5 : 267
		6 : 272
		7 : 278
		8 : 298
		9 : 259


7. Training: <br /> 
	7.0. check theses changes: <br /> 
    7.0.1. updated number of classes : 36 classes <br /> 
		7.0.2. updated classes name text file <br /> 
		7.0.3. change config file (batch size, resolution,checkpoint) <br /> 
		7.0.4. add line to hide warnings of tensorflow <br /> 
		
	** Architecture of YOLO v2** <br/>
	![alt text](https://github.com/abhishek795jha/idfy_task/blob/main/readme_images/fig_3.png?raw=true) 
    
	7.1. `[FROM SCRATCH]` Started training using YOLO v2 tiny. Trained for 25 epochs --- NO RESULT <br /> 
  
	7.2. `[FROM SCRATCH]` Started training with YOLO V2. Trained for 12 epochs 	  --- NO RESULT <br /> 
  
	7.3. `[USING PRE-TRAINED WEIGHTS]` Started training using YOLO v2 tiny. Trained for 10 epochs --- NO RESULT <br /> 
  
	7.4. `[USING PRE-TRAINED WEIGHTS]` Started training using YOLO v2. Trained for 5 epochs 	--- NO RESULT <br /> 
	
	**NOTE : Traied training with changing these configurations:** <br /> 
		  (a) Network input shape : 288x288, 352x352, 416x416, 480x480, 512x512, 640x640, 768x768, 1024x1024 <br /> 
		  (b) Batch sizes (according to my memory and network architecture) : 4, 8, 16, 32 <br /> 
		  (c) Threshold value for detection: 0.01, 0.05, 0.06, 0.07, 0.08, 0.09, 0.1, 0.2, 0.3, 0.5 <br /> 
		  
      
	## IMPORTANT:
  
  As, I was not getting any detection even on very low threshold value (not even wrong detection), <br /> 
  I downloaded the original weights and configuration file and ran the inference on sample images (like car,dog, people) but there was no detection at all.<br />
  So, there is a high posibility that the repository i used to implemnt YOLO is not working in first place.

## Future work
- I started to find the flow in this repo or I will start writing the code for YOLO v3 in keras.
- I need to get some more data of number plates.
- WIll annotate characters manually using LableImg
- Will have to use data augmentation methods to increase size of dataset.

- One ther thing i will try is to use two step approach. First detect number plate in image and then perform OCR on it.
