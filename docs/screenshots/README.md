# 📸 Deployment Screenshots

This folder contains screenshots demonstrating the Travel Memory MERN application deployment process on AWS EC2.

## Screenshot Guide

### 1. **01-pm2-status.png**
- Shows PM2 status output
- Displays backend service running on port 3000
- Shows process ID, uptime, and memory usage
- Command: `pm2 status`
- <img width="940" height="164" alt="image" src="https://github.com/user-attachments/assets/4d4a35b9-4521-4a3e-99c5-191d0c28b231" />

### 2. **02-app-running.png**
- Screenshot of the live application running
- URL: https://rushi.online
- Shows Travel Memory UI with sample data
- Demonstrates frontend is successfully deployed
- <img width="940" height="338" alt="image" src="https://github.com/user-attachments/assets/3372f47f-ac6f-4f5b-aca5-0aa6935433ec" />



### 3. **03-nginx-logs.png**
- Nginx access logs showing incoming requests
- Displays reverse proxy traffic
- Shows successful proxy passes to backend
- Path: `/var/log/nginx/access.log`
- <img width="940" height="538" alt="image" src="https://github.com/user-attachments/assets/3c9b3d55-2cb8-42f2-9467-d05964cb1873" />


### 4. **04-curl-request.png**
- cURL request to backend API endpoint
- Command: `curl -i http://127.0.0.1:3000/trip`
- Shows successful API response with 200 status
- Demonstrates backend connectivity
- <img width="940" height="414" alt="image" src="https://github.com/user-attachments/assets/e0be0181-aa24-46f9-b7f6-b3f7731eff89" />


### 5. **05-live-app.png**
- Browser screenshot of the deployed application
- Shows the Travel Memory application interface
- Displays working features and navigation
- Proves end-to-end deployment success
- <img width="940" height="538" alt="image" src="https://github.com/user-attachments/assets/8252b9b2-814d-4044-bd73-eb1d5a442930" />





```
01-pm2-status.png          # PM2 process manager status
02-app-running.png        # Application running screenshot
03-nginx-logs.png         # Nginx server logs
04-curl-request.png       # API endpoint testing
```

## Quick Links

- 📖 [Main README Documentation](../../README.md)
- 🌐 [Live Application](https://rushi.online)
- 📊 [GitHub Repository](https://github.com/Rushiargade/TravelMemory-Deployment)

---

**Last Updated:** December 5, 2025
**Purpose:** Assignment Submission Documentation
**Course:** HeroVired DevOps & Python Program
