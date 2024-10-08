⚠️ Work in progress, needs a massage.

# Geospatial Data Processing and Analysis

This field involves the collection, processing, and analysis of spatial and geographic data, often using advanced technologies like drones, LiDAR, and photogrammetry. Here are some key areas within this specialty:

## Key Areas

- Remote Sensing: Capturing data from a distance using drones, satellites, or other sensors.
- Photogrammetry: Using photographs to measure and map distances between objects.
- LiDAR (Light Detection and Ranging): Using laser pulses to create high-resolution maps and 3D models.
- Geospatial Analysis: Analyzing spatial data to extract meaningful information and insights.
- GIS (Geographic Information Systems): Managing and analyzing geographic data using specialized software.

## Applications
- Urban Planning: Creating detailed maps and models for city planning and development.
- Environmental Monitoring: Tracking changes in landscapes, forests, and water bodies.
- Agriculture: Monitoring crop health and managing fields more efficiently.
- Construction and Engineering: Surveying land and creating accurate models for construction projects.
- Disaster Management: Assessing damage and planning recovery efforts after natural disasters.

## Tools and Technologies
- Software: OpenCV, OpenMVG, OpenMVS, PCL, GDAL, QGIS.
- Cloud Services: AWS Lambda, Amazon S3, AWS Batch, Azure Maps, Azure AI.
- Devices: Drones, LiDAR-equipped devices like the iPhone 12 Pro.

This specialty combines elements of computer science, geography, and engineering to solve complex spatial problems and create detailed visualizations.

Here’s a comprehensive guide including app suggestions for capturing LiDAR data on your iPhone, methods for streaming this data to the cloud, and detailed examples of what you can achieve with AWS Lambda for image and LiDAR data processing.

