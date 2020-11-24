# GriP on Data Science

_Based on [Cookiecutter Data Science](https://github.com/drivendata/cookiecutter-data-science), this Data Science
project template aims to provide a more opinionated and firm framework,
especially tailored towards beginners._

## Why use this project structure?

While the [Cookiecutter Data
Science](https://github.com/drivendata/cookiecutter-data-science) template aims
at being a great starting point, it leaves you with a lot of design choices:

- What kind of `make` targets should you provide?
- How to organize training and evaluating models?

Certainly, one could argue that the answer to these questions depends on the
concrete project at hand. However, I think that beginning Data Scientists
especially (or rather beginners in any field, really) need a firm and more
refined framework to start their journey. To be honest, when I found out about the Data Science
cookiecutter, I was overwhelmed by the potential and the choices I had to make
when starting my first Data Science project based on the template.

Furthermore, there are a lot of [forks of the Cookiecutter Data Science
template](https://github.com/drivendata/cookiecutter-data-science/network/members),
a lot of which aim at providing more orientation. The main example I was
inspired by is [Cookiecutter
EasyData](https://github.com/hackalog/cookiecutter-easydata) that provides a
plethora of additional make targets, is able to manage dependencies via `conda`,
and adds a lot of boilerplate code assisting in building up a Data Science
project.

While the aforementioned template tackles a lot of the weaknesses of the
Cookiecutter Data Science template, I find that it is too sophisticated and not
well-documented enough to be easy to use (even though the maintainers started an
[example project](https://github.com/hackalog/bus_number) explaining their
template structure and how to use it). Thus, I decided to take the middle
ground between the two.

### Start with 100% test coverage

Depending on the amount of data, transformations may take quite some time and
should be tested before they run on the whole dataset -- it's aggravating when a
ten minute transformation yields the wrong result because you applied it to the
wrong column.

This project provides you with tests that illustrate how your command line
scripts that apply the transformation can be tested.

### Be as elaborate as you want with your log messages

With the logging configuration that comes with this project, all `INFO` level
messages will be emitted to the console. If you need more detailed information
but do not want to pollute the console, use the `DEBUG` level that will be
automatically logged into log files in the `log` folder.

### Code style: black

The initial code and tests are formatted by `black`.

### Use pre-defined `make` targets for common tasks

There are pre-defined `make` targets that 

- help you define your data transformation pipeline until you arrive at your
final dataset (`make data/raw`, `make data/interim`, `make
data/processed/dataset.csv`) and
- accelerate choosing and evaluating Machine Learning models (`make train` and
`make evaluate`).

The latter are configured via simple dictionaries. Details below. 

## Getting started

### Requirements

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

### Starting a new project

Starting a new project is as easy as running this command at the command line. No need to create a directory first, the cookiecutter will do it for you.

```nohighlight
cookiecutter https://github.com/waveFrontSet/cookiecutter-data-science
```

-------------
#### Create a new environment

After the new project has been created you should create a new `conda` environment
using

```nohighlight
make create_environment
```

Activate it using `conda activate project_name`.

-------------
#### Implement the `data/raw` target

The `data/raw` target is the specification of how to obtain the raw data of your
project. At the beginning, it is left empty. Decide what you need to do to
obtain the raw data and decide how to save it. If necessary, write a script next
to the `generic_processing.py` script.

-------------
#### Implement generic clean ups and transformations

The `data/interim` target is for generic clean ups and transformations
(examples: Removing unnecessary columns, removing duplicated rows). It aims at
the `generic_processing.py` script. Edit it according to your needs.

-------------
#### Implement transformations and aggregations resulting in your final dataset

The `data/processed/dataset.csv` target is the road to your final dataset. This
dataset will be used to train a model. The target aims at the
`build_features.py` script. Edit it according to your needs.

-------------
#### Decide which models to train

Models are trained via the `train` target. Edit the `MODELS` dictionary in the
`model_config.py` file to configure which models to train. The `TARGET_VALUE`
constant should contain the name of the column that is the prediction target of
your models.

*Example*: If you want to fit a Linear Regression model as well as
a Decision Tree with standard hyperparameters, use

```python
from sklearn.linear_model import LinearRegression
from sklearn.tree import DecisionTreeRegressor

MODELS = {
    "linearRegression": LinearRegression(),
    "decisionTree": DecisionTreeRegressor(),
}
```

Issuing `make train` will fit both models to the train set and dump the
resulting models into the folder `models/`. The filename of each model will be
its key.

Note that this target automatically invokes the `train_test_split` target that
splits `data/processed/dataset.csv` into a train set `train.csv` and a test set
`test.csv`.

-------------
#### Evaluate your models

Models are evaluated via the `evaluate` target. Edit the `METRICS` dictionary in
the `metric_config.py` file to configure which metrics to evaluate for each model.

*Example*: If you want to evaluate both models from the above paragraph using
R2-Score and mean absolute error, use

```python
from sklearn.metrics import r2_score, mean_absolute_error

METRICS = {
    "linearRegression": [r2_score, mean_absolute_error],
    "decisionTree": [r2_score, mean_absolute_error],
}
```

Issuing `make evaluate` will deserialize both models and evaluate them using the
defined metrics on the test set. The results are printed to the console and,
additionally, will be written into the file `models/report.json`.

## Directory structure

The majority is the same as in the basic Cookiecutter Data Science template.
Added are the `tests` folder as well as the `model_config.py` and the
`metric_config.py`. Plus, `make_dataset.py` was renamed to
`generic_processing.py`.

```nohighlight
├── LICENSE
├── Makefile           <- Makefile with commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default Sphinx project; see sphinx-doc.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.py           <- Make this project pip installable with `pip install -e`
├── {{ cookiecutter.module_name }}                <- Source code for use in this project.
│   ├── __init__.py    <- Makes {{ cookiecutter.module_name }} a Python module
│   │
│   ├── data           <- Scripts to download or generate data and for generic transformations
│   │   └── generic_processing.py
│   │
│   ├── features       <- Scripts to turn raw data into features for modeling
│   │   └── build_features.py
│   │
│   ├── models         <- Scripts to train models and then use trained models to make
│   │   │                 predictions
│   │   ├── metric_config.py <- Contains the dictionary that configures the
│   │   │                       metrics used to evaluate your models
│   │   ├── model_config.py <- Contains the dictionary that configures the
│   │   │                       models to train and the target value
│   │   ├── predict_model.py
│   │   └── train_model.py
│   │
│   └── visualization  <- Scripts to create exploratory and results oriented visualizations
│       └── visualize.py
├── tests              <- Folder with initial tests for 100% coverage
└── tox.ini            <- tox file with settings for running tox; see tox.testrun.org
```

### Keep secrets and configuration out of version control

You _really_ don't want to leak your AWS secret key or Postgres username and password on Github. Enough said — see the [Twelve Factor App](http://12factor.net/config) principles on this point. Here's one way to do this:

#### Store your secrets and config variables in a special file

Create a `.env` file in the project root folder. Thanks to the `.gitignore`, this file should never get committed into the version control repository. Here's an example:

```nohighlight
# example .env file
DATABASE_URL=postgres://username:password@localhost:5432/dbname
AWS_ACCESS_KEY=myaccesskey
AWS_SECRET_ACCESS_KEY=mysecretkey
OTHER_VARIABLE=something
```

#### Use a package to load these variables automatically.

If you look at the stub script in `{{ cookiecutter.module_name }}/data/make_dataset.py`, it uses a package called [python-dotenv](https://github.com/theskumar/python-dotenv) to load up all the entries in this file as environment variables so they are accessible with `os.environ.get`. Here's an example snippet adapted from the `python-dotenv` documentation:

```python
# {{ cookiecutter.module_name }}/data/dotenv_example.py
import os
from dotenv import load_dotenv, find_dotenv

# find .env automagically by walking up directories until it's found
dotenv_path = find_dotenv()

# load up the entries as environment variables
load_dotenv(dotenv_path)

database_url = os.environ.get("DATABASE_URL")
other_variable = os.environ.get("OTHER_VARIABLE")
```

#### AWS CLI configuration
When using Amazon S3 to store data, a simple method of managing AWS access is to set your access keys to environment variables. However, managing mutiple sets of keys on a single machine (e.g. when working on multiple projects) it is best to use a [credentials file](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html), typically located in `~/.aws/credentials`. A typical file might look like:
```
[default]
aws_access_key_id=myaccesskey
aws_secret_access_key=mysecretkey

[another_project]
aws_access_key_id=myprojectaccesskey
aws_secret_access_key=myprojectsecretkey
```
You can add the profile name when initialising a project; assuming no applicable environment variables are set, the profile credentials will be used be default.
