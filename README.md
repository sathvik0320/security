# Documentation 
## main.py (perception)

- YOLO-based Segmentation:
    Uses a pre-trained YOLO model to segment objects in video frames(yolo11n-seg.pt).
- eCAL Messaging:
    Publishes detected object data, segmentation masks eCAL.
- Lowest Point Detection:
    Identifies the lowest points of segmented objects near a vertical centerline.

> * code explanation *
  - mains imports 
    - ~~~
    from ultralytics import YOLO
    import cv2, numpy as np, torch
    import ecal.core.core as ecal_core
    from ecal.core.publisher import ProtoPublisher
    import pickle, pathlib as Path
    import torch.nn.functional as F
    import proto_messages.schemas.proto.data_pb2 as _data
    ~~~
    


