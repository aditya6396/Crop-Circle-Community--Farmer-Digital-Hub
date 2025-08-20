# Crop-Circle-Community--Farmer-Digital-Hub
## Overview
This is a Flask-based web application designed to assist farmers with agricultural tasks, including crop recommendation, fertilizer suggestion, and disease detection for rice and sugarcane crops. The application leverages machine learning models for crop disease classification and crop recommendation, utilizing weather data from an external API and image processing for disease detection.

## Features
- **Crop Recommendation**: Recommends suitable crops based on soil nutrients (N, P, K), pH, rainfall, temperature, and humidity.
- **Fertilizer Suggestion**: Provides fertilizer recommendations based on crop type and soil nutrient levels.
- **Disease Detection**: Identifies diseases in rice and sugarcane crops using pre-trained machine learning models and image processing.
- **Mobile App Support**: Includes API endpoints for mobile applications to predict diseases in rice and sugarcane crops.
- **Weather Integration**: Fetches real-time temperature and humidity data using the OpenWeatherMap API.

## Prerequisites
- Docker and Docker Compose
- Python 3.9.13
- Access to the internet for fetching weather data
- A valid API key for OpenWeatherMap (configured in `config.py`)

## Installation

1. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   cd Agriculture
   ```

2. **Set Up Environment**:
   Ensure the `requirements.txt` file is present with the following dependencies:
   - Flask==2.0.2
   - markupsafe==2.0.1
   - numpy==1.21.2
   - pandas==1.3.3
   - requests==2.26.0
   - Pillow==8.3.2
   - pyyaml==5.1
   - opencv-python
   - matplotlib
   - scikit-learn==1.2.2
   - torch==1.8.0+cpu
   - torchvision==0.9.0+cpu
   - detectron2

3. **Configure Weather API Key**:
   Update the `config.py` file with your OpenWeatherMap API key:
   ```python
   weather_api_key = "your-api-key-here"
   ```

4. **Build and Run with Docker**:
   Ensure the `Dockerfile` and `docker-compose.yml` are configured as provided. Run the following command:
   ```bash
   docker-compose up --build
   ```
   The application will be accessible at `http://localhost:9000`.

## Project Structure
- **`app.py`**: Main Flask application file containing routes for home, crop recommendation, fertilizer suggestion, and disease prediction.
- **`Dockerfile`**: Defines the Docker image for the application, installing Python dependencies and setting up the environment.
- **`docker-compose.yml`**: Configures the Docker service to run the Flask app on port 9000 and connects to an external network (`medical_network`).
- **`config.py`**: Stores the OpenWeatherMap API key.
- **`requirements.txt`**: Lists all Python dependencies.
- **`utils/`**: Contains helper modules for disease and fertilizer dictionaries, and the ResNet9 model for disease classification.
- **`models/`**: Stores pre-trained models (`plant_disease_model.pth` for disease classification and `RandomForest.pkl` for crop recommendation).
- **`static/`**: Contains static files, including uploaded images (`static/upload/`), processed output images (`static/output/`), and data images (`static/data/`).
- **`templates/`**: HTML templates for rendering web pages (`index.html`, `crop.html`, `fertilizer.html`, `rice.html`, `sugarcane.html`, etc.).

## Usage

1. **Access the Application**:
   Open a browser and navigate to `http://localhost:9000`.

2. **Crop Recommendation**:
   - Navigate to `/crop-recommend`.
   - Enter soil parameters (N, P, K, pH, rainfall) and city name to fetch weather data.
   - Submit to get a recommended crop.

3. **Fertilizer Suggestion**:
   - Navigate to `/fertilizer`.
   - Provide crop name and soil nutrient levels (N, P, K).
   - Submit to receive fertilizer recommendations.

4. **Disease Detection**:
   - Navigate to `/disease-predict` for general plant disease detection, `/rice` for rice-specific diseases, or `/sugar` for sugarcane-specific diseases.
   - Upload an image of the plant leaf.
   - Receive disease predictions and visualizations.

5. **Mobile App API**:
   - Use `/predict_rice_app_new` for rice disease prediction via a JSON POST request with a base64-encoded image.
   - Use `/predict_sc_app` for sugarcane disease prediction via a JSON POST request with a base64-encoded image.
   - Response includes a base64-encoded output image and predicted disease classes.

## Models
- **Crop Recommendation Model**: A Random Forest model (`RandomForest.pkl`) predicts suitable crops based on environmental and soil data.
- **Disease Classification Model**: A ResNet9 model (`plant_disease_model.pth`) identifies 38 plant disease classes across various crops.
- **Rice Disease Detection**: Uses Detectron2 with a Mask R-CNN model (`model_final.pth`) to detect diseases like brownspot, leafblast, and hispa.
- **Sugarcane Disease Detection**: Uses Detectron2 with a Mask R-CNN model (`Sugarcane_model_final.pth`) to detect diseases like blight and rot.

## API Endpoints
- **GET `/`**: Renders the home page.
- **GET `/crop-recommend`**: Renders the crop recommendation form.
- **POST `/crop-predict`**: Processes crop recommendation inputs and returns predictions.
- **GET `/fertilizer`**: Renders the fertilizer recommendation form.
- **POST `/fertilizer-predict`**: Processes fertilizer inputs and returns recommendations.
- **GET `/rice`**: Renders the rice disease prediction form.
- **GET `/sugar`**: Renders the sugarcane disease prediction form.
- **POST `/disease-predict`**: Processes plant disease image uploads and returns predictions.
- **POST `/predict`**: Processes rice disease image uploads and returns predictions with visualizations.
- **POST `/predict_sc`**: Processes sugarcane disease image uploads and returns predictions with visualizations.
- **POST `/predict_rice_app_new`**: API endpoint for rice disease prediction (mobile app).
- **POST `/predict_sc_app`**: API endpoint for sugarcane disease prediction (mobile app).
- **GET `/meratest`**: Test endpoint to verify application functionality.
