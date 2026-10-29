# ROS 1 Goal Navigation: Jupyter Interface

This branch replaces the terminal action client of the [ROS 1 Goal Navigation](../../tree/main) package with a **Jupyter notebook** that offers a graphical interface and live plots.

> Second assignment of the Research Track 2 course (1st year, 2nd semester), MSc Robotics Engineering, University of Genoa.

## Content

The notebook `scripts/action_client_jy.ipynb` connects to the same `reaching_goal` action server as the original node, and adds:

- **GUI** (`ipywidgets`): input fields for the target (x, y) and buttons to send a goal, cancel it and read the latest feedback.
- **Live status:** goal status, robot position, and distance from the closest obstacle computed from the laser scan.
- **Live plots** (`matplotlib` animation, updated every second): the robot trajectory, and a bar chart of the goals reached and failed.

The launch file starts only the simulation and the service node, because the action client now runs in the notebook.

## Usage

```bash
roslaunch ass2_ros1 ass2.launch
jupyter notebook scripts/action_client_jy.ipynb
```

Then run all the cells of the notebook. See the [main README](../../tree/main#readme) for the installation.