## **Apps for Capturing LiDAR Data on iPhone**
1. **Polycam**
   - **Features**: Capture high-quality 3D models of objects and spaces.
   - **Use Case**: Ideal for creating detailed 3D scans for architecture, design, and AR applications¹(https://www.howtogeek.com/759121/your-iphone-pro-has-lidar-7-cool-things-you-can-do-with-it/).

2. **Canvas**
   - **Features**: Scan rooms and spaces to create accurate floor plans and 3D models.
   - **Use Case**: Perfect for real estate, interior design, and construction planning¹(https://www.howtogeek.com/759121/your-iphone-pro-has-lidar-7-cool-things-you-can-do-with-it/).

3. **3D Scanner App**
   - **Features**: Capture and export 3D models in various formats.
   - **Use Case**: Useful for general 3D scanning and exporting models for further processing¹(https://www.howtogeek.com/759121/your-iphone-pro-has-lidar-7-cool-things-you-can-do-with-it/).

4. **RoomScan LiDAR**
   - **Features**: Create detailed floor plans and 3D models of interiors.
   - **Use Case**: Great for home improvement, real estate, and space planning¹(https://www.howtogeek.com/759121/your-iphone-pro-has-lidar-7-cool-things-you-can-do-with-it/).

### **Egress Options for iPhone LiDAR Data**
1. **Direct Upload to Cloud Storage**
   - **Method**: Use apps like Polycam or Canvas to export LiDAR data directly to cloud storage services like Google Drive, Dropbox, or iCloud.
   - **Use Case**: Convenient for storing and accessing data from multiple devices.

2. **Streaming via Wi-Fi or USB**
   - **Method**: Use apps like Record3D to stream LiDAR data to a local server or cloud service.
   - **Use Case**: Real-time data transfer for immediate processing and analysis²(https://www.youtube.com/watch?v=UzLzLQQJC30).

## **AWS Lambda Image and LiDAR Data Processing Examples**

### **1. Orthomosaics with OpenCV**
- **Objective**: Stitch multiple drone images into a single orthomosaic.
- **Lambda Function**: Use OpenCV to process and stitch images.

```python
import boto3
import cv2
import numpy as np

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Load images (assuming multiple images are uploaded)
    images = [cv2.imread(download_path)]
    
    # Stitch images
    stitcher = cv2.Stitcher_create()
    status, stitched = stitcher.stitch(images)
    
    if status == cv2.Stitcher_OK:
        stitched_path = '/tmp/stitched.jpg'
        cv2.imwrite(stitched_path, stitched)
        s3.upload_file(stitched_path, bucket, 'stitched.jpg')
        return {
            'statusCode': 200,
            'body': 'Stitched image uploaded successfully!'
        }
    else:
        return {
            'statusCode': 500,
            'body': 'Image stitching failed!'
        }
```

### **2. 3D Models with OpenMVG/OpenMVS**
- **Objective**: Create 3D models from drone images.
- **Lambda Function**: Preprocess images and use AWS Batch to run OpenMVG and OpenMVS for 3D reconstruction.

```python
import boto3
import subprocess

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Run OpenMVG and OpenMVS commands
    subprocess.run(['openMVG_main_SfMInit_ImageListing', '-i', download_path, '-o', '/tmp/sfm'])
    subprocess.run(['openMVG_main_ComputeFeatures', '-i', '/tmp/sfm/sfm_data.json', '-o', '/tmp/sfm'])
    subprocess.run(['openMVG_main_ComputeMatches', '-i', '/tmp/sfm/sfm_data.json', '-o', '/tmp/sfm'])
    subprocess.run(['openMVG_main_IncrementalSfM', '-i', '/tmp/sfm/sfm_data.json', '-o', '/tmp/sfm', '-m', '/tmp/sfm'])
    subprocess.run(['openMVG_main_openMVG2openMVS', '-i', '/tmp/sfm/sfm_data.json', '-o', '/tmp/mvs/scene.mvs'])
    subprocess.run(['DensifyPointCloud', '/tmp/mvs/scene.mvs'])
    subprocess.run(['ReconstructMesh', '/tmp/mvs/scene_dense.mvs'])
    subprocess.run(['TextureMesh', '/tmp/mvs/scene_dense_mesh.mvs'])
    
    # Upload the 3D model to S3
    s3.upload_file('/tmp/mvs/scene_dense_mesh_texture.ply', bucket, '3d_model.ply')
    
    return {
        'statusCode': 200,
        'body': '3D model created and uploaded successfully!'
    }
```

### **3. Point Clouds with PCL**
- **Objective**: Generate and process point clouds from LiDAR data.
- **Lambda Function**: Use PCL to process LiDAR data and generate point clouds.

```python
import boto3
import pcl

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Load the LiDAR data
    cloud = pcl.load(download_path)
    
    # Process the point cloud (example: downsample)
    sor = cloud.make_voxel_grid_filter()
    sor.set_leaf_size(0.01, 0.01, 0.01)
    cloud_filtered = sor.filter()
    
    # Save the processed point cloud
    processed_path = '/tmp/processed.pcd'
    pcl.save(cloud_filtered, processed_path)
    s3.upload_file(processed_path, bucket, 'processed.pcd')
    
    return {
        'statusCode': 200,
        'body': 'Point cloud processed and uploaded successfully!'
    }
```

### **4. Digital Surface Models (DSM) with GDAL**
- **Objective**: Generate DSMs from drone images.
- **Lambda Function**: Use GDAL to process images and create DSMs.

```python
import boto3
import subprocess

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Run GDAL commands to generate DSM
    subprocess.run(['gdal_translate', '-of', 'GTiff', download_path, '/tmp/dsm.tif'])
    subprocess.run(['gdalwarp', '-r', 'bilinear', '-t_srs', 'EPSG:4326', '/tmp/dsm.tif', '/tmp/dsm_warped.tif'])
    
    # Upload the DSM to S3
    s3.upload_file('/tmp/dsm_warped.tif', bucket, 'dsm.tif')
    
    return {
        'statusCode': 200,
        'body': 'DSM created and uploaded successfully!'
    }
```

### **5. Digital Terrain Models (DTM) with GDAL**
- **Objective**: Generate DTMs from LiDAR data.
- **Lambda Function**: Use GDAL to process LiDAR data and create DTMs.

```python
import boto3
import subprocess

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Run GDAL commands to generate DTM
    subprocess.run(['gdal_translate', '-of', 'GTiff', download_path, '/tmp/dtm.tif'])
    subprocess.run(['gdalwarp', '-r', 'bilinear', '-t_srs', 'EPSG:4326', '/tmp/dtm.tif', '/tmp/dtm_warped.tif'])
    
    # Upload the DTM to S3
    s3.upload_file('/tmp/dtm_warped.tif', bucket, 'dtm.tif')
    
    return {
        'statusCode': 200,
        'body': 'DTM created and uploaded successfully!'
    }
```

### **6. Contour Maps with QGIS**
- **Objective**: Generate contour maps from DSM or DTM data.
- **Lambda Function**: Use QGIS to process DSM or DTM data and create contour maps.

```python
import boto3
import subprocess

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
```
Source: Conversation with Copilot, 10/1/2024
- (1) Your iPhone Pro Has LiDAR: 7 Cool Things You Can Do With It - How-To Geek. https://www.howtogeek.com/759121/your-iphone-pro-has-lidar-7-cool-things-you-can-do-with-it/.
- (2) How to Use LiDAR on iPhone & iPad -- What Can It Do?. https://www.youtube.com/watch?v=UzLzLQQJC30.
- (3) github.com. https://github.com/interkid/demo-facerecognition/tree/8042e5d5be6c0ba10ee18a802572fce240af7e00/testcode%2Flambda_test.py.
- (4) github.com. https://github.com/vodelerk/AwsStepFunctions/tree/56b726cb6e18a4b162da0b43462f3ae8ed529e58/TriggerLambdaFunction.py.
- (5) github.com. https://github.com/SundarAnand/Custom-Face-recognition-using-AWS-rekognition/tree/8188af92d00bc79de37eeb27a33509dc8eca9126/lambda_function-2.py.

---

### **1. Orthomosaics**
- **Service**: AWS Lambda, Amazon S3, and OpenCV
- **Process**:
  1. **Upload Images**: Store your drone images in an S3 bucket.
  2. **Lambda Function**: Use AWS Lambda to process the images and stitch them together using OpenCV.

```python
import boto3
import cv2
import numpy as np

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Load images (assuming multiple images are uploaded)
    images = [cv2.imread(download_path)]
    
    # Stitch images
    stitcher = cv2.Stitcher_create()
    status, stitched = stitcher.stitch(images)
    
    if status == cv2.Stitcher_OK:
        stitched_path = '/tmp/stitched.jpg'
        cv2.imwrite(stitched_path, stitched)
        s3.upload_file(stitched_path, bucket, 'stitched.jpg')
        return {
            'statusCode': 200,
            'body': 'Stitched image uploaded successfully!'
        }
    else:
        return {
            'statusCode': 500,
            'body': 'Image stitching failed!'
        }
```

### **2. 3D Models**
- **Service**: AWS Lambda, Amazon S3, and OpenMVG/OpenMVS
- **Process**:
  1. **Upload Images**: Store your drone images in an S3 bucket.
  2. **Lambda Function**: Trigger a Lambda function to preprocess the images.
  3. **AWS Batch**: Use AWS Batch to run OpenMVG and OpenMVS for 3D reconstruction.

```python
import boto3
import subprocess

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Run OpenMVG and OpenMVS commands
    subprocess.run(['openMVG_main_SfMInit_ImageListing', '-i', download_path, '-o', '/tmp/sfm'])
    subprocess.run(['openMVG_main_ComputeFeatures', '-i', '/tmp/sfm/sfm_data.json', '-o', '/tmp/sfm'])
    subprocess.run(['openMVG_main_ComputeMatches', '-i', '/tmp/sfm/sfm_data.json', '-o', '/tmp/sfm'])
    subprocess.run(['openMVG_main_IncrementalSfM', '-i', '/tmp/sfm/sfm_data.json', '-o', '/tmp/sfm', '-m', '/tmp/sfm'])
    subprocess.run(['openMVG_main_openMVG2openMVS', '-i', '/tmp/sfm/sfm_data.json', '-o', '/tmp/mvs/scene.mvs'])
    subprocess.run(['DensifyPointCloud', '/tmp/mvs/scene.mvs'])
    subprocess.run(['ReconstructMesh', '/tmp/mvs/scene_dense.mvs'])
    subprocess.run(['TextureMesh', '/tmp/mvs/scene_dense_mesh.mvs'])
    
    # Upload the 3D model to S3
    s3.upload_file('/tmp/mvs/scene_dense_mesh_texture.ply', bucket, '3d_model.ply')
    
    return {
        'statusCode': 200,
        'body': '3D model created and uploaded successfully!'
    }
```

### **3. Point Clouds**
- **Service**: AWS Lambda, Amazon S3, and PCL (Point Cloud Library)
- **Process**:
  1. **Upload LiDAR Data**: Store your LiDAR data files in an S3 bucket.
  2. **Lambda Function**: Use AWS Lambda to process the LiDAR data and generate point clouds using PCL.

```python
import boto3
import pcl

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Load the LiDAR data
    cloud = pcl.load(download_path)
    
    # Process the point cloud (example: downsample)
    sor = cloud.make_voxel_grid_filter()
    sor.set_leaf_size(0.01, 0.01, 0.01)
    cloud_filtered = sor.filter()
    
    # Save the processed point cloud
    processed_path = '/tmp/processed.pcd'
    pcl.save(cloud_filtered, processed_path)
    s3.upload_file(processed_path, bucket, 'processed.pcd')
    
    return {
        'statusCode': 200,
        'body': 'Point cloud processed and uploaded successfully!'
    }
```

### **4. Digital Surface Models (DSM)**
- **Service**: AWS Lambda, Amazon S3, and GDAL
- **Process**:
  1. **Upload Images**: Store your drone images in an S3 bucket.
  2. **Lambda Function**: Preprocess the images.
  3. **EC2 Instance**: Use an EC2 instance with GDAL to generate DSMs from the processed images.

```python
import boto3
import subprocess

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Run GDAL commands to generate DSM
    subprocess.run(['gdal_translate', '-of', 'GTiff', download_path, '/tmp/dsm.tif'])
    subprocess.run(['gdalwarp', '-r', 'bilinear', '-t_srs', 'EPSG:4326', '/tmp/dsm.tif', '/tmp/dsm_warped.tif'])
    
    # Upload the DSM to S3
    s3.upload_file('/tmp/dsm_warped.tif', bucket, 'dsm.tif')
    
    return {
        'statusCode': 200,
        'body': 'DSM created and uploaded successfully!'
    }
```

### **5. Digital Terrain Models (DTM)**
- **Service**: AWS Lambda, Amazon S3, and GDAL
- **Process**:
  1. **Upload LiDAR Data**: Store your LiDAR data files in an S3 bucket.
  2. **Lambda Function**: Preprocess the LiDAR data.
  3. **EC2 Instance**: Use an EC2 instance with GDAL to generate DTMs from the processed data.

```python
import boto3
import subprocess

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Run GDAL commands to generate DTM
    subprocess.run(['gdal_translate', '-of', 'GTiff', download_path, '/tmp/dtm.tif'])
    subprocess.run(['gdalwarp', '-r', 'bilinear', '-t_srs', 'EPSG:4326', '/tmp/dtm.tif', '/tmp/dtm_warped.tif'])
    
    # Upload the DTM to S3
    s3.upload_file('/tmp/dtm_warped.tif', bucket, 'dtm.tif')
    
    return {
        'statusCode': 200,
        'body': 'DTM created and uploaded successfully!'
    }
```

### **6. Contour Maps**
- **Service**: AWS Lambda, Amazon S3, and QGIS
- **Process**:
  1. **Upload DSM or DTM**: Store your DSM or DTM files in an S3 bucket.
  2. **Lambda Function**: Preprocess the DSM or DTM.
  3. **EC2 Instance**: Use an EC2 instance with QGIS to generate contour maps from the DSM or DTM data.

```python
import boto3
import subprocess

s3 = boto3.client('s3')

def lambda_handler(event, context):
    bucket = event['Records']['s3']['bucket']['name']
    key = event['Records']['s3']['object']['key']
    
    download_path = f'/tmp/{key}'
    s3.download_file(bucket, key, download_path)
    
    # Run QGIS commands to generate contour maps
    subprocess.run(['qgis_process', 'run', 'native:contour', '--', 'INPUT=/tmp/dsm.tif', 'BAND=1', 'INTERVAL=10', 'OUTPUT=/tmp/contours.shp'])
    
    # Upload the contour map to S3
    s3.upload_file('/tmp/contours.shp', bucket, 'contours.shp')
```
Source: Conversation with Copilot, 10/1/2024
- (1) github.com. https://github.com/interkid/demo-facerecognition/tree/8042e5d5be6c0ba10ee18a802572fce240af7e00/testcode%2Flambda_test.py.
- (2) github.com. https://github.com/vodelerk/AwsStepFunctions/tree/56b726cb6e18a4b162da0b43462f3ae8ed529e58/TriggerLambdaFunction.py.
- (3) github.com. https://github.com/SundarAnand/Custom-Face-recognition-using-AWS-rekognition/tree/8188af92d00bc79de37eeb27a33509dc8eca9126/lambda_function-2.py.

----

## Practical applications of iPhone LIDAR
iPhone Pro models from 12 onwards have a LIDAR scanner. What can we practically do with this as a hobbyist wanting to so a few experimental labs?

### **3D Point Cloud Processing with CloudCompare**
**Objective**: Process and analyze 3D point clouds captured with your iPhone.
**Tools**: iPhone with LiDAR, 3D Scanner App, CloudCompare (desktop software).
**Steps**:
1. Use the 3D Scanner App to capture a 3D scan.
2. Export the scan as a LAS file.
3. Import the LAS file into CloudCompare.
4. Perform operations like filtering, segmentation, and measurement on the point cloud¹(https://opentopography.org/blog/iphone-lidar-applications-geosciences).

### **Topographic Mapping with QGIS**
**Objective**: Create topographic maps from LiDAR data.
**Tools**: iPhone with LiDAR, SiteScape App, QGIS (desktop GIS software).
**Steps**:
1. Capture terrain data using the SiteScape App.
2. Export the data as a point cloud file (e.g., LAS).
3. Import the point cloud into QGIS.
4. Generate a Digital Elevation Model (DEM) and create contour maps²(https://3dinsider.com/iphone-lidar-scanner/).

### **Environmental Monitoring with Python and PDAL**
**Objective**: Monitor environmental changes using LiDAR data.
**Tools**: iPhone with LiDAR, 3D Scanner App, Python, PDAL (Point Data Abstraction Library).
**Steps**:
1. Capture environmental data with the 3D Scanner App.
2. Export the data as a LAS file.
3. Use PDAL in Python to process the point cloud.
4. Analyze changes over time by comparing multiple scans³(https://www.protocols.io/view/iphone-lidar-tutorial-yxmvm21w9g3p/v3).

### **Indoor Mapping with Matterport**
**Objective**: Create detailed indoor maps and virtual tours.
**Tools**: iPhone with LiDAR, Matterport App, Matterport Cloud.
**Steps**:
1. Scan indoor spaces using the Matterport App.
2. Upload the scans to the Matterport Cloud.
3. Use the Matterport platform to create detailed floor plans and virtual tours⁴(https://forums.developer.apple.com/forums/thread/674623).

### **Archaeological Site Survey with ArcGIS Online**
**Objective**: Survey and analyze archaeological sites.
**Tools**: iPhone with LiDAR, 3D Scanner App, ArcGIS Online.
**Steps**:
1. Capture site data with the 3D Scanner App.
2. Export the data as a point cloud file.
3. Upload the point cloud to ArcGIS Online.
4. Use ArcGIS tools to analyze and visualize the site⁵(https://www.it-jim.com/blog/iphones-12-pro-lidar-how-to-get-and-interpret-data/).

### **LIDAR enhanced Orthomosaics**
**Objective**: High-resolution, georeferenced aerial images stitched together. Orthomosaics do not need LIDAR, but can be enhanced by using it.
**Applications**
- **Urban Planning**: Create detailed maps of urban areas for planning and development.
- **Environmental Monitoring**: Monitor changes in landscapes, vegetation, and other environmental factors.
- **Archaeological Surveys**: Map and analyze archaeological sites with high precision.
**Steps**
1. **Capture LiDAR Data**: Use an app like "3D Scanner App" or "Polycam" on your iPhone to capture LiDAR data of the area you want to map.
2. **Capture Aerial Imagery**: If possible, use a drone to capture high-resolution aerial images of the same area. This will provide the necessary imagery for the orthomosaic.
3. **Export Data**: Export the LiDAR data as a point cloud file (e.g., LAS or PLY) and the aerial images in a format compatible with your photogrammetry software (e.g., JPEG or TIFF).
4. **Process LiDAR Data**: Use software like CloudCompare or PDAL to process the LiDAR data. This can include filtering, noise reduction, and generating a Digital Elevation Model (DEM).
5. **Photogrammetry Software**: Use photogrammetry software like Agisoft Metashape, Pix4D, or OpenDroneMap to create the orthomosaic. These tools can integrate LiDAR data to improve the accuracy of the orthomosaic.
6. **Combine Data**: Import both the aerial images and the processed LiDAR data into the photogrammetry software. The software will use the LiDAR data to correct distortions and improve the accuracy of the orthomosaic.

## Libraries
**3D Scanning and Modeling**
   - **Tools**: Polycam, 3D Scanner App, OpenMVG, OpenMVS,
**Point Clouds**
   - **Tools**: PCL (Point Cloud Library), Open3D
**Digital Surface Models (DSM)**
**Digital Terrain Models (DTM)**
**Contour Maps**
   - **Tools**: QGIS, GDAL
