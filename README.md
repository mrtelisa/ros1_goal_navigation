# ROS 1 Goal Navigation: Sphinx Documentation

This branch adds the code documentation of the [ROS 1 Goal Navigation](../../tree/main) package, generated with **Sphinx**.

> First assignment of the Research Track 2 course (1st year, 2nd semester), MSc Robotics Engineering, University of Genoa.

## Content

- **Docstrings** in reStructuredText format for every module and function of `action_client.py` and `service.py`: description, parameters, return values.
- **Sphinx configuration** in `source/` (`conf.py`, `index.rst`, one page per node), with the Read the Docs theme.
- **Generated HTML documentation** in `docs/`, ready to be published with GitHub Pages.

The code and its behaviour are the same as in `main`: see the [main README](../../tree/main#readme) for the nodes, installation and usage.

## Build the documentation

```bash
pip install sphinx sphinx-rtd-theme
make html     # output in build/html/
```

The already generated version is in `docs/`: open `docs/index.html` in a browser.
