# Vue Project

## Overview
This is a Vue.js project configured with Tailwind CSS and Vite. The project includes a charting component for visualizing data for student score tracking.

## Project Structure
### Root Directory
- index.html: The main HTML template for the Vue application.
- main.js: The entry point of the application. It initializes the Vue app and mounts it to the DOM.
- App.vue: The root Vue component that serves as the main container for the app.
- tailwind.config.js: Configuration file for Tailwind CSS, used to customize styles and themes.
- vite.config.js: Configuration file for Vite, a fast build tool for Vue projects.
- package.json: Lists dependencies and scripts for the project.
- jsconfig.json: Configuration for JavaScript language support and module paths.

### src/ Directory
The src folder contains the main application logic.

- **components/**: Contains reusable Vue components.
  - **BarChart.vue**: A charting component built with Chart.js to render bar charts based on data props.

### Additional Details
- The project uses *Tailwind CSS* for styling and *Chart.js* for data visualization.
- *Vite* is used for development and build processes.
