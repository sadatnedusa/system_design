# Live Streaming Project Architecture

<img width="2528" alt="image" src="https://github.com/user-attachments/assets/dcb85c27-4775-4ab6-98b0-f1b09b3a9b73">


### Summary of "Live Streaming Architecture"

The **Live Streaming Architecture** refers to the comprehensive framework that supports the real-time transmission and delivery of video and audio content over the internet. It enables live events (e.g., sports, concerts, webinars) to be streamed to various devices, including browsers, mobile phones, and smart TVs. The architecture is composed of multiple components, each with specific roles in ensuring smooth and efficient live streaming. 

#### Key Components:

1. **Live Event Capture**:
   - **Recording**: The live event is captured using cameras and microphones, where both video and audio streams are recorded. These signals are then converted into digital formats for encoding.

2. **Encoding**:
   - **Encoding**: The recorded content is compressed and encoded into a suitable format (e.g., H.264 video and AAC audio). The encoded data is usually optimized for different bitrates and resolutions to ensure compatibility with varying network speeds and device capabilities.

3. **RTMP/RTSP Protocols**:
   - **RTMP/RTSP**: The encoded stream is then transmitted to the streaming server using Real-Time Messaging Protocol (RTMP) or Real-Time Streaming Protocol (RTSP), which ensures the video can be broadcast to the audience in real-time.

4. **Streaming Server**:
   - **Streaming Server Engine**: The streaming server processes the incoming stream, handles requests from clients, and distributes the stream to different endpoints.
   - **Recording Module**: Stores the live video for later use or on-demand playback.
   
5. **Content Storage**:
   - **Segment Storage/Cloud Storage**: The streaming content is divided into small segments (chunks) for easier and faster delivery, stored on cloud servers or dedicated storage systems. Cloud storage ensures scalability and global reach.

6. **Content Delivery Network (CDN)**:
   - **CDN Interconnect**: Content is distributed across the globe using a CDN, which caches and serves the content from edge locations closest to end-users. This reduces latency and improves delivery speed, ensuring a smooth viewing experience.
   - **Fastly CDN**: A specialized CDN provider like Fastly can optimize content delivery by using edge servers to quickly serve content to users.

7. **End-User Devices**:
   - **Browser/Client Devices**: End users access the stream through web browsers on desktops and laptops, mobile devices, or dedicated streaming players. These devices interact with the CDN to retrieve and play the live content in real-time.

### Flow Overview:

The general flow starts with the live event being captured, followed by encoding and transmission to the streaming server. 
The server processes and stores the content while distributing it to a CDN.
The CDN efficiently delivers the content to end-user devices, ensuring minimal delay and high quality across diverse networks and devices.



---
## **Encoding **
Encoding is a critical process in a streaming project that involves converting raw video and audio files into a digital format suitable for streaming.
It compresses the media while maintaining acceptable quality, making it feasible to transmit over the internet efficiently.

### **Why Encoding is Important**
1. **Bandwidth Efficiency**: Reduces file size to match the viewer's internet speed without buffering.
2. **Compatibility**: Ensures the content is playable across various devices and platforms.
3. **Scalability**: Allows multiple quality levels (adaptive bitrate streaming) for diverse user needs.

### **Key Concepts in Encoding**
#### 1. **Codecs**
   - **Definition**: A codec (encoder/decoder) is a software or hardware used to compress and decompress media.
   - **Types**:
     - **Video Codecs**:
       - H.264/AVC: Most widely used; balances quality and compression.
       - H.265/HEVC: Better compression efficiency than H.264; ideal for 4K/8K.
       - VP9: Open-source alternative by Google.
       - AV1: New open-source codec offering superior compression.
     - **Audio Codecs**:
       - AAC (Advanced Audio Codec): Common for streaming.
       - MP3: Legacy format.
       - Opus: High-quality and low-latency.
   - **Selection Criteria**: Compatibility, performance, and use case (e.g., live streaming or VOD).

#### 2. **Containers**
   - **Definition**: A container bundles encoded video and audio streams along with metadata (e.g., subtitles, chapters).
   - **Examples**:
     - MP4: Widely compatible and used for VOD.
     - MKV: Supports multiple codecs and subtitles.
     - FLV: Often used with RTMP.
     - TS: Common in live streaming (e.g., HLS).

#### 3. **Bitrate**
   - **Definition**: The amount of data processed per second (measured in kbps or Mbps).
   - **Types**:
     - **CBR (Constant Bitrate)**: Fixed bitrate; predictable but less efficient.
     - **VBR (Variable Bitrate)**: Adjusts bitrate based on content; more efficient.
   - **Impact**:
     - Higher bitrate = Better quality + Higher bandwidth.
     - Lower bitrate = Lower quality + Reduced bandwidth needs.

#### 4. **Resolution and Frame Rate**
   - **Resolution**: Determines the clarity of the video (e.g., 720p, 1080p, 4K).
   - **Frame Rate**: Frames per second (fps); common rates are 24, 30, and 60 fps. Higher frame rates provide smoother motion but require more resources.

#### 5. **Encoding Presets**
   - **Definition**: Pre-configured settings in encoders to balance speed, quality, and efficiency.
   - **Examples**:
     - Faster presets: Quicker encoding but lower compression.
     - Slower presets: Higher compression but more time-intensive.

