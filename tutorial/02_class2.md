# 🚀 Class 2 – Running Deep Learning Workflows on HPC

During this class we are going to run a Jupyter notebook using an interactive session.

For this we are going to use the first lab presented as interactive labs presented in Bravo, C., Maldonado, S., & Oskarsdottir, M. (2026). *Deep Learning in Banking: Integrating Artificial Intelligence for Next-Generation Financial Services*. John Wiley & Sons.

# 📥 1. Downloading the notebook

Now go to the book page (if possible) https://www.bankingbook.ml/ and download the lab into the local and upload to the cluster (to practice!)

# 🗂️ 2. Preparing the working directory

If you could not access to the internet lets copy the file directly into our desired folder

- In our tutorial folder (in my case tutorial_hpc_bcp) folder.
- Create a folder for the second class
- mkdir class2
- download the file into that folder wget -O lab2.ipynb https://github.com/Banking-Analytics-Lab/DLinBankingBook/blob/main/Labs/TextBook_Lab_Chap2_Image_Processing.ipynb (Model with Lidar images in this case to predict the loan delinquency ratio)

# 🗂️ 2. Preparing the working directory

For runing the previous notebook in the cluster we need to 

- Activate the environment and add te extra packages required for the notebook.

```text
  #!/bin/bash
  #

  module load python
  module scipy-stack
  module list
  module load gcc
  module load cuda
  module load opencv
  source /home/salfonso/p3_env_nvl_test/bin/activate
  ```
  We can create the bcpt_env.sh file to activate the env with the modules and activate it with : source bct_env.sh 
  
  pip install --no-index gdown ipywidgets torchcam livelossplot opencv-python torchvision

 a. Get the data (in login node)
 - gdown  'https://drive.google.com/file/d/1uDziz14xOgUTmW2DxPufuQMATL6O8Ial/view?usp=sharing' -O /home/salfonso/projects/def-cbravo/salfonso/tutorial_hpc_bcp/usg_lidar.zip
 - gdown  'https://drive.google.com/file/d/1-YWiPPtLQmkuWjcZcckM9vgv4Fsm68Je/view?usp=sharing' -O /home/salfonso/projects/def-cbravo/salfonso/tutorial_hpc_bcp/sample_data.csv
 - unzip the first file
 - unzip usg_lidar.zip
   b. Also we need to get the weights for the ResNet50 model that is going to be used in the script in class2 create the folder
   - mkdir resnet50_model_weights
   - nano resnet50w_download.py
   
     ```text
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
      
      print(f"Weights saved to: {save_path}")
   ```
   - run the previous code in the login node to get the weights, this is in the resnet50_model_weights, python resnet50w_download.py

# 💻 6. Launching an interactive Jupyter session

Now lets ask for the interactive section, in this case 

salloc --account=def-cbravo --gpus=a100_2g.10gb:1 --cpus-per-task=1 --mem-per-cpu=2G --time=0:40:00 srun $VIRTUAL_ENV/bin/notebook.sh
  
