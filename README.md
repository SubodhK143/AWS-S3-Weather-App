# 🌦️ Secure Weather App Deployment on AWS

A hands-on AWS project demonstrating how to deploy a **static Weather WebApp** using **Amazon S3** and secure/publicly distribute it through **Amazon CloudFront** with HTTPS.

> **Project by Subodh Kumar**  
> AWS Cloud Support Engineer | AWS Certified  
> [LinkedIn](https://www.linkedin.com/in/subodh-kumar-aws-certified/) · [Weather App Source Code](https://github.com/SubodhK143/Weather_App)

---

## 📌 Project Overview

This project focuses on deploying an existing static Weather WebApp to AWS and making it accessible over the internet.

### Objectives

- Host a static website using **Amazon S3**
- Configure **S3 Static Website Hosting**
- Upload the Weather App files to S3
- Configure the required S3 bucket policy
- Distribute the application through **Amazon CloudFront**
- Enable secure **HTTPS** access through CloudFront
- Test the Weather App with different locations

The project documentation describes the deployment architecture and implementation steps in detail. fileciteturn0file0L2-L17

---

## 🏗️ Architecture

```text
                     ┌─────────────────┐
                     │      Users      │
                     └────────┬────────┘
                              │
                              │ HTTPS
                              ▼
                  ┌──────────────────────┐
                  │   Amazon CloudFront  │
                  │  CDN / HTTPS Access  │
                  └──────────┬───────────┘
                             │
                             │ Origin Request
                             ▼
                  ┌──────────────────────┐
                  │      Amazon S3       │
                  │   Static Web Hosting │
                  └──────────┬───────────┘
                             │
                             ▼
                  ┌──────────────────────┐
                  │   Weather App Files  │
                  │ HTML / CSS / JS      │
                  └──────────────────────┘
                             │
                             ▼
                    OpenWeather API
```

The architecture in the project documentation shows users accessing the application through CloudFront, with S3 serving as the website origin. fileciteturn0file0L20-L30

---

## 🛠️ Technologies Used

### AWS Services

- ☁️ **Amazon S3**
- 🚀 **Amazon CloudFront**

### Web Technologies

- HTML
- CSS
- JavaScript
- OpenWeather API

These technologies are listed in the project documentation. fileciteturn0file0L9-L17

---

## 📂 Source Code

The Weather App source code used for this deployment is available here:

👉 **[Weather App – GitHub](https://github.com/SubodhK143/Weather_App)**

The project documentation identifies this repository as the source code for the Weather WebApp. fileciteturn0file0L25-L30

---

# 🚀 Deployment Steps

## 1. Create an IAM User

For the deployment, the project documentation specifies an IAM user with permissions for:

- `AmazonS3FullAccess`
- `CloudFrontFullAccess`

Use an IAM user rather than routinely performing deployment tasks with the root account. fileciteturn0file0L33-L40

> **Production note:** For a real production environment, prefer least-privilege IAM policies instead of broad `FullAccess` permissions.

---

## 2. Create an S3 Bucket

Go to:

**AWS Console → S3 → Create bucket**

Configure:

- A globally unique bucket name
- AWS Region
- Required public-access settings for this demonstration

The original project used a bucket named `weather-webapp`. fileciteturn0file0L39-L46

---

## 3. Upload the Weather App

After creating the bucket:

**S3 → Bucket → Upload**

Upload the application files, including:

```text
index.html
CSS files
JavaScript files
Images
Other required static assets
```

The project documentation shows the Weather App files being uploaded to the S3 bucket and retaining Standard storage settings. fileciteturn0file0L47-L59

---

## 4. Enable S3 Static Website Hosting

Navigate to:

**S3 → Bucket → Properties → Static website hosting**

Enable static website hosting and configure:

```text
Index document: index.html
```

After enabling hosting, AWS provides a static website endpoint that can be used to test the application. fileciteturn0file0L60-L65

---

## 5. Configure the S3 Bucket Policy

Initially, the project received an **Access Denied** response because the required bucket policy had not been configured.

Go to:

**S3 → Bucket → Permissions → Bucket Policy**

The policy used in the project was:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Statement1",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::weather-webapp/*"
    }
  ]
}
```

This grants public read access to objects in the specified bucket. fileciteturn0file0L67-L83

> ⚠️ **Security note:** Public S3 object access should be used carefully. For production architectures, a private S3 bucket behind CloudFront is generally preferable.

---

## 6. Test the S3 Website

After saving the bucket policy, refresh the S3 website endpoint.

The project documentation shows the Weather App successfully loading through S3, but the direct S3 website connection remained insecure. fileciteturn0file0L85-L90

---

# 🔐 7. Configure Amazon CloudFront

To provide HTTPS access, create a CloudFront distribution.

Navigate to:

**AWS Console → CloudFront → Create Distribution**

### Origin

Select the S3 website/origin used by the project.

### Viewer Protocol

Configure the distribution to support HTTP/HTTPS access as required.

### Default Root Object

Set:

```text
index.html
```

The project documentation uses `index.html` as the default root object and creates a CloudFront distribution for the Weather App. fileciteturn0file0L91-L103

---

## 8. Access the Application Using CloudFront

After the CloudFront distribution becomes available:

1. Copy the CloudFront distribution URL.
2. Open the URL in a browser.
3. Verify that the Weather App loads.
4. Confirm that the browser uses an HTTPS connection.

The project documentation shows the application being successfully accessed through the CloudFront URL using HTTPS. fileciteturn0file0L105-L112

---

# 🧪 Testing

The Weather App was tested with different locations.

Example testing included:

```text
Mumbai
Adelaide
Antarctica
```

The screenshots in the project documentation demonstrate successful weather results for supported locations as well as a location-not-found response. fileciteturn0file0L113-L116

### Expected Flow

```text
Enter Location
      ↓