### **Tools for Encoding**
#### 1. **Software Encoders**
   - **FFmpeg**: Command-line tool for encoding and processing media.
   - **OBS Studio**: Popular for live streaming.
   - **HandBrake**: User-friendly for VOD encoding.
   
#### 2. **Hardware Encoders**
   - Use dedicated chips for encoding, providing faster and more efficient processing.
   - Examples:
     - NVIDIA NVENC (GPU-based).
     - Intel Quick Sync.
     - Dedicated appliances (e.g., Teradek, AJA).

#### 3. **Cloud Encoding**
   - Services like AWS Elemental, Google Media APIs, or Azure Media Services provide scalable encoding solutions.


### **Streaming Protocols and Encoding**
- **HLS (HTTP Live Streaming)**: Uses H.264/H.265 and AAC with `.m3u8` playlists.
- **DASH (Dynamic Adaptive Streaming over HTTP)**: Codec-agnostic with support for multiple formats.
- **RTMP/RTSP**: Often paired with H.264 and AAC.


### **Adaptive Bitrate Streaming (ABR)**
- **Definition**: Encodes the video into multiple resolutions and bitrates.
- **Purpose**: Dynamically adjusts the stream quality based on the viewer’s internet speed.
- **Workflow**:
  1. Original video is encoded into multiple renditions (e.g., 240p, 360p, 720p).
  2. Segmented and delivered over protocols like HLS or DASH.


### **Best Practices for Encoding**
1. **Choose the Right Codec**: H.264 for compatibility, H.265 for efficiency.
2. **Optimize Bitrate and Resolution**: Match target audience bandwidth and device capabilities.
3. **Implement ABR**: Ensure smooth playback for all viewers.
4. **Use Efficient Tools**: Employ hardware encoders or cloud services for high-demand projects.
5. **Test Playback**: Verify playback quality across devices and platforms.

---

### **Optimized Encoding Pipeline for Bitrate and Resolution**
To build an encoding pipeline that optimizes bitrate and resolution, follow these steps:


### **1. Input Media Preparation**
- **Source Quality**: Start with the highest quality raw input (e.g., 4K ProRes or uncompressed).
- **Framerate Standardization**: Ensure the source has a consistent framerate (e.g., 30 fps or 60 fps).
- **Audio Quality**: Maintain high-quality audio (e.g., 48 kHz sampling rate, uncompressed WAV).


### **2. Encoding Requirements**
#### **Target Resolutions and Bitrates**
Define the renditions based on the target audience's bandwidth and device capabilities. Below is an example for an adaptive bitrate (ABR) setup:

| **Resolution** | **Bitrate (Video)** | **Bitrate (Audio)** | **Framerate** |
|-----------------|---------------------|---------------------|---------------|
| 2160p (4K)     | 12 Mbps            | 192 kbps           | 30/60 fps     |
| 1440p (2K)     | 6 Mbps             | 192 kbps           | 30 fps        |
| 1080p (Full HD) | 4 Mbps             | 128 kbps           | 30 fps        |
| 720p (HD)      | 2.5 Mbps           | 128 kbps           | 30 fps        |
| 480p (SD)      | 1 Mbps             | 96 kbps            | 30 fps        |
| 360p           | 0.6 Mbps           | 64 kbps            | 30 fps        |


### **3. Tools and Frameworks**
- **Encoder**: Use a reliable encoder like **FFmpeg**, a cloud encoder, or a hardware solution (e.g., NVIDIA NVENC).
- **Container**: Use `.mp4` for compatibility or segment it for streaming protocols like HLS/DASH.
- **Streaming Protocol**: HLS or DASH for adaptive streaming.


### **4. Encoding Steps**
#### **Step 1: Video Encoding**
Use FFmpeg or similar tools to encode multiple resolutions and bitrates.

```bash
# Example: Encode for different resolutions and bitrates
ffmpeg -i input.mp4 \
  -c:v libx264 -preset slow -crf 23 -b:v 12M -maxrate 12M -bufsize 24M -s 3840x2160 -r 30 -c:a aac -b:a 192k output_2160p.mp4 \
  -c:v libx264 -preset slow -crf 23 -b:v 6M -maxrate 6M -bufsize 12M -s 2560x1440 -r 30 -c:a aac -b:a 192k output_1440p.mp4 \
  -c:v libx264 -preset slow -crf 23 -b:v 4M -maxrate 4M -bufsize 8M -s 1920x1080 -r 30 -c:a aac -b:a 128k output_1080p.mp4 \
  -c:v libx264 -preset slow -crf 23 -b:v 2.5M -maxrate 2.5M -bufsize 5M -s 1280x720 -r 30 -c:a aac -b:a 128k output_720p.mp4 \
  -c:v libx264 -preset slow -crf 23 -b:v 1M -maxrate 1M -bufsize 2M -s 854x480 -r 30 -c:a aac -b:a 96k output_480p.mp4 \
  -c:v libx264 -preset slow -crf 23 -b:v 0.6M -maxrate 0.6M -bufsize 1.2M -s 640x360 -r 30 -c:a aac -b:a 64k output_360p.mp4
```

#### **Step 2: Audio Encoding**
- Encode audio tracks in **AAC** for efficiency and compatibility.
- Use mono or stereo settings depending on the application.

