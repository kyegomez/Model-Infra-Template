# Use the PyTorch NGC Container as the base image
FROM nvcr.io/nvidia/pytorch:23.08-py3

# Setting up a working directory ensures any subsequent commands are run from this path inside the container
WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy your model's Python script into the container
# Remember, the Docker context should have the model directory and inside that, app.py should exist
COPY model/app.py .

# If your application exposes a web server or needs to listen on a port, this exposes it to the host
EXPOSE 8080

# This will ensure your Python script runs when the Docker container starts
CMD ["python", "app.py"]