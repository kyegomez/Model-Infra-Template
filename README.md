[![Multi-Modality](agorabanner.png)](https://discord.gg/qUtxnK2NMf)

# Model-Infra-Template
A template for deploying ultra powerful LLMs effortlessly with the best optimizations


# Model config



# Dockerfile Deployment
To deploy the PyTorch Docker container, please follow the steps below:

Step 1: Install Prerequisites Before deploying the PyTorch Docker container, ensure that you have the following prerequisites installed on your host system:

Docker Engine
NVIDIA GPU Drivers
NVIDIA Container Toolkit
For specific versions and detailed installation instructions, refer to the Framework Containers Support Matrix and the NVIDIA Container Toolkit Documentation.

Step 2: Pull the Docker Image To pull the PyTorch Docker image, execute the following command:

docker pull nvcr.io/nvidia/pytorch:23.08-py3
Replace 23.08-py3 with the appropriate version you want to use.

Step 3: Run the Docker Container To run the PyTorch Docker container, use the following command:

If you have Docker 19.03 or later:

docker run --gpus all -it --rm nvcr.io/nvidia/pytorch:23.08-py3
If you have Docker 19.02 or earlier:

nvidia-docker run -it --rm nvcr.io/nvidia/pytorch:23.08-py3
Make sure to replace 23.08-py3 with the correct version.

Step 4: Verify PyTorch Installation After running the container, you can verify the PyTorch installation by importing it as a Python module and checking if CUDA is available. Execute the following commands inside the container:

```python
>>> import torch
>>> print(torch.cuda.is_available())
If CUDA is available, it will print "True".
```
Step 5: Mount External Data and Model Descriptions (Optional) If you need to use data and model descriptions from locations outside the container, you can mount host directories as Docker bind mounts. Use the following command as an example:

docker run --gpus all -it --rm -v local_dir:container_dir nvcr.io/nvidia/pytorch:23.08-py3
Replace local_dir with the path to the local directory and container_dir with the path inside the container.

Note: If you are using Torch multiprocessing for multi-threaded data loaders, you might need to increase the shared memory size of the container. You can do this by adding --ipc=host or --shm-size= options to the docker run command.

That's it! You have successfully deployed the PyTorch Docker container. You can now start using PyTorch for deep learning tasks.

For more information on customizing your PyTorch image and additional details, refer to the /workspace/README.md file inside the container.


To create a ready-to-use Dockerfile to deploy the production-grade model located in model/app.py, you can use the following Dockerfile:

# Use the PyTorch NGC Container as the base image
FROM nvcr.io/nvidia/pytorch:23.08-py3

# Set the working directory inside the container
WORKDIR /app

# Copy the model file into the container
COPY model/app.py .

# Expose the desired port (if necessary)
EXPOSE 8080

# Set the entrypoint command to run the model
CMD ["python", "app.py"]
This Dockerfile starts from the PyTorch NGC Container with the specified version (23.08-py3). It sets the working directory inside the container to /app and copies the model file app.py into the container. If your model requires any additional dependencies or files, you should include instructions to install or copy them as well.

The EXPOSE command is optional and should only be included if your model needs to listen on a specific port inside the container.

Finally, the CMD command specifies the command to execute when the container starts, which in this case is running the model using Python.

You can build the Docker image using the following command:

docker build -t my_model .
After the image is built, you can run the container using the following command:

docker run --gpus all -it --rm -p 8080:8080 my_model
Replace 8080 with the desired port if necessary.

This will start the container and deploy your production-grade model specified in model/app.py.

Note: Make sure you have Docker Engine, NVIDIA GPU Drivers, and NVIDIA Container Toolkit installed on your host system as mentioned in the prerequisites section.

# License
MIT



