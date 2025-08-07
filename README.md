# Flask Machine Learning Azure App

A Flask-based machine learning web service that predicts Boston housing prices using scikit-learn. The application is designed for deployment on Azure Web Apps with continuous integration and deployment through Azure Pipelines.

## Overview

This application provides a REST API endpoint that accepts housing characteristics and returns predicted housing prices using a pre-trained machine learning model. The model uses the Boston Housing dataset features to make predictions.

## Features

- **REST API**: Simple JSON-based prediction endpoint
- **Machine Learning**: Uses scikit-learn for housing price predictions
- **Azure Integration**: Ready for Azure Web App deployment
- **CI/CD Pipeline**: Automated testing, linting, and deployment with Azure Pipelines
- **Data Preprocessing**: Automatic feature scaling using StandardScaler
- **Logging**: Comprehensive logging for monitoring and debugging

## Project Structure

```
.
├── app.py                      # Main Flask application
├── azure-pipelines.yml        # Azure DevOps CI/CD pipeline configuration
├── requirements.txt            # Python dependencies
├── Makefile                   # Build automation commands
├── make_predict.sh            # Local testing script
├── make_predict_azure_app.sh  # Azure deployment testing script
├── boston_housing_prediction.joblib  # Pre-trained ML model (not included)
└── README.md                  # This file
```

## Requirements

- Python 3.7+
- Flask 2.2.3
- pandas 1.1.5
- scikit-learn 0.22.2
- pylint 2.6.2

## Installation

### Local Development Setup

1. **Clone the repository**
   ```bash
   git clone <your-repository-url>
   cd <repository-name>
   ```

2. **Create virtual environment**
   ```bash
   make setup
   source ~/.flask-ml-azure/bin/activate
   ```

3. **Install dependencies**
   ```bash
   make install
   ```

4. **Run linting (optional)**
   ```bash
   make lint
   ```

### Manual Setup

If you prefer manual setup:

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install --upgrade pip
pip install -r requirements.txt
```

## Usage

### Running Locally

1. **Start the Flask application**
   ```bash
   python app.py
   ```
   
   The application will run on `http://localhost:5000`

2. **Test the prediction endpoint**
   ```bash
   chmod +x make_predict.sh
   ./make_predict.sh
   ```

### API Endpoints

#### Home Endpoint
- **URL**: `/`
- **Method**: GET
- **Description**: Returns a simple HTML page confirming the service is running

#### Prediction Endpoint
- **URL**: `/predict`
- **Method**: POST
- **Content-Type**: `application/json`

**Request Format**:
```json
{
  "CHAS": {"0": 0},
  "RM": {"0": 6.575},
  "TAX": {"0": 296.0},
  "PTRATIO": {"0": 15.3},
  "B": {"0": 396.9},
  "LSTAT": {"0": 4.98}
}
```

**Response Format**:
```json
{
  "prediction": [20.35373177134412]
}
```

### Feature Descriptions

The model expects the following Boston Housing dataset features:

- **CHAS**: Charles River dummy variable (1 if tract bounds river; 0 otherwise)
- **RM**: Average number of rooms per dwelling
- **TAX**: Full-value property-tax rate per $10,000
- **PTRATIO**: Pupil-teacher ratio by town
- **B**: Proportion of blacks by town
- **LSTAT**: Percentage of lower status of the population

## Azure Deployment

### Prerequisites

1. Azure subscription
2. Azure DevOps account
3. Azure Web App service created
4. Service connection configured in Azure DevOps

### Deployment Steps

1. **Update the Azure Pipeline configuration**
   - Modify `azure-pipelines.yml` with your specific values:
   - Update `azureServiceConnectionId`
   - Update `webAppName`

2. **Push to master branch**
   The Azure Pipeline will automatically:
   - Build the application
   - Run linting tests
   - Deploy to Azure Web App

3. **Test the deployed application**
   ```bash
   # Update the script with your Azure app name
   chmod +x make_predict_azure_app.sh
   ./make_predict_azure_app.sh
   ```

### Manual Azure Deployment

If deploying manually to Azure Web App:

1. Create a ZIP file of your project
2. Use Azure CLI or Azure Portal to deploy
3. Ensure the pre-trained model file `boston_housing_prediction.joblib` is included

## Model Requirements

This application requires a pre-trained scikit-learn model saved as `boston_housing_prediction.joblib`. The model should be trained on the Boston Housing dataset and support the features listed above.

To create the model file:

```python
from sklearn.ensemble import RandomForestRegressor
from sklearn.externals import joblib
from sklearn.datasets import load_boston

# Load and train your model
boston = load_boston()
model = RandomForestRegressor()
model.fit(X_train, y_train)

# Save the model
joblib.dump(model, 'boston_housing_prediction.joblib')
```

## Development

### Code Quality

The project uses pylint for code quality checking:

```bash
make lint
```

### Testing

To run tests (when implemented):

```bash
make test
```

### Build All

To run the complete build process:

```bash
make all
```

## Logging

The application includes comprehensive logging:
- Request payload logging
- Model loading status
- Prediction processing steps
- Error handling and reporting

Logs are available through the Flask logger and can be viewed in Azure App Service logs.

## Troubleshooting

### Common Issues

1. **Model not found error**
   - Ensure `boston_housing_prediction.joblib` is in the project root
   - Verify the model was trained with compatible scikit-learn version

2. **Import errors**
   - Check Python version compatibility (3.7+)
   - Ensure all requirements are installed: `pip install -r requirements.txt`

3. **Azure deployment issues**
   - Verify Azure service connection configuration
   - Check Azure Web App configuration and logs
   - Ensure the app name in deployment scripts matches your Azure Web App

### Error Handling

The application includes basic error handling:
- Model loading failures return "Model not loaded" message
- Invalid JSON payloads are logged
- Prediction errors are caught and logged

## Support

For issues and questions:
- Check the application logs
- Review Azure Web App diagnostics
- Ensure model file is properly deployed

---

**Note**: Remember to update the `make_predict_azure_app.sh` script with your actual Azure Web App name before testing the deployed application.