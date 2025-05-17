Image Classification App
This application provides image classification for two domains:

Sports - Identifies sports equipment and activities
Vegetables & Fruits - Identifies various vegetables and fruits

Features

Upload images for real-time classification
View top 5 most likely classifications with confidence scores
See prediction time for performance metrics
Responsive Gradio interface with tabbed navigation

Technical Details

Built with Gradio, PyTorch, and TorchVision
Loads optimized CPU models from S3
Deployed on Hugging Face Spaces

How to Use

Select the appropriate tab for your image type (Sports or VegFruits)
Upload an image using the interface
Click the "Classify" button
View the results showing the top classifications and confidence scores

Development
This app was developed as part of an MLOps project demonstrating:

Model deployment automation
Integration with cloud storage (AWS S3)
Containerization for inference
CI/CD through GitHub Actions
