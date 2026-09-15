1. Build the based cuda image using tensorflow image from nvcr.
```
cd notebooks/components/example-notebook-servers/nvcr-cuda

2. Build the jupyter overlay using the nvcr-cuda image and publish it. 
```
cd notebooks/components/example-notebook-servers/nvcr-cuda-jupyter
docker build -t nvcr-cuda-jupyter-full:1.3 .
docker tag nvcr-cuda-jupyter-full:1.3 <dockerhub-repo>/custom-jupyter-tensorflow:1.3
docker images
docker push <dockerhub-repo>/custom-jupyter-tensorflow:1.3
```
