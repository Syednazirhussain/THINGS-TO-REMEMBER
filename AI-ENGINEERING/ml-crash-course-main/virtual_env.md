# 1. Go to project
cd /<project_dir>

# 2. Create environment
python3 -m venv .venv

# 3. Activate
source .venv/bin/activate

# 4. Upgrade pip
python -m pip install --upgrade pip

# 5. Install packages
python -m pip install numpy pandas

# 6. Save dependencies
python -m pip freeze > requirements.txt

# 7. Work on your project
python main.py

# 8. Leave environment
deactivate

# 9. Install Jupyter Notebook
python -m pip install notebook

# 10. Start Jupyter Notebook
jupyter notebook