#### **Step 3: Segmenting for Streaming**
For adaptive bitrate streaming, segment videos into smaller chunks (e.g., `.ts` files for HLS) and generate manifest files (`.m3u8` for HLS or `.mpd` for DASH).

```bash
# Generate HLS segments and master playlist
ffmpeg -i input.mp4 \
  -map 0:v:0 -map 0:a:0 -b:v:4M -s 1920x1080 -f hls -hls_time 10 -hls_playlist_type vod 1080p.m3u8 \
  -map 0:v:0 -map 0:a:0 -b:v:2.5M -s 1280x720 -f hls -hls_time 10 -hls_playlist_type vod 720p.m3u8 \
  -map 0:v:0 -map 0:a:0 -b:v:1M -s 854x480 -f hls -hls_time 10 -hls_playlist_type vod 480p.m3u8
```

#### **Step 4: Master Manifest for HLS**
Create a master playlist to point to the different renditions.

```m3u8
# master.m3u8
#EXTM3U
#EXT-X-STREAM-INF:BANDWIDTH=4000000,RESOLUTION=1920x1080
1080p.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=2500000,RESOLUTION=1280x720
720p.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=1000000,RESOLUTION=854x480
480p.m3u8
```

### **5. Delivery and Playback**
#### **Content Delivery Network (CDN)**
Use a CDN (e.g., Cloudflare, Akamai, AWS CloudFront) to distribute the content efficiently.

#### **Streaming Players**
Deploy streaming players (e.g., Video.js, Shaka Player) that support HLS or DASH for adaptive playback.


### **6. Testing and Optimization**
1. **Testing**: Verify playback across devices and networks.
2. **Optimization**: Adjust bitrate, resolution, and encoding parameters based on user feedback or analytics.
3. **Monitoring**: Use tools to track QoS (Quality of Service) and QoE (Quality of Experience).

### **Benefits of this Pipeline**
- Optimized bandwidth usage.
- Seamless adaptive playback.
- High-quality video and audio tailored to user capabilities.

---

## RTMP and RTSP

### RTMP (Real-Time Messaging Protocol) and RTSP (Real-Time Streaming Protocol) are two key protocols used in video streaming projects, each serving different purposes and use cases:

### **RTMP (Real-Time Messaging Protocol)**
- **Purpose**: RTMP is a protocol developed by Adobe for delivering video, audio, and data over the internet.
It is primarily used for live streaming and was commonly associated with Adobe Flash Player.  

- **Common Use Cases**:
  - Live streaming to platforms like YouTube, Facebook, Twitch, etc.
  - Low-latency communication between the broadcaster and server.
  - Streaming between encoding software (e.g., OBS Studio) and Content Delivery Networks (CDNs).
  
- **Key Features**:
  - Works over TCP, ensuring reliable data transmission.
  - Initially optimized for Flash-based video delivery.
  - Offers various versions like RTMPS (secure RTMP) and RTMPT (RTMP tunneled over HTTP).
  - It is gradually being replaced by newer protocols like SRT (Secure Reliable Transport) and HLS (HTTP Live Streaming).


### **RTSP (Real-Time Streaming Protocol)**
- **Purpose**: RTSP is designed for controlling streaming media servers and is often used for IP cameras and surveillance systems.
- **Common Use Cases**:
  - Real-time streaming of video from IP cameras.
  - On-demand video streaming.
  - Applications that require low-latency streams.
  
- **Key Features**:
  - Acts like a "remote control" for streaming, providing features like play, pause, and stop.
  - Often used with RTP (Real-Time Transport Protocol) for data delivery and synchronization.
  - Can work over UDP (preferred for low-latency) or TCP (for reliable transmission).
  - Common in environments where live feeds need to be accessed directly, such as in CCTV systems.


### **Key Differences**
| **Aspect**        | **RTMP**                     | **RTSP**                     |
|--------------------|------------------------------|------------------------------|
| **Usage**          | Live streaming platforms     | Surveillance, low-latency feeds |
| **Transport Protocol** | TCP                          | Usually RTP over UDP or TCP   |
| **Latency**        | Higher than RTSP             | Lower for real-time feeds     |
| **Adoption**       | Legacy, declining in use     | Common in IP cameras & DVRs   |
| **Security**       | RTMPS for encryption         | Relies on other secure transport layers |

### **Conclusion**
- Use **RTMP** for broadcasting live streams to platforms and CDNs.
- Use **RTSP** for scenarios requiring low-latency access, such as surveillance systems or IP camera streaming. 

---

### **Delivery and Playback with a Content Delivery Network (CDN)**

A **Content Delivery Network (CDN)** ensures your video content is delivered efficiently, quickly, and reliably to viewers worldwide by caching and serving it from servers closest to the end user. Here’s a detailed look at how a CDN integrates into a streaming project:

### **1. Why Use a CDN for Streaming?**
1. **Global Coverage**: CDNs have Points of Presence (PoPs) worldwide, reducing latency.
2. **Scalability**: Handles spikes in traffic by distributing the load.
3. **Reduced Latency**: Delivers content from edge servers closer to users.
4. **Bandwidth Efficiency**: Offloads data from your origin server, reducing strain and costs.
5. **Reliability**: Ensures high uptime and consistent performance.
6. **Adaptive Bitrate Support**: CDNs are optimized for HLS/DASH adaptive streaming.


