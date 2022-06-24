# AI-Aquaculturing

This project is developed for smart aquaculture. We use a camera to capture pictures of fishes in the tank. A deep neural network (MobileNet) is applied to recognize the images to identify the situation of fishes in the tank. EdgeTPU is used for edge computing. We also monitor the water quality by some environmental sensors. All the information is integrated into a website constructed by Laravel, and a Linebot is used to deliver information on mobile phones. 

Four undergraduate students develop this project from the Department of Management Information Systems (MIS), National Pingtung University of Science and Technology (NPUST), Taiwan.

## 首先配置系統環境


* 安裝Anaconda
  https://www.anaconda.com/products/individual
  
  配置Anacond主要是為了簡單化安裝必要用到之套件，減少新手配置環境變數遇到之困難，另外Anaconda有著Managing environments，確保當環境配置出錯時不影響主系統

  另外如果您是對環境配置熟練不過的高手，可以跳過安裝Anaconda，照著接下來的教學中出現的套件自行安裝
  
  ![Anaconda](https://upload.wikimedia.org/wikipedia/en/c/cd/Anaconda_Logo.png)

---
1. 當您安裝完成Anaconda後，我們開始創建一個新的環境

   * 打開您的Command OR Terminal並輸入以下指令，創建名為train的新環境python版本為3.5
   
     `conda create -n train pip python=3.5`<br><br>
  
2. 當您創建完成後透過以下指令來確認您是否有成功創建，下圖為成功畫面

   * 打開您的Command OR Terminal並輸入以下指令

      `conda env list`

       ![conda env list](https://github.com/cairongfu/AI-Aquaculturing/blob/main/ReadmePicture/Anaconda_Create_Env.JPG)<br><br>

3. 切換到新創的train環境底下

   * 打開您的Command OR Terminal並輸入以下指令，環境就會從原本的base變成train

     `conda activate train`
    
     ![conda env list](https://github.com/cairongfu/AI-Aquaculturing/blob/main/ReadmePicture/Anaconda_Change_Env.JPG)<br><br>
  
    ### **更多關於Anaconda管理環境指令可以參考以下網址**

    ### [Anaconda Managing Environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)<br><br>

4. 此步驟是為了配置必要套件，部分套件Anaconda安裝時已自動配置，保險起見還是在全部輸入一次。

    ```diff
    - 未安裝Anaconda的人，以下套件請全部手動安裝
    ```
   * 打開您的Command OR Terminal並輸入以下指令

      - `python -m pip install --upgrade pip `
      - `pip install tensorflow-gpu==1.15`
      - `conda install -c anaconda protobuf`
      - `pip install Cython`
      - `pip install contextlib2`
      - `pip install pillow`
      - `pip install lxml`
      - `pip install jupyter`
      - `pip install matplotlib`
      - `pip install opencv-python`
      - `pip install pandas`
<br><br>

5. 接著找一個你喜歡的目錄把tensorflow-models git clone下來
  
   ### [Tensorflow Models](https://github.com/tensorflow/models)

   ```diff
   - 注意(git clone 此專案時就有tensorflow-models檔案)
   ```

   * 打開您的Command OR Terminal並輸入以下指令

     `git clone https://github.com/tensorflow/models.git`
---

## 開始配置環境變數

  - 打開您的Command OR Terminal並輸入以下指令：
    - 要設定的環境變數有 models、research、slim
    - Key入以下指令
    - 每次重開command都要重設一次環境變數
    - 直接寫到環境變數裡面去就不用每次都重設
    - **接下來所有路徑位置都要替換成自己得路徑位置**

### Windows環境 

<br>

```
set PYTHONPATH=C:\Users\RONGF\Desktop\modle\models;C:\Users\RONGF\Desktop\modle\models\research;C:\Users\RONGF\Desktop\modle\models\research\slim
```
<br>

### Linux環境

<br>

```
export PYTHONPATH=/home/itriedgetpunpust/ipynb/TsaiJungFu/cai-train-path/models:/home/itriedgetpunpust/ipynb/TsaiJungFu/cai-train-path/models/research:/home/itriedgetpunpust/ipynb/TsaiJungFu/cai-train-path/models/research/slim
```

<br>

* 使用 protoc Bullding 檔案

  接著到下列網址下載protobuf工具(依照自身作業系統下載)

  ### [Protobuf Tool](https://github.com/protocolbuffers/protobuf/releases)

  ~~接著下載use_protobuf.py檔，以下網址若失效至專案檔裡Bullding資料夾內找~~

  ~~[use_protobuf.py](https://trello.com/1/cards/5e95106ac42ee761ec1a911d/attachments/5e957d679b6f7a4c7d04c68a/download?backingUrl=https%3A%2F%2Ftrello-attachments.s3.amazonaws.com%2F5e6856f39a79a12665199a15%2F5e95106ac42ee761ec1a911d%2Fa6624a2e61cbf0896f81f551275073fb%2Fuse_protobuf.py)~~

- ~~打開您的Command OR Terminal並輸入以下指令：~~
  - ~~cd 至 use_protobuf.py檔案資料夾底下~~
  - ~~Key入以下指令 **注意空白符號**~~

    ~~`
    ~~python use_protobuf.py 指定到剛剛下載的models/research/object_detection底下的protos資料夾內 在指定存放protobuf工具的路徑~~
    `~~

    <br>


    **~~範例~~**

    <br>


    ~~`
    python use_protobuf.py models/research/object_detection/protos C:/Users/Eggs/Desktop/tensorflow/bin/protoc
    `~~
<br>

- Windows成功下載protobuf檔案後解壓縮會出現bin資料夾，在環境變數中設定bin路徑
  - 打開您的Command OR Terminal並輸入以下指令：
  - cd models\research
  - Key入以下指令

    ```
    protoc --python_out=. .\object_detection\protos\anchor_generator.proto .\object_detection\protos\argmax_matcher.proto .\object_detection\protos\bipartite_matcher.proto .\object_detection\protos\box_coder.proto .\object_detection\protos\box_predictor.proto .\object_detection\protos\eval.proto .\object_detection\protos\faster_rcnn.proto .\object_detection\protos\faster_rcnn_box_coder.proto .\object_detection\protos\grid_anchor_generator.proto .\object_detection\protos\hyperparams.proto .\object_detection\protos\image_resizer.proto .\object_detection\protos\input_reader.proto .\object_detection\protos\losses.proto .\object_detection\protos\matcher.proto .\object_detection\protos\mean_stddev_box_coder.proto .\object_detection\protos\model.proto .\object_detection\protos\optimizer.proto .\object_detection\protos\pipeline.proto .\object_detection\protos\post_processing.proto .\object_detection\protos\preprocessor.proto .\object_detection\protos\region_similarity_calculator.proto .\object_detection\protos\square_box_coder.proto .\object_detection\protos\ssd.proto .\object_detection\protos\ssd_anchor_generator.proto .\object_detection\protos\string_int_label_map.proto .\object_detection\protos\train.proto .\object_detection\protos\keypoint_box_coder.proto .\object_detection\protos\multiscale_anchor_generator.proto .\object_detection\protos\graph_rewriter.proto .\object_detection\protos\calibration.proto .\object_detection\protos\flexible_grid_anchor_generator.proto
    ```
    
<br>

**完成後你會在models\research\object_detection\protos資料夾底下看到相同檔名副檔名為.py之檔案**

<br>

- 打開您的Command OR Terminal並輸入以下指令：
  - cd 到您剛剛git clone 下來的models/research
  - Key入以下指令
  - python setup.py build
  - python setup.py install

**Windows、Linux在install時錯誤把請用python3.6環境進行build、install、install**

---

## 測試

下載此ipynb檔並取代原來models\research\object_detection底下的object_detection_tutorial ipynb檔

以下網址若失效至專案檔裡Bullding資料夾內找

[object_detection_tutorial.ipynb](https://trello-attachments.s3.amazonaws.com/5e6856f39a79a12665199a15/5e95106ac42ee761ec1a911d/47e97716398632b02c1493c826cd87f7/object_detection_tutorial.ipynb) 

- 打開您的Command OR Terminal並輸入以下指令：
  - cd 到您剛剛git clone 下來的models\research\object_detection資料夾內
  - Key入以下指令
  - jupyter notebook object_detection_tutorial.ipynb


### 執行此object_detection_tutorial.ipynb如果有出現圖片如下圖就表示您環境配置成功囉！！

![object_detection_tutorial](https://github.com/cairongfu/AI-Aquaculturing/blob/main/ReadmePicture/object_detection_tutorial.png)

---

# 圖像標記

**注意接下來這個步驟可以選擇不做，此步驟是為了簡化電腦運算的負擔(建議是做拉)**<br>

準備好您的圖片放在同一個資料夾

接著下載以下py檔，以下網址若失效至專案檔裡Bullding資料夾內找<br>
[transform_image_resolution.py](https://trello.com/1/cards/5e95106ac42ee761ec1a911d/attachments/5e9511d9d333a51c33985b77/download?backingUrl=https%3A%2F%2Ftrello-attachments.s3.amazonaws.com%2F5e6856f39a79a12665199a15%2F5e95106ac42ee761ec1a911d%2Fb8d987f3d54237772f0829de8ae98a26%2Ftransform_image_resolution.py)


- 打開您的Command OR Terminal並輸入以下指令：
  - cd 到您存放transform_image_resolution.py目錄底下
  - Key入以下指令
    ```
        python transform_image_resolution.py -d 圖片檔案路徑 -s 圖片縮放比例
    ```

    <br>

    範例

    <br>

    ```
        python transform_image_resolution.py -d images/ -s 800 600
    ```

<br>

### 接著標記所有圖片使用LabelImg

[LabelImg Github](https://github.com/tzutalin/labelImg)

* git clone https://github.com/tzutalin/labelImg.git
* pip install lxml
* pip install pyqt5
* 如遇到`No module named 'libs.resources'`錯誤輸入下列指令

  ```
  pyrcc5 -o ./libs/resources.py resources.qrc
  ```

<br>

### 接著創建一個名為images的資料夾並在此資料夾並在此資料夾底下創建兩個新資料夾分別為

<br>

**test**     將20％ 的圖片數量放到test資料夾內(包含標記過後對應的XML檔)

**train**    將80％ 的圖片數量放到train資料夾內(包含標記過後對應的XML檔)

<br>

### 接著下載以下兩個py檔，如字面上意思將您的XML轉成CSV檔

* **注意以下code其中的 image_path路徑、xml_df.to_csv路徑，改為自己XML位置**
* 以下網址若失效至專案檔裡Bullding資料夾內找


### [xml_to_csv.py](https://trello-attachments.s3.amazonaws.com/5e6856f39a79a12665199a15/5e95106ac42ee761ec1a911d/3f1c24f7a144f306e502a2fb2d01b95f/xml_to_csv.py) 

下列★部分，為要替換成自己路徑代碼

```
def main():
    for folder in ['train', 'test']:
  ★  image_path = os.path.join(os.getcwd(), ('images/' + folder))
        xml_df = xml_to_csv(image_path)
  ★  xml_df.to_csv(('images/'+folder+'_labels.csv'), index=None)
    print('Successfully converted xml to csv.')
```

- 打開您的Command OR Terminal並輸入以下指令：
  - cd 到您存放xml_to_csv.py目錄底下
  - Key入以下指令
    ```
    python xml_to_csv.py
    ```

    ![generate_tfrecord_command.py](https://github.com/cairongfu/AI-Aquaculturing/blob/main/ReadmePicture/xml_to%20_csv_command.JPG)

**Successfully converted xml to csv後會在您的images資料夾內出現兩個檔分別如下：**

`test_labels.csv`

`train_labels.csv`

![generate_tfrecord_file.py](https://github.com/cairongfu/AI-Aquaculturing/blob/main/ReadmePicture/xml_to_csv_file.jpg)

<br>

### 接著執行這個generate_tfrecord.py檔

### [generate_tfrecord.py](https://trello-attachments.s3.amazonaws.com/5e6856f39a79a12665199a15/5e95106ac42ee761ec1a911d/268cdee44b44ff9624a0e72db60a72c1/generate_tfrecord.py)

<br>

**注意以下code其中的 def class_text_to_int部分，這部分需要依照您要訓練的label新增，這邊我只訓練一類故此只有 Clownfish如要新增多label就自行增加**
 
```
if  row_label == 'label name':
       return lable number(直接填寫數字)如下code
    elseif:
       ......
    else:
       None
```

下列★部分要特別注意填寫的內容

**label name**

**return number**

```
def class_text_to_int(row_label):
★  if row_label == 'Clownfish':
★      return 1
    else:
        None
```
- 打開您的Command OR Terminal並輸入以下指令：
  - cd 到您存放generate_tfrecord.py目錄底下
  - Key入以下指令
  - **特別注意您存放test_labels.csv、train_labels.csv ，--csv_input路徑位置，如不一樣請自行修改**

    ```
    python generate_tfrecord.py --csv_input=images/train_labels.csv --image_dir=images/train --output_path=train.record
    ```

    ```
    python generate_tfrecord.py --csv_input=images/test_labels.csv --image_dir=images/test --output_path=test.record
    ```
  - **成功Bullding tfrecode檔案如下圖**

    ![generate_tfrecord_command_test.py](https://github.com/cairongfu/AI-Aquaculturing/blob/main/ReadmePicture/generate_tfrecord_test.JPG)

    ![generate_tfrecord_command_train.py](https://github.com/cairongfu/AI-Aquaculturing/blob/main/ReadmePicture/generate_tfrecord_train.JPG)

  - **會在您下存放generate_tfrecord.py檔路徑底下出現下圖兩隻檔案**

    ![generate_tfrecord_file.py](https://github.com/cairongfu/AI-Aquaculturing/blob/main/ReadmePicture/generate_tfrecord_file.JPG)

### **把剛剛轉出來的test.record、train.record檔移至models\research\object_detection底下**

---

## 配置訓練檔案

* 首先到以下找您要訓練的模型預訓練檔並且下載下來解壓縮後放置在你喜愛的路徑下

  ### [Tensorflow Zoo](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf1_detection_zoo.md)

  <br>

* 接著創建一個labelmap.pdtxt檔案如下範例(可拿下面labelmap.pbtxt改)

  ```
  此處id數量和ID請與generate_tfrecord.py一致
  item {
      id: 1
      name: 'ID'
  }
  item {
      id: 2
      name: 'ID'
  }
  item {
      id: 3
      name: 'ID'
  }
  item {
      id: 4
      name: 'ID'
  }
  ```

* 以下網址若失效至專案檔裡Bullding資料夾內找

  ### [labelmap.pbtxt](https://trello.com/1/cards/5e95106ac42ee761ec1a911d/attachments/5e95630c6c00820229eb32e8/download?backingUrl=https%3A%2F%2Ftrello-attachments.s3.amazonaws.com%2F5e6856f39a79a12665199a15%2F5e95106ac42ee761ec1a911d%2F8c1f4d05173cd40e69ec33941f98da12%2Flabelmap.pbtxt)

- 接著到models\research\object_detection底下

- 打開您的Command OR Terminal並輸入以下指令：
  - cd models\research\object_detection
  - Key入以下指令
  - `mkdir training`
- **接著到models\research\object_detection\samples\configs底下找到您要的訓練模型config複製到剛剛新增的training資料夾內**

- **剛剛上一步驟中的labelmap.pdtxt也放入新增的training資料夾內**

- 修改config檔設定

    - model 內的 num_classes數量由您要訓練的label決定

      ```
      model {
        ssd {
          num_classes: 1
          box_coder {
            faster_rcnn_box_coder {
              y_scale: 10.0
              x_scale: 10.0
              height_scale: 5.0
              width_scale: 5.0
            }
          }
      ```

  - fine_tune_checkpoint: 路徑就設定在剛剛下載的tensorflow zoo下載的模型位置
在最後加上model.ckpt

    ```
    fine_tune_checkpoint: "C:/Users/RONGF/Desktop/modle/models/research/object_detection/ssdlite_mobilenet_v2_coco_2018_05_09/model.ckpt"
    ```

  - input_path:放入您的train.record路徑

  - label_map_path:放入您的labelmap.pbtxt路徑

    ```
    train_input_reader: {
      tf_record_input_reader {
        input_path: "C:/Users/RONGF/Desktop/modle/train.record"
      }
      label_map_path: "C:/Users/RONGF/Desktop/modle/models/research/object_detection/training/labelmap.pbtxt"
    }
    ```
  - 此處的input_path放入test.record路徑

  - label_map_path:放入您的labelmap.pbtxt路徑

    ```
    eval_input_reader: {
      tf_record_input_reader {
        input_path: "C:/Users/RONGF/Desktop/modle/test.record"
      }
      label_map_path: "C:/Users/RONGF/Desktop/modle/models/research/object_detection/training/labelmap.pbtxt"
      shuffle: false
      num_readers: 1
    }
    ```

### 目前EdgeTpu僅接受sd_mobilenet_v2_quantized_300x300_coco否則訓練出來模型會有問題！

### 請依照手邊系統適配之模型下去訓練，此處設定檔很重要要配置正確，請詳細檢查是否有打錯

### 更多Config配置參數細節可自行Google查詢

---

## 開始進行模型訓練

到models\research\object_detection\legacy\train複製train.py檔到models\research\object_detection目錄下

- 打開您的Command OR Terminal並輸入以下指令：
  - cd models\research\object_detection
  - Key入以下指令
  - 如下路徑請自行轉換成您自己的路徑-和自己的config檔名稱

    ```
    python train.py --logtostderr --train_dir=training/ --pipeline_config_path=training/config模型名稱.config
    ```

### 沒有報任何錯誤代表您成功開始訓練囉！！

---

 ## 以下是訓練時發生時的幾種樣態及Debug方法
(目前本人我遇到的情況，每個人環境都可能不一樣所以並非全部錯誤都會在下面)

-  No module named 'absl'
    - 打開您的Command OR Terminal並輸入以下指令：
    - **`pip install absl-py`**

- No module named 'tensorflow'
    - 打開您的Command OR Terminal並輸入以下指令：
    - **`pip install tensorflow==1.15`**
 
-  No module named 'pycocotools'
    - cmd key
    - **`pip install "git+https://github.com/philferriere/cocoapi.git#egg=pycocotools&subdirectory=PythonAPI"`**

-  cannot import name 'anchor_generator_pb2' from 'object_detection.protos' (C:\Users\Eggs\anaconda3\lib\site-packages\object_detection-0.1-py3.7.egg\object_detection\protos\__init__.py)
    - 打開您的Command OR Terminal並輸入以下指令：
    - cd C:/Users/Eggs/Desktop/tensorflow/bin/proto (這裡指的是剛剛protoc.exe路徑)
    - --python_out=./ (原地匯出py檔)
    - research\object_detection\protos\*.proto轉換protos資料夾底下所有路徑
    - 接著進到 models\research\object_detection\protos資料夾內複製所有檔案至anaconda內的protos資料夾裡並取代所有

-  No module named 'nets'
    - 打開您的Command OR Terminal並輸入以下指令：
    - cd models\research\slim 底下輸入
    - **`python setup.py build`**
    - **`python setup.py install`**
    - 如果build、install失敗請刪除資料夾內的BUILD檔再進行一次上述兩個指令
  
---

## 凍結出模型

當模型訓練到您要的loss值之後Ctrl+c關閉後在training資料夾內會出現model.ckpt-xxxxx檔

打開training資料夾找到model.ckpt-xxxxx檔案，選擇數字最高的檔案

- 打開您的Command OR Terminal並輸入以下指令：
    - cd models\research\object_detection
    - Key入以下指令
    - `python export_tflite_ssd_graph.py --input_type image_tensor --pipeline_config_path training/依照自己訓練的config檔名稱.config --trained_checkpoint_prefix training/model.ckpt-XXXX(最大數值) --output_directory ssd_graph(可行命名想要的資料夾名稱)`

      ```
      python export_tflite_ssd_graph.py --input_type image_tensor --pipeline_config_path training/ssdlite_mobilenet_v2_coco.config --trained_checkpoint_prefix training/model.ckpt-XXXX --output_directory ssd_graph
      ```

### **成功之後會在object_detection 資料夾內出現ssd_graph資料夾裡面會有tflite_graph.pb檔**

---

## 開始pd檔轉型成tflite檔，建置新的Anaconda Managing environments

如同一開始所教的配置新的環境，建議您在Anaconda建置新的環境。

- 打開您的Command OR Terminal並輸入以下指令：
    - conda create -n tensorflow pip python=3.6
    - conda activate tensorflow

  #### **更多關於Anaconda管理環境**

  #### [Anaconda Managing Environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)

<br>

參照下列網址建置Tensorflow

https://www.tensorflow.org/install/source_windows

首先配置環境安裝以下檔案
* 以下網址若失效至專案檔裡Bullding資料夾內找

  [vs_buildtools.exe](https://trello-attachments.s3.amazonaws.com/5e6856f39a79a12665199a15/5e95106ac42ee761ec1a911d/51081d5a8b8ec951efe2f2afadf8aa7c/vs_buildtools.exe) 

  [vc_redist.x86](https://drive.google.com/file/d/1ZC-5g6hPiw4h0-MkJln75iNZbk03Dy0m/view?usp=sharing)

  [MSYS2](http://repo.msys2.org/distrib/x86_64/msys2-x86_64-20190524.exe)

  [bazel](https://github.com/bazelbuild/bazel/releases/download/0.26.1/bazel-0.26.1-windows-x86_64.zip)

  [Tensorflow](https://github.com/tensorflow/tensorflow/archive/v1.15.0.zip)

### **當您執行到**
```
bazel build //tensorflow/tools/pip_package:build_pip_package
```
### **最後報錯這是沒有關西的**

### **接著不要關掉Command OR Terminal 繼續執行**

```
bazel run --config=opt tensorflow/lite/toco:toco
```
### **這部分會Build成功！！**

### **接著把剛剛訓練出來的.pd檔放到MSYS2安裝路徑下msys64資料夾底下**

### **一樣Command OR Terminal執行以下指令**

**如果之前已build過的人請到當時 git clone 的位置底下，並切換Anaconda環境至tensorflow下**

#### **注意下面路徑和檔案名稱請對應自身的路徑和名稱如還不懂請參照下列網址2.3點使用TOCO創建優化的TensorFlow Lite模型步驟[TensorFlow Lite](https://gilberttanner.com/blog/convert-your-tensorflow-object-detection-model-to-tensorflow-lite)**

``` 
bazel run --config=opt tensorflow/lite/toco:toco -- --input_file=/tflite_graph.pb --output_file=/detect.tflite --input_shapes=1,300,300,3 --input_arrays=normalized_input_image_tensor --output_arrays=TFLite_Detection_PostProcess,TFLite_Detection_PostProcess:1,TFLite_Detection_PostProcess:2,TFLite_Detection_PostProcess:3 --inference_type=QUANTIZED_UINT8 --mean_values=128 --std_values=128 --change_concat_input_ranges=false --allow_custom_ops --default_ranges_min=0 --default_ranges_max=255
```

### **成功之後會在MSYS2安裝路徑下msys64資料夾底下出現detect.tflite檔檔案大小會縮小表成功**

<br>

### 此處重要的點在於您的版本要對應正確！多加留意，很重要所以說三次

### 此處重要的點在於您的版本要對應正確！多加留意，很重要所以說三次

### 此處重要的點在於您的版本要對應正確！多加留意，很重要所以說三次

### **有任何問題請參照Tensorflow官方教學[Tensorflow for Windows](https://www.tensorflow.org/install/source_windows)**

---
## Edge Tpu Compile

為了讓tflite在edage TPU上運行速度正常請參照下列文章在編譯一次模型

**此步驟在Ubuntu、Linux環境下運作會方便很多，請不要自虐在windows做**

[Coral Edge Tpu compiler](https://coral.ai/docs/edgetpu/compiler/)

因此步驟極度簡單就不加以說明參照官方文件做就行了

此步驟完成後會在同一個路徑下出現detect_degetpu.tflite  這就是最後模型了

## **恭喜您成功完成了！**

## **若還有問題麻煩開啟Issues我們盡可能的幫助您**

---

# Edge Tpu 執行及環境介紹
此專案位於AI-Aquaculturing/edgeTPU資料夾中，主要是Edge Tpu執行影像偵測的設定
這些是本專案編輯過的edgetpu檔案，目錄是在 /usr/lib/python3/dist-packages/edgetpuvision/ 底下，主要有異動的檔案有detect.py及server.py

## 須先在Edge Tpu中安裝的套件
```
pip3 install pytz
pip3 install mysql-connector-python
```

## detect檔案位置及介紹，需要先更改裡面資料庫連接設定，也可以依個人需求編輯其他項目
先移動到/usr/lib/python3/dist-packages/edgetpuvision/，並用nano編輯器開啟detect.py
```
cd /usr/lib/python3/dist-packages/edgetpuvision/
sudo nano detect.py
```

第60行到64行請先換成自己的資料庫
```
###connect mysql
hostname = 'hostname'
username = 'username'
password = 'password'
database = 'database'
```

第200行為辨識正確率大於多少要執行上傳資料庫的判斷，可以從0到1自行設定，0=0%、1=100%
```
if socre > 0.7:
```

第227行的'1'為上傳到資料庫中的label，可以依據這台edgetpu辨識的種類的label自行更換
```
data = ('1',"'"+positsion+"'","'"+str(thistime)+"'")
```

## 執行本地物件偵測
影像模型先命名為detect_edgetpu.tflite，放在EdgeTpu根目錄下，其中--displaymode window是視窗化執行的意思，不輸入會以全螢幕執行
```
edgetpu_detect --source /dev/video1:YUY2:1280x720:10/1 --model detect_edgetpu.tflite --displaymode window
```

## 更改server物件偵測port
```
cd /usr/lib/python3/dist-packages/edgetpuvision/streaming
sudo nano server.py
```
在第252行中，可以把4664換成自己需要的port
```
web_port=4664
```
### 最後執行起來並在瀏覽器中輸入edgetpu的實體ip及設定好的port就能及時觀看物件偵測的即時串流了

## 執行server物件偵測(外部可連)
影像模型先命名為detect_edgetpu.tflite，放在EdgeTpu根目錄下
```
edgetpu_detect_server \
--source /dev/video1:YUY2:1280x720:24/1  \
--model detect_edgetpu.tflite
```

---
# AI_Aquaculturing laravel智慧觀賞魚輔助系統網站 網頁教學
此網頁專案位於 AI-Aquaculturing/AI_Aquaculturing_laravel/
此專案在controller連結了mysql資料庫,接著利用[Server-Sent Events](https://en.wikipedia.org/wiki/Server-sent_events)將資料推送到前端,並使用[Chart.js](https://www.chartjs.org/)此JavaScript圖表即時視覺化顯示最新觀賞魚資訊給使用者觀看

## 資料庫數據
資料庫數據位於mysql資料夾中的fishai.sql,可以直接將其匯入資料庫中

## 資料庫連結方法
修改`.env`檔案中這幾行
```
DB_HOST=YOUR_HOST
DB_DATABASE=YOUR_DATABASE
DB_USERNAME=YOUR_USERNAME
DB_PASSWORD=YOUR_PASSWORD
```
```
如在本地執行,則DB_HOST可以設為127.0.0.1
```
## 如何執行?
使用cmd執行指令
```sh
cd 專案根目錄
php artisan serve
```

## 執行時要指定host以及port時
```sh
php artisan serve --host=YOUR_HOST --port=YOUR_PORT
```

## 網站登入帳號及密碼
```sh
帳號:test1234@gmail.com
密碼:123456789
```
<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/d/total.svg" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/v/stable.svg" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://poser.pugx.org/laravel/framework/license.svg" alt="License"></a>
</p>

## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.

## Learning Laravel

Laravel has the most extensive and thorough [documentation](https://laravel.com/docs) and video tutorial library of all modern web application frameworks, making it a breeze to get started with the framework.

If you don't feel like reading, [Laracasts](https://laracasts.com) can help. Laracasts contains over 1500 video tutorials on a range of topics including Laravel, modern PHP, unit testing, and JavaScript. Boost your skills by digging into our comprehensive video library.

## Laravel Sponsors

We would like to extend our thanks to the following sponsors for funding Laravel development. If you are interested in becoming a sponsor, please visit the Laravel [Patreon page](https://patreon.com/taylorotwell).

### Premium Partners

- **[Vehikl](https://vehikl.com/)**
- **[Tighten Co.](https://tighten.co)**
- **[Kirschbaum Development Group](https://kirschbaumdevelopment.com)**
- **[64 Robots](https://64robots.com)**
- **[Cubet Techno Labs](https://cubettech.com)**
- **[Cyber-Duck](https://cyber-duck.co.uk)**
- **[Many](https://www.many.co.uk)**
- **[Webdock, Fast VPS Hosting](https://www.webdock.io/en)**
- **[DevSquad](https://devsquad.com)**
- **[OP.GG](https://op.gg)**

## Contributing

Thank you for considering contributing to the Laravel framework! The contribution guide can be found in the [Laravel documentation](https://laravel.com/docs/contributions).

## Code of Conduct

In order to ensure that the Laravel community is welcoming to all, please review and abide by the [Code of Conduct](https://laravel.com/docs/contributions#code-of-conduct).

## Security Vulnerabilities

If you discover a security vulnerability within Laravel, please send an e-mail to Taylor Otwell via [taylor@laravel.com](mailto:taylor@laravel.com). All security vulnerabilities will be promptly addressed.

## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

---
# 物聯網_EdgeTPU CoralBroad
本專案之影像辨識硬體使用Google開發的EdgeTPU CoralBroad，在開始前須先安裝刷入系統
## 環境配置
首次刷入系統，請注意開關方向，順序為on off off off

![image](https://trello-attachments.s3.amazonaws.com/5e6b1f00699f7c5e70677191/5e6dd610d3d4212bf29753fb/72d420ecde8b4c8969025aba79902f5b/%E9%96%8B%E9%97%9C.png)

並請先準備一台具備Linux系統的主機，並且安裝下列工具
安裝fastboot
```
sudo apt-get install fastboot
```
安裝mdt工具
```
pip3 install --user mendel-development-tool
```
安裝Screen工具
```
sudo apt-get install screen
```

### Step1：開啟終端機介面，添加udev規則
```
sudo sh -c "echo 'SUBSYSTEM==\"usb\", ATTR{idVendor}==\"0525\", MODE=\"0664\", \
GROUP=\"plugdev\", TAG+=\"uaccess\"' >> /etc/udev/rules.d/65-edgetpu-board.rules"
```
### Step2:連接micro usb
此時會看到綠色、紅色燈亮起
之後輸入
```
dmesg | grep ttyUSB
```
你應該會看到類似
```
[ 6437.706335] usb 2-13.1: cp210x converter now attached to ttyUSB0
[ 6437.708049] usb 2-13.1: cp210x converter now attached to ttyUSB1
```
之後輸入指令
```
sudo screen /dev/ttyUSB0 115200
```
會進入一個完全空白的畫面屬於正常，不用擔心，請繼續下一步
### Step3：上電（將Type-C插入pwr插口 此時應看到紅色燈亮起
輸入
```
fastboot 0
```
讓設備進入刷機模式 即可縮小這個終端機界面
將終端機將顯示
```
< waiting for any device >
```
## 首次刷入系統
### Step 1：連接Type C （OTG插口
開啟新的終端機界面 輸入以下
```
fastboot devices
```
出現類似 1b0741d6f0609912 fastboot 訊息代表成功
### Step 2：下載系統檔案並刷入TPU

開啟一個新的終端機界面
```
cd ~/Downloads
```
```
curl -O https://dl.google.com/coral/mendel/enterprise/mendel-enterprise-day-13.zip
```
```
unzip mendel-enterprise-day-13.zip \
&& cd mendel-enterprise-day-13
```
```
bash flash.sh
```
此時會看到TPU的終端機一直閃爍跑資料 代表開始刷入 等待即可

## 首次登入
```
帳號密碼都是mendel
```

## 重新刷入系統
當您系統出問題導致無法順利執行時，可以嘗試此步驟
首先將tpu開關用成上下上上

### Step1：連接micro usb 進入串口界面 請勿上電
```
sudo screen /dev/ttyUSB0 115200
```
此時顯示空白正常
### Step2：將sd卡刷入重刷程序
請先安裝balena [balena載點](https://www.balena.io/etcher/)
將資料夾mendel-enterprise-day-13 裡面的recovery.img刷入
### Step3：插入sd卡 以及插入OTC並上電（插入pwr）開新終端機界面，並輸入
```
fastboot devices
```
顯示類似
1b0741d6f0609912        fastboot  代表成功

### Step3：打開新的終端機介面，並輸入
```
cd ~/Downloads
```
```
curl -O https://dl.google.com/coral/mendel/enterprise/mendel-enterprise-day-13.zip
```
```
unzip mendel-enterprise-day-13.zip \
&& cd mendel-enterprise-day-13
```
```
bash flash.sh
```
顯示Finished代表成功
### Step4：將tpu用回上下下下並輸入帳號密碼 成功重刷
帳號密碼預設都是mendel

---
# 更新TPU software以及運行範例模型
```
mdt shell #進入TPU終端
```
## 確認板子有沒有更新到最新
```
sudo apt-get update
sudo apt-get dist-upgrade
```
## 之後安裝TensorFlow Lite
```
pip3 install https://dl.google.com/coral/python/tflite_runtime-2.1.0.post1-cp37-cp37m-linux_aarch64.whl
```
## 運行demo
```
edgetpu_demo --stream
若是沒有螢幕連接（使用電腦mdt操作tpu） 瀏覽器輸入 http://192.168.100.2:4664/
```
## 跑TensorFlow Lite模型
### Step1 下載範例
```
sudo apt-get install git

mkdir coral && cd coral

git clone https://github.com/google-coral/tflite.git
```
### Step 2 下載分類器依賴項
```
cd tflite/python/examples/classification

bash install_requirements.sh
```
### Step3 運行
```
python3 classify_image.py \
--model models/mobilenet_v2_1.0_224_inat_bird_quant_edgetpu.tflite \
--labels models/inat_bird_labels.txt \
--input images/parrot.jpg
```
---
----INFERENCE TIME----
Note: The first inference on Edge TPU is slow because it includes loading the model into Edge TPU memory.
13.6ms
2.8ms
2.7ms
2.7ms
2.7ms

-------RESULTS--------
Ara macao (Scarlet Macaw): 0.76562
---
看到這個代表已成功
---
# Edge TPU 連接相機以及運行範例
## 明白自己的usb相機支援格式 （順便檢查有沒有成功連接
```
v4l2-ctl --list-formats-ext --device /dev/video1
```
應出現類似
---
ioctl: VIDIOC_ENUM_FMT
Type: Video Capture

[0]: 'MJPG' (Motion-JPEG, compressed)
    Size: Discrete 640x480
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 320x240
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 800x600
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 1024x768
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 1280x720
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 1280x1024
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 1600x1200
        Interval: Discrete 0.067s (15.000 fps)
    Size: Discrete 1920x1080
        Interval: Discrete 0.067s (15.000 fps)
    Size: Discrete 2048x1536
        Interval: Discrete 0.067s (15.000 fps)
    Size: Discrete 2592x1944
        Interval: Discrete 0.067s (15.000 fps)
    Size: Discrete 640x480
        Interval: Discrete 0.033s (30.000 fps)
[1]: 'YUYV' (YUYV 4:2:2)
    Size: Discrete 640x480
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 320x240
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 800x600
        Interval: Discrete 0.033s (30.000 fps)
    Size: Discrete 1024x768
        Interval: Discrete 0.067s (15.000 fps)
    Size: Discrete 1280x720
        Interval: Discrete 0.133s (7.500 fps)
    Size: Discrete 1280x1024
        Interval: Discrete 0.133s (7.500 fps)
    Size: Discrete 1600x1200
        Interval: Discrete 0.333s (3.000 fps)
    Size: Discrete 1920x1080
        Interval: Discrete 0.333s (3.000 fps)
    Size: Discrete 2048x1536
        Interval: Discrete 0.333s (3.000 fps)
    Size: Discrete 2592x1944
        Interval: Discrete 0.333s (3.000 fps)
    Size: Discrete 640x480
        Interval: Discrete 0.033s (30.000 fps)
---

## 設置路徑變量
```
export DEMO_FILES="$HOME/demo_files"
```
下載以下
```
The image classification model and labels file
wget -P ${DEMO_FILES}/ https://github.com/google-coral/edgetpu/raw/master/test_data/mobilenet_v2_1.0_224_quant_edgetpu.tflite

wget -P ${DEMO_FILES}/ https://github.com/google-coral/edgetpu/raw/master/test_data/imagenet_labels.txt

The face detection model (does not require a labels file)
wget -P ${DEMO_FILES}/ https://github.com/google-coral/edgetpu/raw/master/test_data/mobilenet_ssd_v2_face_quant_postprocess_edgetpu.tflite
```
## 使用HDMI觀看 人臉辨識demo
```
edgetpu_detect \
--source /dev/video1video1:YUY2:1024x768:15/1 \
--model ${DEMO_FILES}/mobilenet_ssd_v2_face_quant_postprocess_edgetpu.tflite
```
### 使用網址觀看 人臉辨識demo （http://192.168.100.2:4664/
```
edgetpu_detect_server \
--source /dev/video1:YUY2:1024x768:15/1 \
--model ${DEMO_FILES}/mobilenet_ssd_v2_face_quant_postprocess_edgetpu.tflite
```
## 解釋指令意義
---
在自--source變量中（僅適用於USB攝像機），必須使用在USB攝像機設置期間打印的值指定4個參數：
/dev/video1是設備文件。如果是唯一連接的相機，則應該相同。
YUY2是唯一受支持的像素格式（與相同YUYV）。
800x600是圖像分辨率。這必須與您的相機列出的分辨率之一匹配。
24/1是幀速率。它還必須與給定格式的列出的FPS值之一匹配。
---
運行成功都應該看到底下有
INFO:edgetpuvision.streaming.server:Listening on ports tcp: 4665, web: 4664, annexb: 4666
---
# EdgeTPU_Coral建立Swap分區
此步驟提供給安裝工具，內存不足導致失敗時使用
每次開機均得重新掛載SWAP分區
### 查看硬碟資訊
```
df #全部
df /dev/xxxx #指定xxxx硬碟
```
### fdisk not found錯誤碼
目前狀況是因環境變數沒有設置導致
可以先輸入
```
whereis fdisk
```
確定有沒有這個套件
### 進入/etc資料夾
```
cd ..
cd ..
cd /etc
```
### 找到一個檔案叫做bash.bashrc並編輯
```
sudo nano bash.bashrc
```
### 在最後面輸入
```
PATH=$PATH:$HOME/bin:/sbin
#ctrl+O 保存
#ctrl+X離開
```
### 重新啟動tpu，並查詢PATH
```
echo $PATH
```
會發現:sbin已經加入
### 切割分區
首先查看硬碟資訊
```
sudo fdisk -l
```
查看記憶卡資訊
類似如下
```
Disk /dev/mmcblk1: 29.7 GiB, 31914983424 bytes, 62333952 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```
### 使用fdisk工具進入編輯
```
sudo fdisk  /dev/mmcblk1
```
出現
---
Welcome to fdisk (util-linux 2.33.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x64cb75f7.

Command (m for help): 
---
查看選項
```
m #輸入enter
```
```
s #create a new empty Sun partition table
```
### 首先刪除原本磁區
```
D #按ENTER輸入
```
### 建立新磁區

n
Partition type
p primary (0 primary, 0 extended, 4 free)
e extended (container for logical partitions)

Select (default p):
```
p /只要建立一個因此選擇主要分割區選p
```
Partition number (1-4, default 1):
```
預設排序就行 直接enter
```
First sector (2048-62333951, default 2048):
```
一樣預設就行 Enter
```
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-62333951, default 62333951):
選擇磁區大小 +xG 像我這裡要6g
```
+6G
```
之後輸入w保存並離開
```
w
```
輸入
```
sudo fdisk -l
```
可以看到新的
---
Device Boot Start End Sectors Size Id Type

/dev/mmcblk1p1 2048 12584959 12582912 6G 83 Linux
---
代表成功新增磁區

## 格式化並掛載磁區

### 新建完一個磁區後必須格式化後才能使用
```
sudo mkfs.ext4 /dev/mmcblk1p1
```
### 先進入底層 建立一個新資料夾 用來掛載磁區
```
cd ..
cd ..
sudo mkdir mountfile
```
### 掛載新磁區
```
sudo mount /dev/mmcblk1p1 /mountfile
```

## 在掛載的分割區中新建一個swap file 並應用套用
## 查看swap
```
swapon -s
or
free -h
```

## 建立一個空白檔案 dd工具會跑一陣子 請耐心等待
```
sudo dd if=/dev/zero of=./mem_4G.swp bs=4096 counts=1048576
```

## 建立swap交換區檔案
```
sudo mkswap ./mem_4G.swp
```
## 啟用swap
```
sudo swapon ./mem_4G.swp
```
---
# 物聯網_ESP32
本專案使用ESP32並與Mysql資料庫聯動

## 環境配置
在開始前請先安裝妥 CH340G USB to UART 驅動程式
[CH340G USB to UAR載點](https://sparks.gogo.co.nz/ch340.html)

### STEP.1 啟動 Arduino IDE 並點擊下拉功能表「檔案」>「偏好設定」
在偏好設定頁面找到「額外的開發版管理員網址」。
輸入
```
https://dl.espressif.com/dl/package_esp8266_index.json,https://dl.espressif.com/dl/package_esp32_index.json
```

### STEP.2 重新啟動Arduino IDE並點擊「工具」>「開發版」>「開發版管理員」
輸入 esp32，點擊「安裝」，然後等待幾分鐘下載完畢後點擊右下角落的「關閉」鈕關閉視窗。

### STEP.3 點擊下拉功能表「工具」>「開發版」，捲動到下方找到適合您的開發板，然後再選擇連接埠。
依照使用的ESP開發版型號作選擇，本專案使用「DOIT ESP32 DEVKIT V1」。

### STEP.4 本專案以Windows 10為範例，在本機電腦[文件]找到Arduino\libraries資料夾
將本專案使用到之程式庫解壓丟入 

[Arduino\libraries載點](https://drive.google.com/file/d/19YQ9WdW99EQ9eSvvlOBOF20QolmNIzzK/view?usp=sharing)
---
## ino檔編輯並燒入

### STEP.1 在WIFI以及MYSQL程式碼部分輸入自己的帳號密碼
```
IPAddress server_addr(140,xxx,xxx,xxx);   // 安裝Mysql的電腦的IP地址
char user[] = "xxx";              // Mysql的用戶名
char password[] = "xxxx";        // 登陸Mysql的密碼
```
```
char ssid[] = "TP-LINK_07B4";     // your network SSID (name)
char pass[] = "12345678910";  // your network password
```
### STEP.2 在程式各功能部分 定義Sensor的信號腳位
```
#define PH_PIN 35
#define oneWireBus 12  //溫度gpio_pin
#define waterSensor 13
```
### STEP.3 確認輸入正確，並且在ESP32插入好感測器腳位後，即可燒錄上傳
```
注意! ESP32的12腳位燒入時不能插上，否則會導致燒錄失敗
```

---
# LINE BOT

本程式只需在config檔案輸入LINEBOT上取得的金鑰權杖，以及輸入MYSQL網址、帳號密碼、資料庫即可上傳至HEROKU使用，不多贅述

---
# 參考文章

https://gilberttanner.com/blog/convert-your-tensorflow-object-detection-model-to-tensorflow-lite

https://gilberttanner.com/blog/creating-your-own-objectdetector

https://gilberttanner.com/blog/installing-the-tensorflow-object-detection-api

## *最後感謝在這專案所有貢獻者*
