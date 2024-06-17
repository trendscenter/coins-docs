### Usage Instructions for Running the Singularity Container

These instructions will guide you through the process of setting up and running the `trendscenter/coins-dicom-receiver` Singularity container.

#### Step 1: Create the `$RECEIVER_HOME` Directory

First, create a directory named `$RECEIVER_HOME` which will be used to store configuration and data for the DICOM receiver.

```bash
mkdir -p $RECEIVER_HOME
```

#### Step 2: Decide on the Running User and Group

Determine which user and group will own and have read/write access to the `$RECEIVER_HOME` directory. For example, if the user is `dicomuser` and the group is `dicomgroup`, change the ownership of the directory:

```bash
sudo chown -R dicomuser:dicomgroup $RECEIVER_HOME
```

#### Step 3: Create and Edit `dicom_receiver.env`

Create a file named `dicom_receiver.env` in the `$RECEIVER_HOME` directory. This file will contain the necessary environment variables for the DICOM receiver. Open the file in a text editor to set the required parameters.

```bash
touch $RECEIVER_HOME/dicom_receiver.env
nano $RECEIVER_HOME/dicom_receiver.env
```

Add and set the necessary parameters within this file. Save and close the file once you have set the required environment variables.

#### Step 4: Pull and Convert the Docker Image to a Singularity Image

Use Singularity to pull the Docker image and convert it to a Singularity image file (`.sif`):

```bash
singularity pull docker://trendscenter/coins-dicom-receiver:0.0.1
```

This command will download the Docker image from Docker Hub and convert it to a Singularity image file named `trendscenter_coins-dicom-receiver_0.0.1.sif`.

#### Step 5: Verify the Singularity Image

Verify that the Singularity image has been created correctly:

```bash
singularity inspect trendscenter_coins-dicom-receiver_0.0.1.sif
```

#### Step 6: Run the Singularity Container as the Specified User

To run the Singularity container, you must execute the command as the user specified in Step 2. Use the `su` command or run it as a regular user if you're already logged in as the correct user.

```bash
sudo -u dicomuser singularity exec --writable-tmpfs \
  --bind $RECEIVER_HOME/dicom_receiver.env:/home/coins/dicom-receiver/utilities/dicom_receiver.env \
  --bind $RECEIVER_HOME/received_images:/home/coins/received_images \
  --bind $RECEIVER_HOME/validated_images:/home/coins/validated_images \
  --bind $RECEIVER_HOME/lost+found:/home/coins/lost+found \
  trendscenter_coins-dicom-receiver_0.0.1.sif \
  /bin/bash -c 'cd /home/coins && ./start_dicom_receiver.sh'
```

### Summary of Commands

1. **Create the `$RECEIVER_HOME` directory**:

    ```bash
    mkdir -p $RECEIVER_HOME
    ```

2. **Change the ownership of the `$RECEIVER_HOME` directory** (replace `dicomuser` and `dicomgroup` with your chosen user and group):

    ```bash
    sudo chown -R dicomuser:dicomgroup $RECEIVER_HOME
    ```

3. **Create and edit the `dicom_receiver.env` file**:

    ```bash
    touch $RECEIVER_HOME/dicom_receiver.env
    nano $RECEIVER_HOME/dicom_receiver.env
    ```

4. **Pull and convert the Docker image to a Singularity image**:

    ```bash
    singularity pull docker://trendscenter/coins-dicom-receiver:0.0.1
    ```

5. **Verify the Singularity image**:

    ```bash
    singularity inspect trendscenter_coins-dicom-receiver_0.0.1.sif
    ```

6. **Run the Singularity container as the specified user** (replace `dicomuser` with your chosen user):

    ```bash
    sudo -u dicomuser singularity exec --writable-tmpfs \
      --bind $RECEIVER_HOME/dicom_receiver.env:/home/coins/dicom-receiver/utilities/dicom_receiver.env \
      --bind $RECEIVER_HOME/received_images:/home/coins/received_images \
      --bind $RECEIVER_HOME/validated_images:/home/coins/validated_images \
      --bind $RECEIVER_HOME/lost+found:/home/coins/lost+found \
      trendscenter_coins-dicom-receiver_0.0.1.sif \
      /bin/bash -c 'cd /home/coins && ./start_dicom_receiver.sh'
    ```

By following these steps, you will set up and run the `trendscenter/coins-dicom-receiver` Singularity container with the necessary configurations. Adjust paths and commands as needed based on your specific environment and requirements.
