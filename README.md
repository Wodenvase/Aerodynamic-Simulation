ANN Surrogate for Aerodynamic Simulation
Overview
This project demonstrates the use of a deep neural network (specifically a U-Net architecture) to act as a surrogate model for traditional Computational Fluid Dynamics (CFD) simulations.

The Problem: High-fidelity CFD simulations of airflow over objects like airfoils are computationally expensive, often taking hours or days to complete. This makes them impractical for real-time design optimization and analysis.

The AI Solution: We train a U-Net model to learn the complex relationship between an airfoil's shape and the resulting aerodynamic flow field. Once trained, the model can predict the pressure and velocity fields around a new airfoil almost instantly, providing a massive speedup over traditional solvers.

Key Features
Instantaneous Prediction: Reduces simulation time from minutes/hours to milliseconds.

Physics-Informed Learning: The model learns the underlying principles of fluid dynamics from a large dataset of simulation examples.

Image-to-Image Translation: Leverages a U-Net, a powerful architecture for tasks that map an input image (the airfoil shape) to an output image (the flow field).

Scalable Data Generation: Includes a fully self-contained Python script to generate thousands of unique training samples.

⚙️ How It Works
The project is broken down into three main phases:

1. Data Generation
A synthetic dataset is created using a Python-based panel method solver. This fast, simplified aerodynamic solver generates hundreds of simulations by systematically varying two key parameters:

Airfoil Shape: Using different NACA 4-digit airfoil profiles (e.g., NACA 2412, NACA 4415).

Angle of Attack: Simulating the airfoil at various angles relative to the incoming airflow.

Each data sample consists of an input image (a 128x128 binary mask of the airfoil) and three corresponding output images (128x128 fields for pressure coefficient, x-velocity, and y-velocity).

2. Model Architecture: U-Net
The U-Net architecture is ideal for this task. Its "encoder-decoder" structure with skip connections allows it to:

Encode the input shape into a compressed feature representation.

Decode these features back into the full-resolution flow field images.

Preserve fine details (like the airfoil's sharp edges) via skip connections, leading to more accurate predictions.

3. Training & Evaluation
The generated data is split into train.npz, validation.npz, and test.npz files (70%, 15%, 15% split). The model is then trained using the Mean Squared Error loss function to minimize the difference between its predicted flow fields and the ground truth data from the simulations.

📁 Project Structure
.
├── airfoil_data/
│   ├── train.npz
│   ├── validation.npz
│   └── test.npz
│
├── data_gen.ipynb       # Standalone script to generate the dataset.
├── train_and_evaluate.ipynb # Jupyter Notebook to build, train, and evaluate the U-Net.
└── README.md               # This file.

🚀 How to Run This Project
Prerequisites
Make sure you have Python 3 installed, then install the required libraries:

pip install numpy tensorflow scikit-image scikit-learn tqdm matplotlib

Step 1: Generate the Dataset
Run the data generation script from your terminal. This will create the airfoil_data directory and populate it with the necessary .npz files.

python create_dataset.py

(This may take several minutes to complete as it runs all the simulations.)

Step 2: Train the Model
Open and run the train_and_evaluate.ipynb Jupyter Notebook. The notebook will:

Load the pre-split train, validation, and test datasets.

Build the U-Net model architecture.

Train the model and save the final weights to airfoil_surrogate_model.h5.

Display plots of the training and validation loss curves.

Step 3: Visualize the Results
The final step is to use the trained model to make a prediction on a sample from the test set and visually compare it to the ground truth.

An example output showing the ground truth pressure field (left) vs. the U-Net's instantaneous prediction (right).

🛠️ Technologies Used
Python

NumPy

TensorFlow (with Keras API)

Matplotlib

Scikit-learn

Scikit-image

Tqdm
