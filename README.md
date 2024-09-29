


# Crop Shield

## Description
Crop Shield is an advanced application designed to protect crops from wild animals using a combination of image detection with machine learning, drone surveillance, and piezo sensors. This system provides real-time monitoring and early warnings, helping farmers efficiently safeguard their crops and reduce potential damage.

## Features
- **Image Detection with Machine Learning**: Uses YOLOv8 for detecting wild animals that may harm the crops.
- **Drone Surveillance**: Real-time aerial monitoring to cover large areas and identify threats.
- **Piezo Sensors**: Detects physical disturbances around the crop fields.
- **Real-Time Monitoring**: Provides continuous updates and alerts to the farmers.

## Installation

### Step 1: Environment Setup
Create a new environment for the project. You can use Anaconda for managing environments.

```bash
conda create -n yolov8_custom python=3.9
conda activate yolov8_custom
```

### Step 2: Clone the Repository
Clone this repository to your local machine.

```bash
git clone https://github.com/BBALU1660/Crop_Sheild_AI.git
cd Crop_Sheild_AI
```

### Step 3: Install Dependencies
Install the necessary Python packages.

```bash
pip install ultralytics
```

## Dataset and Model Files

Due to storage limitations, the dataset and trained model files are hosted on Google Drive. You can download them from the following link:

[Google Drive - Crop Shield Files](https://drive.google.com/drive/folders/1hS-h5j_jTvd1ophcTSlPQqmoNZBrHXTb)

## Training the Model

### Step 1: Create or Download Dataset
You can use Roboflow or any other tool to create and label your dataset.

### Step 2: Train the Model
Navigate to the directory containing your dataset and train the model using YOLOv8.

```bash
cd /path/to/your/dataset
yolo task=detect mode=train epochs=100 data=data.yaml model=yolov8m.pt imgsz=640 batch=8
```

## Testing the Model

### Step 1: Test with Images or Videos
You can test the trained model using random images or videos.

```bash
yolo task=detect mode=predict model=/path/to/your/trained/model.pt source=/path/to/your/test/images show=True conf=0.5
```

## Using the Webcam for Real-Time Detection
Use the following Python code to capture images from the webcam and test the model.

```python
from IPython.display import display, Javascript
from google.colab.output import eval_js
from base64 import b64decode

def take_photo(filename='photo.jpg', quality=0.8):
    js = Javascript('''
        async function takePhoto(quality) {
            const div = document.createElement('div');
            const capture = document.createElement('button');
            capture.textContent = 'Capture';
            div.appendChild(capture);

            const video = document.createElement('video');
            video.style.display = 'block';
            const stream = await navigator.mediaDevices.getUserMedia({video: true});

            document.body.appendChild(div);
            div.appendChild(video);
            video.srcObject = stream;
            await video.play();

            google.colab.output.setIframeHeight(document.documentElement.scrollHeight, true);

            await new Promise((resolve) => capture.onclick = resolve);

            const canvas = document.createElement('canvas');
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            canvas.getContext('2d').drawImage(video, 0, 0);
            stream.getVideoTracks()[0].stop();
            div.remove();
            return canvas.toDataURL('image/jpeg', quality);
        }
    ''')
    display(js)
    data = eval_js('takePhoto({})'.format(quality))
    binary = b64decode(data.split(',')[1])
    with open(filename, 'wb') as f:
        f.write(binary)
    return filename

from IPython.display import Image

try:
    filename = take_photo()
    print('Saved to {}'.format(filename))
    display(Image(filename))
except Exception as err:
    print(str(err))
```

### References
- [YOLOv8 Documentation](https://docs.ultralytics.com)
- [Tutorial Video](https://www.youtube.com/watch?v=gRAyOPjQ9_s&t=582s)