-In the file lets change some parts to be able to run it in the interactive session (Remmber to use  in your local terminal ssh -L 8888:ngXXXX.narval.calcul.quebec:XXXX salfonso@narval.alliancecan.ca)

    * Coment all the pip statements and the unzip comment since  we did it previously
    
    * After the first model is trained change 
    model_path = "/content/drive/MyDrive/Colab Notebooks/DL in Banking Book/DeepLearningInBankingBook/TextBook_Lab/c2_best_simple_cnn_1e-5_top.pth" to
    model_path = "best_simple_cnn.pth"
    
    
    * And in the section Training a ResNet-50 for Loan Delinquency Prediction

    Add the WEIGHTS_PATH = "resnet50_model_weights/resnet50_imagenet.pth"
    and change  self.backbone = models.resnet50(weights='DEFAULT') to :
    self.backbone = models.resnet50(weights=None)
    self.backbone.load_state_dict(torch.load(WEIGHTS_PATH)

    * change plt.savefig('/content/drive/MyDrive/Colab Notebooks/DL in Banking Book/Images/C2_SmoothGradCam.pdf') to
    plt.savefig('C2_SmoothGradCam.pdf')



# 📄 8. Converting the notebook into a Python script 

Since for runing the scripts in an interactive session has a time limit and the connection could be fragile (internet connection-tunnel). This is usually used for looking all is runing fine and to get a sense about the time and resources that were used.

In the following part we are going to convert the notebook file to a python, in the class folder convert the file to .py script

jupyter nbconvert --to script lab2.ipynb

This generates a lab2.py file

1. Remove
  ```text 
  get_ipython().run_line_magic('matplotlib', 'inline')
  from IPython.display import Image as DisplayImage
  ```
2, Replace
  ```text 
  DisplayImage(filename='usgs_lidar/10180_796_USGS_13_n33w101_20191031.jpg')
  DisplayImage(filename='usgs_lidar/nan_835_USGS_13_n47w115_20241127.jpg')
  to
  img = Image.open('usgs_lidar/10180_796_USGS_13_n33w101_20191031.jpg')
  plt.imshow(img, cmap='gray')
  plt.axis("off")
  plt.savefig("example_lidar_1.png", bbox_inches="tight")
  plt.close()
   ```
3. Replace plt.show() in the imshow function for
  ```text
    plt.savefig("training_examples.png", dpi=300, bbox_inches="tight")
    plt.close()
  ```
 4. Replace the rest of plt.show() to plt.close()
    
 6. For ResNet result
    ```text
    Change liveloss= PlotLosses() to
    liveloss = PlotLosses(outputs=[MatplotlibPlot(figpath="ConvergenceResNet50.pdf")])
    ```
5. Track the time

```text
    import time
    total_start = time.time()
    
    Add in important moments 
    print("\n===== Training Simple CNN =====")
    cnn_start = time.time()
    
    After the cnn has finished 
    cnn_end = time.time()
    cnn_time = cnn_end - cnn_start
    
    print("\n===== Simple CNN Summary =====")
    print(f"Simple CNN execution time: {cnn_time:.2f} seconds "
          f"({cnn_time/60:.2f} minutes)")
    
    print("\n===== Training ResNet50 =====")
    resnet_start = time.time()
    
    resnet_end = time.time()
    resnet_time = resnet_end - resnet_start
    
    print("\n===== ResNet50 Summary =====")
    print(f"ResNet50 execution time: {resnet_time:.2f} seconds "
          f"({resnet_time/60:.2f} minutes)")
    
    total_end = time.time()
    total_time = total_end - total_start
    
    print("\n" + "="*60)
    print("EXECUTION SUMMARY")
    print("="*60)
    print(f"Simple CNN Test RMSE: {mse ** 0.5:.4f}")
    print(f"Simple CNN time:   {cnn_time:.2f} seconds "
          f"({cnn_time/60:.2f} minutes)")
    print(f"ResNet50 Test RMSE: {rmse ** 0.5:.4f}")
    print(f"ResNet50 time:     {resnet_time:.2f} seconds "
          f"({resnet_time/60:.2f} minutes)")
    print(f"Total script time: {total_time:.2f} seconds "
          f"({total_time/60:.2f} minutes)")
    print("="*60)   
     ```
For making more orginize the results lets move the script in another subfolder called script in the folder class2
- mkdir script
- mv lab2.py script/lab2.py
  Make the previous changes and also the changes in the paths for the data sets needed.
```text
  df = pd.read_csv('../sample_data.csv', dtype=str)
  df['loan_delinquency'] = df['loan_delinquency'].astype(float)
  df['LiDAR_File'] = ('../' + df['LiDAR_File'].astype(str).str.replace("\\", "/")) 
 ```
and also 

```text
'resnet50_model_weights/resnet50_imagenet.pth' to '../resnet50_model_weights/resnet50_imagenet.pth'
 ```


# ⚙️ 9. Batch execution

In the folder create lab2job.sh
nano lab2job.sh
```text
#!/bin/bash
#
#SBATCH --job-name=image-deliquency
#SBATCH --gpus=a100_2g.10gb:1
#SBATCH --cpus-per-task=1
#SBATCH --mem-per-cpu=2G
#SBATCH --time=00:40:00
#SBATCH --account=def-cbravo
#SBATCH --output=lab2job.out

module load python scipy-stack
module load gcc
module load cuda
module load opencv
source ~/p3_env_nvl_test/bin/activate
python lab2.py
 ```

In the terminal in narval submit the job

(p3_env_nvl_test) [salfonso@narval1 script]$ sbatch lab2job.sh

