# MY ACCESSIBILITY SERVICE

Updated implementation of [Accessbilityservice] with extra functionalities to access video previous shortcuts, browse and download videos. 

## Introduction
This project is for users to be able to upload screenrecorded demonstrations of user actions and to convert them into a shortcut that can be replayed into their device. 

## Usage
The front-end application, which is run on the device, will allow the user to upload a video demonstration. This video will then be broken down through a modified version of [V2S] to extract the detected action components. These actions will be stored in a json file with the action type, location and action hint. 

Once complete, the user will be able to enable the shortcut and rerun the action from the video onto their own device. 

The device loops through the json file and executes the action type at the specific location. After each action execution, the device checks the similatrity of the device screen and the video frame and if the similarity percent is below 90, it will try and rerun the previous action execution. After three tries, an 'action hint' will appear on the screen to prompt the user to click the right button/location. 

## Pre-requisites

### Model Links
Unable to store models on github repo due to size limitations. Please download the below files and place them in the correct folders. 
Link to Models: https://drive.google.com/drive/folders/1Hvd3IYrseYh7yO4uVlOu2qgHnuw1NZGc?usp=sharing

Model Type | Model Name | Location | 
--- | --- | --- |
Opactiy Model | model-saved-alex8-tuned.h5 | ```pythonv2s/v2s/phase1/detection/opacity_model/``` | 
Touch Model | frozen_inference_graph_n5.pb | ```pythonv2s/v2s/phase1/detection/touch_model/saved_model_n5/``` | 
Touch Model | saved_model.pb | ```pythonv2s/v2s/phase1/detection/touch_model/saved_model_n5/``` | 
Touch Model | frozen_inference_graph_n6.pb | ```pythonv2s/v2s/phase1/detection/touch_model/saved_model_n6p/``` | 
Touch Model | saved_model.pb | ```pythonv2s/v2s/phase1/detection/touch_model/saved_model_n6/``` | 
Touch Model | saved_model.pb | ```pythonv2s/v2s/phase1/detection/touch_model/saved_model_n7/``` | 

### Environment
1. Ceate a conda environment with python
2. Run ```cd to \BackendWithV2S\python_v2s``
3. Run ```pip install .``` or manually install the following list of requirements 
    - 'ffmpeg',
    - 'ffmpeg-python',
    - 'Keras==2.2.4',
    - 'Keras-Applications==1.0.8',
    - 'Keras-Preprocessing==1.1.0',
    - 'matplotlib==3.1.1',
    - 'numpy>=1.20.2',
    - 'Pillow==6.2.0',
    - 'scipy==1.3.1',
    - 'tensorboard==2.0.0',
    - 'tensorflow==2.5.0',
    - 'tensorflow-macos==2.0.0' (only install if you run a MacOS device)
4. Run ```cd to \BackendWithV2S\flask_application```
5. Run ```pip install -r requirements.txt```
6. Create an uploads folder in main directory 

### Docker
Needed for redis, which is a database then can save our result. Create and start an image of the backend. 
1. Download docker, install needed dependency such as WSL
2. Run ```docker pull redis```
3. Run ```docker run -d -p 6379:6379 redis```

### Emulator
Only set up if emulator is needed. 
1. Select API33 System Image with the target that says Google API
2. Click Advanced Settings and change the Internal Storage and SD Card to 2000MB (or a value larger than 800) 
3. Open the Emulator and click Extended Controls (three dots on the side)
4. Settings -> Proxy -> Change Host to your IPV4 Address 

## Running the Application 
### Backend
1. Activate your environment
2. ```cd to \BackendWithV2S\flask_application```
3. Run ```python3 app.py```
4. In another cmd/terminal, run ```celery -A app.celery worker --loglevel=INFO --pool=solo``` to run celery, which is a task queue system
4. Open your docker application
5. Find the container that you have created last time for redis, start the image
6. To remove queue task run celery -A app.celery purge in cmd.

### Front-end
1. Build Gradle file
2. In HomeFragment change the ip address to be static to your ipv4 address
3. Run AccessibilityService App
4. Click on 'Select' Button to select video
5. Click on 'Upload' Button to upload video of selection
6. A new page should appear updating the progress of the upload
7. In the mean time, the user can view the video and its relavant info such as video address and name
7. Once complete, go to phone settings -> accessibility
8. Allow AccessibilityService access. 'Do Action' pop-up button should appear. 
9. Navigate to first screen of video demonstration. Click Do Action. 
10. Users are able to see a list of previously uploaded shortcuts in the shortcut history
11. Users can click on the blue action button to load up the specific action to avoid having to upload the video again
12. Inside the shortcut histories, the user can also view relavant info about the video
13. Users are able to browse videos in homepage and explore page, clicking on download in the explore page will allow them to download the video



