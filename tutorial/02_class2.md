During this class we are going to run a jupyter notebook using an interactive session.

For this we are going to use the first lab presented as interactive labs presented in Bravo, C., Maldonado, S., & Oskarsdottir, M. (2026). Deep Learning in Banking: Integrating Artificial Intelligence for Next-Generation Financial Services. John Wiley & Sons.

Now go to the book page (if possible) https://www.bankingbook.ml/ and download the lab into the local and upload to the cluster (to practice!)

If you could not access to the internet lets copy the file directly into our desired folder

- In our tutorial folder (in my case tutorial_hpc_bcp) folder.
- Create a folder for the second class
- mkdir class2
- download the file into that folder wget -O lab2.ipynb https://github.com/Banking-Analytics-Lab/DLinBankingBook/blob/main/Labs/TextBook_Lab_Chap2_Image_Processing.ipynb (Model with Lidar images in this case to predict the loan delinquency ratio)

For runing the previous notebook in the cluster we need to 

- Activate the environment and add te extra packages required for the notebook

  module load gcc
  module load cuda
  module load opencv
  source /home/salfonso/p3_env_nvl_test/bin/activate
  pip install --no-index gdown ipywidgets torchcam livelossplot opencv-python torchvision

 a. Get the data (in login node)
 - gdown  'https://drive.google.com/file/d/1uDziz14xOgUTmW2DxPufuQMATL6O8Ial/view?usp=sharing' -O /home/salfonso/projects/def-cbravo/salfonso/tutorial_hpc_bcp/usg_lidar.zip
 - gdown  'https://drive.google.com/file/d/1-YWiPPtLQmkuWjcZcckM9vgv4Fsm68Je/view?usp=sharing' -O /home/salfonso/projects/def-cbravo/salfonso/tutorial_hpc_bcp/sample_data.csv
 - unzip the first file
 - unzip usg_lidar.zip
   b. Also we need to get the weights for the ResNet50 model that is going to be used in the script in class2 create the folder
   - mkdir resnet50_model_weights
   - nano resnet50w_download.py
     '''text
     import os
     import torch
     from torchvision.models import resnet50, ResNet50_Weights
    
      save_path = "/home/salfonso/projects/def-cbravo/salfonso/tutorial_hpc_bcp/class2/resnet50_model_weights/resnet50_imagenet.pth"
    
    # Create the directory if it does not exist
    os.makedirs(os.path.dirname(save_path), exist_ok=True)
    
    # Download the pretrained model
    model = resnet50(weights=ResNet50_Weights.DEFAULT)
    
    # Save the weights directly to the desired location
    torch.save(model.state_dict(), save_path)
    
    print(f"Weights saved to: {save_path}") '''
   - run the previous code in the login node to get the weights, this is in the resnet50_model_weights, python resnet50w_download.py

  Now lets ask for the interactive section, in this case 

salloc --account=def-cbravo --gpus=a100_2g.10gb:1 --cpus-per-task=1 --mem-per-cpu=2G --time=0:40:00 srun $VIRTUAL_ENV/bin/notebook.sh
  
-In the file lets change some parts to be able to run it in the interactive session (Remmber to use  in your local terminal ssh -L 8888:ngXXXX.narval.calcul.quebec:XXXX salfonso@narval.alliancecan.ca)
    * Coment all the pip statements and the unzip comment since that we did previously
    
    * After the first model is trained change 
    model_path = "/content/drive/MyDrive/Colab Notebooks/DL in Banking Book/DeepLearningInBankingBook/TextBook_Lab/c2_best_simple_cnn_1e-5_top.pth" to
    model_path = "best_simple_cnn.pth"
    
    * And in the sectio Training a ResNet-50 for Loan Delinquency Prediction
    Add the WEIGHTS_PATH = "resnet50_model_weights/resnet50_imagenet.pth"
    and change  self.backbone = models.resnet50(weights='DEFAULT') to :
      self.backbone = models.resnet50(weights=None)
      self.backbone.load_state_dict(torch.load(WEIGHTS_PATH)



### Converting the script to .py and submit the job
Since for runing the scripts in an interactive session has a time limit and the connection could be fragile (internet connection-tunnel). This is usually used for looking all is runing fine and to get a sense about the time and resources that were used.

In the following part we are going to convert the notebook file to a python, in the class folder convert the file to .py script

jupyter nbconvert --to script lab2.ipynb

This generates a lab2.py file

1. Remove
get_ipython().run_line_magic('matplotlib', 'inline')
from IPython.display import Image as DisplayImage

2, Replace 
DisplayImage(filename='usgs_lidar/10180_796_USGS_13_n33w101_20191031.jpg')
DisplayImage(filename='usgs_lidar/nan_835_USGS_13_n47w115_20241127.jpg')

img = Image.open('usgs_lidar/10180_796_USGS_13_n33w101_20191031.jpg')
plt.imshow(img, cmap='gray')
plt.axis("off")
plt.savefig("example_lidar_1.png", bbox_inches="tight")
plt.close()




   