### **2. CDN Workflow in Streaming**
1. **Origin Server**: Hosts your encoded media files and manifest files (e.g., `.m3u8` for HLS).
2. **CDN Caching**:
   - The CDN pulls the content from the origin server when requested (origin pull).
   - Once pulled, the content is cached at edge servers for subsequent requests.
3. **Edge Servers**:
   - Distribute cached media to viewers geographically close to them.
   - Reduce the round-trip time and improve playback performance.
4. **End-User Playback**:
   - A media player (e.g., Video.js, Shaka Player) requests media segments dynamically from the nearest edge server.
   - For ABR (Adaptive Bitrate Streaming), the player switches quality levels based on network conditions.


### **3. Popular CDNs for Video Streaming**
#### **1. Cloudflare Stream/CDN**
   - **Features**: 
     - Free tier for basic CDN needs.
     - Video optimization via their Stream service.
     - Easy integration with HLS/DASH streams.
   - **Best for**: Small to medium projects.

#### **2. Akamai**
   - **Features**:
     - Large global network with ultra-low latency.
     - Tailored for large-scale video streaming events.
   - **Best for**: Enterprises and live events.

#### **3. AWS CloudFront**
   - **Features**:
     - Seamless integration with AWS S3 (origin server) and Elemental Media Services.
     - Supports dynamic content delivery and token-based authentication.
   - **Best for**: AWS ecosystem users.

#### **4. Google Cloud CDN**
   - **Features**:
     - Optimized for Google Cloud Storage as an origin.
     - Built-in support for HLS/DASH.
   - **Best for**: Projects already hosted on Google Cloud.

#### **5. Microsoft Azure CDN**
   - **Features**:
     - Integration with Azure Media Services for live and on-demand streaming.
     - Advanced analytics and monitoring.
   - **Best for**: Microsoft Azure ecosystem users.

### **4. Setting Up a CDN for Video Streaming**
#### **Step 1: Configure Your Origin Server**
- Store media files and manifests (e.g., `.mp4`, `.m3u8`, and `.ts`) on your origin server (e.g., AWS S3, your web server, or a cloud storage service).
- Ensure HTTPS is enabled for secure delivery.

#### **Step 2: Link CDN to the Origin**
- Use the CDN’s console or APIs to specify your origin server URL.
- Configure caching policies (e.g., cache media files but not manifests to allow dynamic updates).

#### **Step 3: Configure Edge Caching**
- Set rules to cache static content like video segments.
- Define cache expiration headers (`Cache-Control`).

#### **Step 4: Use a Custom Domain**
- Set up a CNAME record to link your CDN to a custom domain (e.g., `cdn.mystreamingproject.com`).

#### **Step 5: Enable Adaptive Streaming**
- Ensure the CDN supports the delivery of segment-based streams (HLS/DASH).
- Configure the media player to load the master playlist (e.g., `https://cdn.mystreamingproject.com/master.m3u8`).

#### **Step 6: Secure Your CDN**
- **Token Authentication**: Use signed URLs to restrict access to authorized viewers.
- **Geoblocking**: Limit access to specific regions.
- **HTTPS**: Encrypt all communications to ensure data security.


