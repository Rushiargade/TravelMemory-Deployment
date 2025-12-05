# 📸 Deployment Screenshots

This folder contains screenshots demonstrating the Travel Memory MERN application deployment process on AWS EC2.

## Screenshot Guide

### 1. **01-pm2-status.png**
- Shows PM2 status output
- Displays backend service running on port 3000
- Shows process ID, uptime, and memory usage
- Command: `pm2 status`

### 2. **02-app-running.png**
- Screenshot of the live application running
- URL: https://rushi.online
- Shows Travel Memory UI with sample data
- Demonstrates frontend is successfully deployed

### 3. **03-nginx-logs.png**
- Nginx access logs showing incoming requests
- Displays reverse proxy traffic
- Shows successful proxy passes to backend
- Path: `/var/log/nginx/access.log`

### 4. **04-curl-request.png**
- cURL request to backend API endpoint
- Command: `curl -i http://127.0.0.1:3000/trip`
- Shows successful API response with 200 status
- Demonstrates backend connectivity

### 5. **05-live-app.png**
- Browser screenshot of the deployed application
- Shows the Travel Memory application interface
- Displays working features and navigation
- Proves end-to-end deployment success

### 6. **06-performance.png**
- AWS CloudWatch metrics or ALB health check status
- Shows load balancer distribution
- Displays instance health
- Demonstrates scaling and availability

## How to Add Screenshots

### From Your Word Document:
1. Extract the 6 images from your Word document (image1.png - image6.png)
2. Rename them according to the naming convention above
3. Upload to this folder
4. Verify they appear in the GitHub repository

### Using GitHub Web Interface:
1. Click "Add file" → "Upload files"
2. Drag and drop your screenshots
3. Commit changes with a message

### Using Git Command Line:
```bash
git add docs/screenshots/*.png
git commit -m "docs: Add deployment screenshots"
git push origin main
```

## Screenshot Naming Convention

```
01-pm2-status.png          # PM2 process manager status
02-app-running.png        # Application running screenshot
03-nginx-logs.png         # Nginx server logs
04-curl-request.png       # API endpoint testing
05-live-app.png           # Live application in browser
06-performance.png        # Performance metrics or ALB health
```

## Important Notes

- ✅ Screenshots provide visual proof of successful deployment
- ✅ Include all 6 screenshots from your assignment Word document
- ✅ Use consistent naming for easy reference
- ✅ Ensure screenshots show key deployment milestones
- ✅ Required for assignment submission on V-Learn

## Quick Links

- 📖 [Main README Documentation](../../README.md)
- 🌐 [Live Application](https://rushi.online)
- 📊 [GitHub Repository](https://github.com/Rushiargade/TravelMemory-Deployment)

---

**Last Updated:** December 5, 2025
**Purpose:** Assignment Submission Documentation
**Course:** HeroVired DevOps & Python Program