JavaScript sends request
      ↓
OpenWeather API
      ↓
Weather Data
      ↓
Weather information displayed
```

---

# 🔒 Security & HTTPS

The project demonstrates two stages:

### Stage 1 — S3 Website

```text
User → S3 Static Website
```

The direct S3 website was successfully deployed but remained insecure over HTTP. fileciteturn0file0L85-L90

### Stage 2 — CloudFront

```text
User
  ↓
HTTPS
  ↓
CloudFront
  ↓
S3
```

CloudFront provided the secure HTTPS endpoint used to access the application. fileciteturn0file0L105-L112

---

# 📊 Key AWS Concepts Demonstrated

| Concept | Implementation |
|---|---|
| Static Website Hosting | Amazon S3 |
| Object Storage | Amazon S3 |
| CDN | Amazon CloudFront |
| HTTPS Access | CloudFront |
| Website Entry Point | `index.html` |
| Access Control | S3 Bucket Policy |
| IAM | IAM User & Permissions |
| External API | OpenWeather API |
| Application Testing | Multiple weather locations |

---

# ⚠️ Project Limitation

The CloudFront distribution uses an AWS-provided CloudFront domain because the project did not use a custom domain.

The documentation notes that **Amazon Route 53** can be used with a custom domain and **AWS Certificate Manager (ACM)** can be used for SSL/TLS certificate management. fileciteturn0file0L118-L127

### Possible Production Enhancement

```text
User
  ↓
Custom Domain
  ↓
Route 53
  ↓
CloudFront
  ↓
S3
```

Potential improvements:

- Amazon Route 53 for DNS
- AWS Certificate Manager (ACM) for TLS certificates
- Private S3 bucket
- CloudFront Origin Access Control (OAC)
- Least-privilege IAM policies
- CloudFront caching policies
- CI/CD deployment pipeline

---

# 💼 Interview Explanation

### How to explain this project in an interview

> "I deployed a static Weather Application on AWS using Amazon S3 and Amazon CloudFront. First, I created an S3 bucket and uploaded the HTML, CSS, JavaScript, and application assets. I enabled S3 static website hosting and configured the required bucket permissions. The direct S3 endpoint worked, but it was not providing the secure HTTPS experience I wanted. I then created a CloudFront distribution in front of the S3-hosted application and configured `index.html` as the default root object. Finally, I accessed the application through the CloudFront URL over HTTPS and tested the Weather App with multiple locations."

---

# 📁 Suggested Repository Structure

```text
.
├── README.md
├── index.html
├── css/
│   └── style.css
├── js/
│   └── script.js
├── images/
│   ├── ...
│   └── ...
└── screenshots/
    └── ...
```

---

# 🎯 Learning Outcomes

Through this project, I practiced:

- Amazon S3 static website hosting
- S3 bucket configuration
- S3 object upload
- S3 bucket policies
- IAM permissions
- Amazon CloudFront distribution creation
- HTTPS-based application delivery
- Static website deployment
- Basic AWS architecture design
- Testing and troubleshooting access issues

---

## 👨‍💻 Author

**Subodh Kumar**

AWS Cloud Support Engineer | AWS Certified

- 🔗 **LinkedIn:** [Subodh Kumar](https://www.linkedin.com/in/subodh-kumar-aws-certified/)
- 💻 **Weather App Source:** [GitHub Repository](https://github.com/SubodhK143/Weather_App)

---

## 📚 Project Documentation

The complete step-by-step implementation, screenshots, architecture diagram, testing evidence, and limitations are documented in the accompanying project documentation. 

---

⭐ If you find this project useful, feel free to explore the repository and connect with me on LinkedIn.
