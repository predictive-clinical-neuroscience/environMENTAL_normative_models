# Run the scripts within a docker container

Before proceeding with the following steps, Docker must be installed on your system. Visit [Docker's official website](https://www.docker.com/) for more information.

## A. Build the PCNtoolkit Docker Container

You can build the container in two ways:

1) **Pull from Docker Hub**  
   You can pull the container image directly from Docker Hub. For example:

   ```bash
   docker pull amarquand/pcntoolkit:v0.27-arm64
   ```

   Optionally, rename the image for convenience:

   ```bash
   docker tag amarquand/pcntoolkit:v0.27-arm64 pcntoolkitv027
   ```

   Visit the [PCNtoolkit Docker Hub page](https://hub.docker.com/r/amarquand/pcntoolkit) for more details about available images.

2) **Build from Dockerfile**  
   Alternatively, you can build the container from the Dockerfile available at [PCNtoolkit GitHub](https://github.com/amarquand/PCNtoolkit/blob/master/docker/Dockerfile). Run this command in your current directory:

   ```bash
   curl -O https://raw.githubusercontent.com/amarquand/PCNtoolkit/master/docker/Dockerfile
   ```

   Then, build your Docker image:

   ```bash
   docker build -t pcntoolkitv030 .
   ```

   **Note:** As of now, Method 2 is preferred, as it builds the latest version of PCNtoolkit. Docker Hub will be updated soon. 

You can now run PCNtoolkit from the newly built container. See sections B and C below for further instructions.

---

## B. Download Normative Models (e.g., Structural Models: Cortical Thickness + Subcortical Volume)

1) Open a Linux terminal and navigate to your working directory (make sure the CSV files for model transfer are in this directory):

   ```bash
   cd ~/my_working/directory/
   ```

2) Launch the Docker container by typing:

   ```bash
   docker run -it -v ~/my_working/directory:/opt/shared --entrypoint /bin/bash --rm pcntoolkitv028
   ```

3) Navigate to the newly created shared volume (shared with your local working directory):

   ```bash
   cd /opt/shared
   ```

   **Note:** You should now see all the files from `~/my_working/directory` in the Docker container's `/opt/shared` directory.

4) Download the models:

   ```bash
   wget -O BLR_lifespan_57K_82sites.zip https://www.dropbox.com/sh/bd1j3rs6rg5dvuj/AACOWRU6MCfw6kWoYpjZFRoBa?dl=0
   ```

   **Note:** Other models (e.g., ThickAvg and SurfArea) are available in the [Links_Pretrained_Models.md](https://github.com/predictive-clinical-neuroscience/environMENTAL_normative_models/Links_Pretrained_Models.md) file.

5) Unzip the files and remove the zip archive:

   ```bash
   unzip -d BLR_lifespan_57K_82sites BLR_lifespan_57K_82sites.zip
   rm BLR_lifespan_57K_82sites.zip
   ```

---

## C. Adapt the normative models and estimate deviation scores (Z-scores)

1) After running the Docker container and downloading the appropriate models, navigate to the shared volume (e.g., `/opt/shared`). Make sure the downloaded model, Python script, adaptation file, and test file are all present in this directory:

   ```bash
   cd /opt/shared
   ```

2) Run the Bayesian Linear Regression model transfer script and estimate z-scores for your 'test' data:

   ```bash
   python transfer_BLR_model_test.py 'model_name' 'session_id' 'adapt_file' 'test_file'
   ```

   **Example:**

   ```bash
   python transfer_BLR_model_test.py BLR_lifespan_57K_82sites test_session OpenNeuroTransfer_ct_ad.csv OpenNeuroTransfer_ct_te.csv
   ```
