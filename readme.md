1. Build the based cuda image using tensorflow image from nvcr.
```
cd notebooks/components/example-notebook-servers/nvcr-cuda
docker build -t nvcr-cuda-jupyter:1.0 .
```
2. Build the jupyter overlay using the nvcr-cuda image and publish it.
```
cd ../nvcr-cuda-jupyter/
docker build -t nvcr-cuda-jupyter-full:1.0 .
docker tag nvcr-cuda-jupyter-full:1.0 <dockerhub-repo>/custom-jupyter-tensorflow:1.0
docker images
docker push <dockerhub-repo>/custom-jupyter-tensorflow:1.0
```
