To create a virtual environment 

```
python -m venv <name_for_env>

# This creates a directory that sets up an empty virtualenv

# To activatge the venv in your current environment 
source <name_for_venv>/bin/activate 

# Go about your business with 
pip install <whatever>

# ... 

# Create a list of the current environment's packages for committing to a repo or sharing with your teacher or friends
pip freeze >> requirements.txt

# Finish up and exit with 
deactivate

```


