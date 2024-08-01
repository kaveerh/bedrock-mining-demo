---
#Using Machine Learning model for Video Tracking of People

---


Segment Anything Model 2 (SAM 2) is a foundation model towards solving promptable visual segmentation in images and videos. We extend SAM to video by considering images as a video with a single frame. The model design is a simple transformer architecture with streaming memory for real-time video processing. We build a model-in-the-loop data engine, which improves model and data via user interaction, to collect our SA-V dataset, the largest video segmentation dataset to date. SAM 2 trained on our data provides strong performance across a wide range of tasks and visual domains.

:image[synthetic image generation]{src="/static/safety/model_diagram.png" }

![Open side bar](https://github.com/kaveerh/bedrock-mining-demo/blob/main//static/safety/model_diagram.png)


[Sample Safety video](https://ws-assets-prod-iad-r-pdx-f3b3f9f1a7d6a3d0.s3.us-west-2.amazonaws.com/761eaa28-7624-432c-9c2e-0b1b6c4bd953/workerzonedetection.mp4  "Sample Safety video")




[demo of Segment Anything ](https://sam2.metademolab.com/)
