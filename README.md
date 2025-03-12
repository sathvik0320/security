# Documentation 
## main.py (perception)

- YOLO-based Segmentation:
    Uses a pre-trained YOLO model to segment objects in video frames(yolo11n-seg.pt).
- eCAL Messaging:
    Publishes detected object data, segmentation masks eCAL.
- Lowest Point Detection:
    Identifies the lowest points of segmented objects near a vertical centerline.

> * code explanation *
-  mains imports 
    ~~~
    from ultralytics import YOLO
    import cv2, numpy as np, torch
    import ecal.core.core as ecal_core
    from ecal.core.publisher import ProtoPublisher
    import pickle, pathlib as Path
    import torch.nn.functional as F
    import proto_messages.schemas.proto.data_pb2 as _data
    ~~~
  - importing yolo for detecting objects and segmentation of objects , ecal for publishing and receiving the messages(data) from and     to different nodes

- perception node (initialization)

  ~~~
  ecal_core.initialize(sys.argv, "mask publisher")
  ~~~
  - initializing perception node
  ~~~
  pub_bb = ProtoPublisher("low_points", _data.Data)
  ~~~
  - creating a publisher to send data(lowest_points) to another nodes

- Loading pre-trained model

  ~~~
  model = YOLO("yolo11n-seg.pt")
  ~~~

  - Loading "yolo11n-seg.pt" for instant segmentation of the objects in input frame(live,video,image)
 
- Input to model
  ~~~
  cap = cv2.VideoCapture(video_path) 
  ~~~
  - replace this line of code with necessary mode of input (webcam,image,video)

- Lowest points
  ~~~
  def find_lowest_points_near_line(mask, line_x_func):
  ~~~
  - After passing input to model ,output(masks) are sclaed and passed to above function to find the lowest points of each masks and      stores in as list 

- Publishing
    ~~~
    # Send lowest points data via eCAL
    print("lowest points", lowest_points)
    bb_bytes = pickle.dumps(np.array(lowest_points))  # Serialize lowest points data
    bb_proto = _data.Data()
    bb_proto.data = bb_bytes
    pub_bb.send(bb_proto)  # Publish lowest points data
    ~~~
    - This part of the code will helps to publish the data(lowest points to) using ecal nodes subscribed to it
      
