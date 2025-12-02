# Urban Scene Annotation

**Goal:** Provide high-quality annotations for an object detection computer vision model. Accurate, consistent, and high-quality data annotation is crucial for AI models to make predictions and classifications on seen objects as accurately as possible. Annotating objects in images helps models learn how to detect objects in the real world and through images.

## Background

This project focuses on creating a robust dataset for training AI models to classify various objects found in an urban environment. Four types of annotation are applied:

- Cars and pedestrians are labeled using **instance segmentation**, so models can distinguish between individual instances.

- Road surfaces are labeled using **semantic segmentation** because models only need to classify every pixel as a road or not a road, and individual road segments do not require a unique ID.

- Traffic signs use **bounding boxes** because they are typically small, and the model mainly needs to know the location and class of signs. This also increases the speed and efficiency of annotating an image.

- All images have one **image-wide classification tag**. This is so the model knows how objects look in certain environments, lighting, and weather conditions. These tags are `Daytime_Clear`, `Daytime_Cloudy`, `Daytime_Rainy`, `Nighttime_Clear`, `Nighttime_Cloudy`, and `Nighttime_Rainy`.

By using multiple types of labeling, I cover a wide variety of use cases and create a comprehensive dataset. In providing a high-quality and diverse dataset, AI models can be trained to produce high-accuracy results when detecting various objects found in urban scenes.

## Dataset Description

The dataset consists of 20 images downloaded from the royalty-free website Unsplash. The images represent a mixture of daytime and nighttime scenes, as well as different weather conditions, such as cloudy and rainy. The primary objects of interest are cars, pedestrians, road surfaces, and traffic signs.

## Annotation Methodology

I utilized the CVAT.ai website for the annotation project. Each car and pedestrian was labeled using the mask tool and use complementary color-coding (pedestrians are green, cars are red). Road surfaces were labeled using the mask tool and are colored indigo to be visible under labeled cars and pedestrians. Traffic signs are labeled with bounding boxes that are rotated to fit the sign as tightly as possible.

A tight approach was used for mask annotations to minimize over-segmentation. Masks typically began with the polygon tool to cover outlines as tightly as possible, while the brush tool was used to fill in or remove small mistakes.

Strict guidelines were created and adhered to ensure high-quality annotations and to remove ambiguity and guesswork from the project. Each image was double-checked against the guidelines after completion to ensure annotation quality.

- **Example 1: Occlusion** - Guidelines were established for handling occluded objects. Objects that were less than 25% visible were not annotated.
- **Example 2: Reflections and Shadows** - Addresses whether to annotate shadows and reflections.
- **Example 3: Reducing Ambiguity** - Defines what rules to follow for ambiguous objects.

Guidelines can be found here: [Guidelines.pdf](guidelines.pdf)

Since CVAT.ai lacked a way to measure the size of masks, I developed a QA workaround by using a temporary bounding box to measure adherence to minimum size guidelines. This bounding box was wrapped around an object that appeared very small. If the object did not pass size guidelines, it was not annotated.

After the annotations were complete, they were exported from CVAT.ai into the **CVAT for Images 1.1 (XML)** format to be processed in Python to gather statistics.

## Visual Example

<img alt="An annotated image of a shadowed street" src="example 1.png" width=33%></img>

<img alt="An annotated image of an open street" src="example 2.png" width=33%></img>

<img alt="An annotated image of cloudy urban scene" src="example 3.png" width=33%></img>

<span style="background-color: #fa3253">CAR (Instance Segmentation)</span> objects are labeled with red to *contrast* against road surfaces.

<span style="background-color: #3d3df5">ROAD_SURFACE (Semantic Segmentation)</span> objects are labeled in indigo.

<span style="background-color: #FFCC33; color: black;">TRAFFIC_SIGN (Bounding Box)</span> objects are labeled yellow to *contrast* against road surfaces.

<span style="background-color: #3df53d; color: black;">PEDESTRIAN (Instance Segmentation)</span> objects are labeled green to *contrast* against both road surfaces and cars.

Note the application of **instance segmentation** on occluded cars, and the **tight polygon approach** used for the road surface mask.

## Data Deliverable (.xml)

```xml
  <image id="0" name="adam-borkowski-NyPV7oHdlSo-unsplash.jpg" subset="default" task_id="1757241" width="5720" height="7150">
    <mask label="PEDESTRIAN" source="manual" occluded="1" rle="77, 10, 110, 24, 97, 29, 96, 30, 95, 32, 89, 37, 88, 39, 86, 41, 83, 44, 81, 45, 80, 47, 78, 49, 76, 51, 74, 52, 72, 54, 71, 56, 69, 57, 68, 59, 66, ..." left="4773" top="3680" width="126" height="621" z_order="0">
      <attribute name="Visibility">Partial_Occlusion</attribute>
    </mask>
    ...
    <box label="TRAFFIC_SIGN" source="manual" occluded="0" xtl="4216.52" ytl="3382.89" xbr="4396.81" ybr="3689.65" z_order="0">
      <attribute name="Visibility">Full</attribute>
    </box>
    ...
    <mask label="ROAD_SURFACE" source="manual" occluded="0" rle="1703, 3, 1, 1, 5707, 9, 5704, 12, 5701, 14, 5704, 12, 5708, 8, 5711, 4, 12167, 2, 5717, 4, 5714, 7, 5711, 10, 5708, 13, 4915, 6, 784, 15, 4908, ..." left="0" top="4334" width="5720" height="2816" z_order="0">
      <attribute name="Visibility">Full</attribute>
    </mask>
    ...
    <mask label="CAR" source="manual" occluded="1" rle="877, 2, 1632, 6, 1628, 7, 1627, 9, 1625, 10, 1624, 11, 1623, 12, 1623, 13, 1621, ..." left="2346" top="3475" width="1634" height="1208" z_order="0">
      <attribute name="Visibility">Partial_Occlusion</attribute>
    </mask>
    ...
      <attribute name="Visibility">Full</attribute>
    </mask>
    <tag label="Daytime_Rainy" source="manual">
    </tag>
  </image>
```

Full annotations file: [annotations.xml](annotations.xml)

## Results

- A total of **340** objects were annotated.
- **161** cars were annotated.
- **74** traffic signs were annotated.
- **65** pedestrians were annotated.

The data displays a significant class imbalance between `CAR` (161 instances) and `PEDESTRIAN` (65 instances).

This imbalance is documented as a key data characteristic that may require model training techniques (e.g., re-weighting or oversampling) to prevent bias towards the `CAR` class.

## Conclusion

This project successfully created a high-quality labeled dataset that can be applied to object detection AI models.

Future work should focus on actively balancing the dataset by sourcing scenes with a higher density of currently underrepresented classes. The dataset should also be expanded to include other common urban classes such as buses, bicycles and motorcycles to increase the model's ability to identify a broader range of real-world urban objects.
