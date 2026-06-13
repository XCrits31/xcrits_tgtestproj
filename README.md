# Medical Image Augmentation Tool

A full-stack web application designed to simplify and speed up dataset preparation for deep learning models in medical imaging (Ultrasound, MRI, X-ray). 

Instead of writing custom Python scripts for every batch, users can build augmentation pipelines through a web interface and export data.

## Key Features

* **Pipeline Builder:** Combine multiple transformations like rotation, contrast adjustments, zoom, noise insertion, and elastic deformation.
* **Medical AI Integration:** Uses MONAI and PyTorch under the hood for professional-grade medical image processing.
* **Flexible Export:** Download processed datasets as standard PNGs or ready-to-train PyTorch tensor (`.pt`) files, individually or in bulk (ZIP).
* **History & Reuse:** All pipelines and transformation histories are saved in MySQL for future use.

## Architecture & Tech Stack

The project uses a decoupled architecture where Laravel handles the web interface, queues, and metadata, while a Python service processes the heavy image transformations.

* **Backend:** Laravel 11 (PHP 8.2), Artisan Jobs (Queues), REST API
* **AI & Processing:** Python 3.10, MONAI, PyTorch
* **Real-time Updates:** Pusher Channels, Laravel Echo
* **Frontend:** Blade, Bootstrap 5, Vanilla JavaScript
* **Database:** MySQL 8 + Eloquent ORM
* **DevOps:** Nginx, Crontab

## How It Works (Under the Hood)

1. **Upload:** User uploads medical images.
2. **Pipeline Setup:** User adjusts sliders for transformations.
3. **Queue Processing:** When the user clicks "Regenerate", Laravel dispatches an **Artisan Job** to handle the bulk generation asynchronously, offloading the main thread.
4. **Python Execution:** The background job triggers the Python script utilizing MONAI/PyTorch to perform heavy mathematical matrix deformations.
5. **Real-time Notification:** Once processing is done, Pusher sends an event to the frontend, and the user gets link for downloading and preview changes in all imported images.