### **5. Delivery to End-Users**
- **Client-Side Player**: Use a streaming player (e.g., [Video.js](https://videojs.com), [Shaka Player](https://shaka-player-demo.appspot.com), [JW Player](https://www.jwplayer.com)) integrated into your app or website.
- **Player Features**:
  - **HLS/DASH Support**: Plays adaptive bitrate streams.
  - **Error Handling**: Manages buffering and network issues.
  - **Custom UI**: Provides branding and controls.

### **6. Monitoring and Optimization**
- **Monitoring**:
  - Use CDN analytics to track metrics like latency, cache hit/miss ratios, and traffic distribution.
- **Optimization**:
  - Regularly update your encoding and caching strategies.
  - Monitor playback performance using tools like Google Lighthouse or specialized streaming analytics platforms.

---

### **Detailed Use Case: Setting Up a CDN for Video Streaming**

This guide walks through setting up a **Content Delivery Network (CDN)** for video streaming using a combination of widely available tools and platforms.
We’ll focus on delivering **adaptive bitrate (ABR)** video streams using **HLS** with a CDN like **AWS CloudFront** as an example.


### **1. Prerequisites**
Before setting up the CDN, ensure you have the following:
1. **Encoded Video Files**: Multiple renditions of your video (e.g., 1080p, 720p, 480p) in `.ts` segments and a master playlist (`.m3u8`).
2. **Origin Server**: A storage bucket or web server to host the original content. Example: AWS S3.
3. **Custom Domain**: Optional but recommended for branding (e.g., `cdn.yourdomain.com`).


### **2. Steps to Set Up a CDN for Video Streaming**

#### **Step 1: Configure the Origin Server**
1. **Upload Video Content**:
   - Store your `.ts` segments and `.m3u8` files in a structured format:
     ```
     /videos/
       ├── 1080p/
       │   ├── segment1.ts
       │   ├── segment2.ts
       │   └── ...
       ├── 720p/
       └── master.m3u8
     ```
   - Ensure each resolution has its folder and segments.

2. **Set Permissions**:
   - Configure your bucket (e.g., AWS S3) to allow public read access for the files (or use signed URLs for restricted access).

3. **Enable CORS**:
   - Configure Cross-Origin Resource Sharing (CORS) in the bucket policy to allow requests from your domain:
     ```json
     {
       "AllowedOrigins": ["*"],
       "AllowedMethods": ["GET", "HEAD"],
       "AllowedHeaders": ["*"]
     }
     ```

#### **Step 2: Create a CloudFront Distribution**
1. **Log in to AWS CloudFront Console**.
2. **Create Distribution**:
   - **Origin Domain**: Point to your S3 bucket or origin server URL.
   - **Origin Path**: Specify the path to your video content, if necessary (e.g., `/videos`).
   - **Viewer Protocol Policy**: Set to `Redirect HTTP to HTTPS` for secure delivery.

3. **Set Cache Behavior**:
   - **Cache Policy**: Use a caching policy optimized for streaming.
   - **Allowed HTTP Methods**: Select `GET` and `HEAD`.
   - **TTL (Time to Live)**:
     - Short TTL for `.m3u8` files (e.g., 5 minutes) to allow updates.
     - Longer TTL for `.ts` files (e.g., 1 day) to reduce origin server load.

4. **Enable Signed URLs (Optional)**:
   - Use CloudFront signed URLs to restrict access to authorized viewers.

5. **Deploy**:
   - CloudFront assigns a distribution domain (e.g., `d123abc.cloudfront.net`).


#### **Step 3: Set Up a Custom Domain (Optional)**
1. **Create a CNAME**:
   - In your DNS provider, create a CNAME record pointing `cdn.yourdomain.com` to your CloudFront distribution domain.

2. **SSL Certificate**:
   - Use AWS Certificate Manager (ACM) to get an SSL certificate for your custom domain.
   - Attach the certificate to the CloudFront distribution.


#### **Step 4: Integrate with a Video Player**
1. **Use a Player**:
   - Embed a player like [Video.js](https://videojs.com) or [Shaka Player](https://shaka-player-demo.appspot.com) in your app or website.

2. **Embed Code**:
   ```html
   <video id="videoPlayer" class="video-js vjs-default-skin" controls autoplay>
       <source src="https://cdn.yourdomain.com/videos/master.m3u8" type="application/x-mpegURL">
   </video>
   <script src="https://cdn.jsdelivr.net/npm/video.js"></script>
   <script>
       var player = videojs('videoPlayer');
   </script>
   ```

3. **Test Playback**:
   - Verify that the player switches between renditions (e.g., 1080p, 720p) based on the network conditions.


#### **Step 5: Optimize and Monitor**
1. **Analytics**:
   - Enable CloudFront logs to monitor access patterns, errors, and cache performance.
   - Use AWS CloudWatch for detailed insights into your distribution's performance.

2. **Optimize Cache**:
   - Regularly update cache rules for optimal delivery.
   - Ensure `.m3u8` and `.ts` files are cached correctly.

3. **Scalability**:
   - CloudFront automatically scales to handle traffic spikes.

### **3. Diagram of the Workflow**
1. **Client Requests**:
   - The video player sends requests for the `master.m3u8` and `.ts` files.
2. **CDN Caches**:
   - The CDN serves cached files from edge locations closest to the user.
3. **Origin Server Fallback**:
   - If a file isn’t cached, the CDN fetches it from the origin server, caches it, and delivers it to the client.

```
[Client] ---> [Edge Server/CDN Cache] ---> [Origin Server (S3)]
```

### **4. Benefits of This Setup**
- **Low Latency**: Reduced buffering and faster load times.
- **High Scalability**: Easily handles traffic spikes.
- **Global Reach**: Optimized for worldwide delivery.
- **Secure Access**: Use HTTPS and signed URLs for restricted content.
---

### **Automating the CDN Setup for Video Streaming**

Automation simplifies repetitive tasks and ensures consistency, especially for scaling or managing large video libraries.
Let’s use **AWS CloudFront** and **AWS S3** as an example, automated through **AWS CLI** or **Infrastructure as Code (IaC)** tools like **Terraform** or **CloudFormation**.

### **Option 1: Automating with AWS CLI**

#### **1. Prerequisites**
- AWS CLI installed and configured with proper credentials and permissions.
- An existing S3 bucket with your encoded video files and manifests uploaded.

#### **2. Automate with AWS CLI**
##### **Step 1: Create an S3 Bucket**
```bash
aws s3api create-bucket --bucket my-video-bucket --region us-east-1
```

##### **Step 2: Configure S3 Bucket CORS**
```json
# Save this to cors.json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "HEAD"],
    "AllowedOrigins": ["*"],
    "ExposeHeaders": []
  }
]
```
```bash
aws s3api put-bucket-cors --bucket my-video-bucket --cors-configuration file://cors.json
```

##### **Step 3: Upload Video Files**
```bash
aws s3 cp ./videos/ s3://my-video-bucket/videos/ --recursive
```

##### **Step 4: Create a CloudFront Distribution**
```bash
aws cloudfront create-distribution \
  --origin-domain-name my-video-bucket.s3.amazonaws.com \
  --default-root-object videos/master.m3u8 \
  --query "Distribution.DomainName"
```

### **Option 2: Automating with Terraform**

#### **1. Install Terraform**
- Download and install Terraform from [https://www.terraform.io/downloads](https://www.terraform.io/downloads).

#### **2. Create Terraform Configuration**
**`main.tf`**
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "video_bucket" {
  bucket = "my-video-bucket"
  acl    = "public-read"

  cors_rule {
    allowed_headers = ["*"]
    allowed_methods = ["GET", "HEAD"]
    allowed_origins = ["*"]
  }
}

resource "aws_cloudfront_distribution" "cdn" {
  origin {
    domain_name = "${aws_s3_bucket.video_bucket.bucket}.s3.amazonaws.com"
    origin_id   = "S3-my-video-bucket"
  }

  enabled             = true
  default_root_object = "videos/master.m3u8"

  default_cache_behavior {
    allowed_methods  = ["GET", "HEAD"]
    cached_methods   = ["GET", "HEAD"]
    target_origin_id = "S3-my-video-bucket"

    forwarded_values {
      query_string = false
    }

    viewer_protocol_policy = "redirect-to-https"
  }

  viewer_certificate {
    cloudfront_default_certificate = true
  }
}
```

#### **3. Apply the Configuration**
1. Initialize Terraform:
   ```bash
   terraform init
   ```

2. Apply the configuration:
   ```bash
   terraform apply
   ```

3. Terraform will output the CloudFront domain, which you can use for playback.



### **Option 3: Automating with AWS CloudFormation**

#### **1. Create a CloudFormation Template**
**`cloudfront-s3.yml`**
```yaml
AWSTemplateFormatVersion: "2010-09-09"
Resources:
  VideoBucket:
    Type: "AWS::S3::Bucket"
    Properties:
      BucketName: "my-video-bucket"
      CorsConfiguration:
        CorsRules:
          - AllowedMethods:
              - GET
              - HEAD
            AllowedOrigins:
              - "*"
            AllowedHeaders:
              - "*"

  CloudFrontDistribution:
    Type: "AWS::CloudFront::Distribution"
    Properties:
      DistributionConfig:
        Enabled: true
        Origins:
          - DomainName: !GetAtt VideoBucket.DomainName
            Id: "S3-my-video-bucket"
        DefaultCacheBehavior:
          TargetOriginId: "S3-my-video-bucket"
          ViewerProtocolPolicy: "redirect-to-https"
          AllowedMethods:
            - GET
            - HEAD
        ViewerCertificate:
          CloudFrontDefaultCertificate: true
```

#### **2. Deploy the Template**
```bash
aws cloudformation create-stack --stack-name video-streaming-cdn --template-body file://cloudfront-s3.yml
```



### **Monitoring and Further Automation**

#### **1. Monitoring with CloudWatch**
- Configure CloudWatch alarms for metrics like `CacheHitRate`, `Requests`, and `4XXErrorRate`.

#### **2. Automate Analytics Collection**
- Use AWS Lambda to process CloudFront logs and push analytics to a dashboard.

#### **3. CI/CD for Encoding and Deployment**
- Integrate encoding pipelines with CI/CD tools like Jenkins, GitHub Actions, or AWS CodePipeline.
- Automate file uploads and CDN invalidations during updates.


### **What’s Next?**
- **Expand Security**:
  - Add signed URLs to restrict access to authorized users.
  - Implement geoblocking to limit content availability by region.
- **Test Performance**:
  - Simulate high traffic with tools like Apache JMeter or Locust.
  - Optimize cache settings based on user behavior.

---

### Detailed Setup for Advanced Automation in Streaming Projects  

This guide expands on the topics you requested for automating monitoring, deployment, and analytics in a **CDN-based streaming project**.

## **1. Monitoring with CloudWatch**

Amazon CloudWatch provides real-time insights into CDN performance, helping you monitor and improve your streaming system.

### **Steps to Set Up Monitoring**

#### **Step 1: Enable Logging for CloudFront**
1. **Log Requests**:
   - Enable access logs in your CloudFront distribution.
   - Specify an S3 bucket to store the logs:
     ```bash
     aws cloudfront update-distribution \
       --id <distribution_id> \
       --default-cache-behavior "{
         \"Logging\": {
           \"Enabled\": true,
           \"Bucket\": \"my-log-bucket.s3.amazonaws.com\",
           \"Prefix\": \"cloudfront-logs/\"
         }
       }"
     ```

2. **Enable Standard Metrics**:
   - CloudFront provides metrics like `Requests`, `BytesDownloaded`, and `CacheHitRate` by default.

#### **Step 2: Set CloudWatch Alarms**
Create alarms to notify your team of critical thresholds:
```bash
aws cloudwatch put-metric-alarm \
  --alarm-name "High4XXErrorRate" \
  --metric-name "4XXErrorRate" \
  --namespace "AWS/CloudFront" \
  --statistic "Average" \
  --period 300 \
  --threshold 10 \
  --comparison-operator "GreaterThanOrEqualToThreshold" \
  --evaluation-periods 1 \
  --dimensions Name=DistributionId,Value=<distribution_id> \
  --alarm-actions <SNS_topic_ARN>
```
- **Key Metrics to Monitor**:
  - **CacheHitRate**: Ensures CDN efficiency.
  - **Requests**: Tracks traffic spikes.
  - **4XXErrorRate & 5XXErrorRate**: Identifies access or server issues.

#### **Step 3: Visualize Metrics with Dashboards**
- Create CloudWatch Dashboards to consolidate CDN metrics:
  ```bash
  aws cloudwatch put-dashboard \
    --dashboard-name "CDNMonitoring" \
    --dashboard-body file://dashboard.json
  ```
  Example `dashboard.json`:
  ```json
  {
    "widgets": [
      {
        "type": "metric",
        "x": 0,
        "y": 0,
        "width": 12,
        "height": 6,
        "properties": {
          "metrics": [
            ["AWS/CloudFront", "Requests", "DistributionId", "<distribution_id>"],
            ["AWS/CloudFront", "4XXErrorRate", "DistributionId", "<distribution_id>"]
          ],
          "view": "timeSeries",
          "stacked": false,
          "region": "us-east-1"
        }
      }
    ]
  }
  ```

## **2. CI/CD for Encoding and Deployment**

CI/CD ensures that changes in your video library and configurations are seamlessly deployed to the CDN.

### **Steps to Automate Encoding and Deployment**

#### **Step 1: Automate Video Encoding**
1. **Use AWS MediaConvert**:
   - Create encoding jobs using AWS MediaConvert:
     ```bash
     aws mediaconvert create-job \
       --role <MediaConvertRole> \
       --settings file://job-settings.json
     ```
   - Example `job-settings.json` for HLS encoding:
     ```json
     {
       "Input": {
         "FileInput": "s3://input-bucket/videos/input.mp4"
       },
       "OutputGroups": [
         {
           "Name": "HLS Group",
           "Outputs": [
             {
               "VideoDescription": {
                 "CodecSettings": {
                   "Codec": "H.264"
                 }
               },
               "ContainerSettings": {
                 "Container": "M3U8"
               }
             }
           ]
         }
       ]
     }
     ```

2. **Trigger Encoding Automatically**:
   - Use **Amazon S3 Events** to trigger encoding when new videos are uploaded.

#### **Step 2: Deploy CDN Updates**
1. **Update CloudFront Cache**:
   - Automate cache invalidations when new content is added:
     ```bash
     aws cloudfront create-invalidation \
       --distribution-id <distribution_id> \
       --paths "/videos/*"
     ```

2. **Integrate with CI/CD Tools**:
   - Use **GitHub Actions**, **Jenkins**, or **AWS CodePipeline** to trigger encoding and deployment workflows.

#### **Step 3: CI/CD Pipeline Example (GitHub Actions)**
```yaml
name: Video Encoding and Deployment

on:
  push:
    branches:
      - main

jobs:
  encode-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Upload Video to S3
        run: aws s3 cp ./videos/ s3://input-bucket/videos/ --recursive

      - name: Trigger Encoding
        run: aws mediaconvert create-job --role <MediaConvertRole> --settings file://job-settings.json

      - name: Invalidate CloudFront Cache
        run: aws cloudfront create-invalidation --distribution-id <distribution_id> --paths "/videos/*"
```


## **3. Automate Analytics Collection**

Analytics help you understand viewer behavior and optimize your streaming project.

### **Steps to Automate Analytics**

#### **Step 1: Enable CloudFront Logs**
1. **Log Viewer Data**:
   - Configure CloudFront to send logs to an S3 bucket (as described above).
2. **Process Logs**:
   - Use AWS Lambda to process logs as they arrive in the S3 bucket.

#### **Step 2: Create Lambda for Log Processing**
1. **Install AWS SDK for Lambda**:
   - Use the AWS SDK to read and process log files.
2. **Example Lambda Function**:
   ```python
   import boto3
   import gzip
   import json

   s3 = boto3.client('s3')

   def lambda_handler(event, context):
       for record in event['Records']:
           bucket = record['s3']['bucket']['name']
           key = record['s3']['object']['key']

           # Download and process the log file
           response = s3.get_object(Bucket=bucket, Key=key)
           log_data = gzip.decompress(response['Body'].read()).decode('utf-8')

           # Parse and analyze log entries
           for line in log_data.splitlines():
               entry = json.loads(line)
               print(f"Viewer IP: {entry['c-ip']}, Status: {entry['sc-status']}")
   ```
3. **Deploy Lambda**:
   - Use an S3 event trigger to invoke the function when a new log file is added.

#### **Step 3: Aggregate and Visualize Analytics**
1. **Aggregate with AWS Athena**:
   - Query logs stored in S3 with Athena:
     ```sql
     SELECT c_ip AS ViewerIP, COUNT(*) AS Requests
     FROM cloudfront_logs
     GROUP BY c_ip
     ORDER BY Requests DESC;
     ```

2. **Visualize with Amazon QuickSight**:
   - Create dashboards to visualize metrics like viewer locations, popular content, and error rates.

---

### **Next Steps**
1. GitHub Actions workflow.
2. Monitoring dashboard?

Setting up **monitoring, CI/CD, and analytics** for your CDN-based video streaming project.


## **Approach 1: AWS Native Tools (CloudWatch, Lambda, MediaConvert)**

This approach relies on AWS tools like CloudWatch, MediaConvert, and Athena, fully integrated into AWS services.

### **1. Monitoring with CloudWatch**

#### **Step 1: Enable Logging for CloudFront**
- Log requests to an S3 bucket:
  ```bash
  aws cloudfront update-distribution \
    --id <distribution_id> \
    --default-cache-behavior '{"Logging": {"Enabled": true, "Bucket": "my-log-bucket.s3.amazonaws.com", "Prefix": "cloudfront-logs/"}}'
  ```

#### **Step 2: Create CloudWatch Alarms**
1. Monitor `CacheHitRate`, `4XXErrorRate`, and `Requests`:
   ```bash
   aws cloudwatch put-metric-alarm \
     --alarm-name "HighCacheMissRate" \
     --metric-name "CacheHitRate" \
     --namespace "AWS/CloudFront" \
     --statistic "Average" \
     --threshold 80 \
     --comparison-operator "LessThanThreshold" \
     --dimensions Name=DistributionId,Value=<distribution_id> \
     --evaluation-periods 2 \
     --alarm-actions <SNS_topic_ARN>
   ```

2. Subscribe to an SNS topic to receive notifications (email, SMS, etc.).

#### **Step 3: Create a CloudWatch Dashboard**
Combine key metrics into a single view:
```bash
aws cloudwatch put-dashboard \
  --dashboard-name "CDNPerformance" \
  --dashboard-body file://dashboard.json
```
**Example `dashboard.json`:**
```json
{
  "widgets": [
    {
      "type": "metric",
      "x": 0,
      "y": 0,
      "width": 6,
      "height": 6,
      "properties": {
        "metrics": [["AWS/CloudFront", "CacheHitRate", "DistributionId", "<distribution_id>"]],
        "view": "timeSeries",
        "stacked": false,
        "region": "us-east-1"
      }
    }
  ]
}
```

### **2. CI/CD for Encoding and Deployment**

#### **Step 1: Automate Encoding with AWS MediaConvert**
1. **Create Encoding Jobs**:
   Save the job settings:
   **`job-settings.json`**
   ```json
   {
     "Input": {
       "FileInput": "s3://input-bucket/videos/input.mp4"
     },
     "OutputGroups": [
       {
         "Name": "HLS Group",
         "Outputs": [
           {
             "ContainerSettings": {
               "Container": "M3U8"
             },
             "VideoDescription": {
               "CodecSettings": {
                 "Codec": "H.264"
               }
             }
           }
         ]
       }
     ]
   }
   ```
2. **Run Encoding Job**:
   ```bash
   aws mediaconvert create-job \
     --role <MediaConvertRole> \
     --settings file://job-settings.json
   ```

#### **Step 2: Automate CloudFront Cache Invalidation**
After encoding, invalidate the cache to refresh content:
```bash
aws cloudfront create-invalidation \
  --distribution-id <distribution_id> \
  --paths "/videos/*"
```

#### **Step 3: Automate with AWS CodePipeline**
1. Use CodePipeline to automate the sequence:
   - **Source**: S3 upload triggers pipeline.
   - **Build**: AWS Lambda runs MediaConvert encoding jobs.
   - **Deploy**: AWS CLI command invalidates CloudFront cache.



### **3. Automate Analytics Collection with Lambda and Athena**

#### **Step 1: Process Logs with AWS Lambda**
1. **Write Lambda Function**:
   **`lambda_function.py`**
   ```python
   import boto3
   import gzip

   def lambda_handler(event, context):
       s3 = boto3.client('s3')
       bucket_name = event['Records'][0]['s3']['bucket']['name']
       key = event['Records'][0]['s3']['object']['key']

       response = s3.get_object(Bucket=bucket_name, Key=key)
       log_data = gzip.decompress(response['Body'].read()).decode('utf-8')

       for line in log_data.splitlines():
           fields = line.split('\t')
           print(f"Viewer IP: {fields[4]}, Status Code: {fields[8]}")
   ```

2. **Deploy Lambda**:
   - Create an S3 trigger for the log bucket.
   - Add necessary permissions for S3 and CloudFront log processing.

#### **Step 2: Query Logs with Athena**
1. Set up an Athena table for CloudFront logs:
   ```sql
   CREATE EXTERNAL TABLE cloudfront_logs (
       date STRING,
       time STRING,
       c_ip STRING,
       sc_status INT
   ) 
   ROW FORMAT DELIMITED
   FIELDS TERMINATED BY '\t'
   STORED AS TEXTFILE
   LOCATION 's3://my-log-bucket/cloudfront-logs/';
   ```

2. Query example:
   ```sql
   SELECT c_ip, COUNT(*) AS request_count
   FROM cloudfront_logs
   GROUP BY c_ip
   ORDER BY request_count DESC;
   ```

#### **Step 3: Visualize with QuickSight**
- Integrate Athena queries with QuickSight for dashboards.

## **Approach 2: Using GitHub Actions for CI/CD**

GitHub Actions can automate encoding, deployment, and monitoring without relying solely on AWS.

### **Steps to Set Up**

#### **Step 1: Automate Encoding and Deployment**
1. **GitHub Actions Workflow**:
   **`.github/workflows/encode-deploy.yml`**
   ```yaml
   name: Encode and Deploy

   on:
     push:
       branches:
         - main

   jobs:
     encode-and-deploy:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout Code
           uses: actions/checkout@v3

         - name: Upload Video to S3
           run: aws s3 cp ./videos/ s3://input-bucket/videos/ --recursive

         - name: Trigger Encoding
           run: aws mediaconvert create-job --role <MediaConvertRole> --settings file://job-settings.json

         - name: Invalidate CloudFront Cache
           run: aws cloudfront create-invalidation --distribution-id <distribution_id> --paths "/videos/*"
   ```

#### **Step 2: Monitor with Logs**
Integrate **CloudFront log processing** using a GitHub-hosted runner or deploy to Lambda for automation.




