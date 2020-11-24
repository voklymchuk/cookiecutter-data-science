# GriP on Data Science

_Based on [Cookiecutter Data Science](https://github.com/drivendata/cookiecutter-data-science), this Data Science
project template aims to provide a more opinionated and firm framework,
especially tailored towards beginners._

## Quickstart

The following should get you set up quickly. For more details, [consult the documentation](https://wavefrontset.github.io/grip-on-data-science/#getting-started).

### Requirements to use the cookiecutter template:
-----------
 - Python>=3.6
 - [Cookiecutter Python package](http://cookiecutter.readthedocs.org/en/latest/installation.html) >= 1.4.0: This can be installed with pip by or conda depending on how you manage your Python packages:

``` bash
$ pip install cookiecutter
```

or

``` bash
$ conda config --add channels conda-forge
$ conda install cookiecutter
```


### To start a new project, run:
------------

    cookiecutter https://github.com/waveFrontSet/cookiecutter-data-science
    
### Create a new `conda` environment:
------------
    
    make create_environment

### Overview over the next steps
------------

After activating the `conda` environment, you are all set up. Here are the next
steps:

- Define how to obtain the raw data of your project and update the `data/raw`
  target in the `Makefile` accordingly.
- Define generic processing and clean up transformations in 
  `{{ cookiecutter.module_name }}/data/generic_processing.py` to produce interim data.
- Define project specific transformations to obtain the final data set in 
  `{{ cookiecutter.module_name }}/features/build_features.py`.
- Edit `{{ cookiecutter.module_name }}/models/model_config.py` to decide what
  models you want to build and what the target value of the prediction will be.
  Issuing `make train` will automatically split your dataset into a train and a
  test set and then fit the models on the train set.
- Edit `{{ cookiecutter.module_name }}/models/metric_config.py` to decide what
  metrics you want to use to evaluate the model performance. Issuing `make
  evaluate` will evaluate the models using the defined metrics on the test set.


### Installing development requirements
------------

    pip install -r requirements.txt

### Running the tests
------------

    pytest tests
