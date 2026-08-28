# Blockchain Course Project 🪙

A simple blockchain built from scratch with Python and Flask, exposed as a REST API.
Mine blocks, view the full chain, and verify that the chain is valid.

---

## 📁 Project Structure

```
blockchain-course/
├── env/              # Virtual environment (NOT pushed to GitHub - see .gitignore)
├── blockchain.py     # The Blockchain class + Flask API routes
├── giftcoin.py       # Main entry point (imports from blockchain.py) - RUN THIS
├── requirements.txt  # List of packages needed to run this project
├── .gitignore        # Keeps env/, __pycache__/ etc. out of GitHub
└── README.md         # This file
```

> ⚠️ **Important:** The `env/` folder will NOT be pushed to GitHub (it should be listed
> in `.gitignore`). Anyone cloning this repo — including future you — must recreate the
> virtual environment. Follow the steps in the next section.

---

## 🚀 How to Set Up the Project From Scratch

These are the steps to run this project on a fresh machine (or after re-cloning the repo,
since the `env/` folder won't exist).

### Step 1: Clone the repository

```bash
git clone https://github.com/gift56/how-to-create-a-blockchain.git
cd how-to-create-a-blockchain
```

### Step 2: Create a new virtual environment

A virtual environment is an isolated, project-specific folder of installed packages.
It keeps this project's dependencies separate from your global Python installation.

```bash
# Windows (cmd.exe or PowerShell)
python -m venv env

# macOS / Linux
python3 -m venv env
```

This creates a folder named `env/` containing its own Python interpreter and pip.

> 💡 You can name the folder anything (`venv`, `.venv`, etc.), but if you rename it,
> update `.gitignore` and any commands below to match. `env` is what this project uses.

### Step 3: Activate the virtual environment

You must activate it **every time** you open a new terminal before running or
installing anything:

```bash
# Windows - PowerShell
env\Scripts\Activate.ps1

# Windows - cmd.exe
env\Scripts\activate.bat

# Windows - Git Bash
source env/Scripts/activate

# macOS / Linux
source env/bin/activate
```

✅ You'll know it worked when you see `(env)` at the beginning of your terminal prompt:

```
(env) C:\Users\DELL\Desktop\blockchain-course>
```

To leave the virtual environment later, run:

```bash
deactivate
```

> **PowerShell error?** If you get "running scripts is disabled", run this once:
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```

### Step 4: Install the dependencies

With the venv **activated**, install everything from `requirements.txt`:

```bash
pip install -r requirements.txt
```

That single command installs Flask, requests, and everything else the project needs
into the `env/` environment.

To install a single new package instead (e.g. requests):

```bash
pip install requests
```

And if you install a new package, keep `requirements.txt` up to date:

```bash
pip freeze > requirements.txt
```

### Step 5: Run the app

```bash
python giftcoin.py
```

`giftcoin.py` is the main entry point — it imports the `Blockchain` class and Flask app
from `blockchain.py` and starts the server.

The Flask server starts at **http://127.0.0.1:5000** (it also listens on your network
at `0.0.0.0:5000`, so other devices on your Wi-Fi can reach it via your local IP).

Press `Ctrl+C` to stop the server.

---

## 🌐 API Endpoints

| Method | Endpoint     | Description                              |
|--------|--------------|------------------------------------------|
| GET    | `/`          | Home — confirms the node is running      |
| GET    | `/mine_block`| Mines a new block (takes a few seconds)  |
| GET    | `/get_chain` | Returns the full blockchain and its size |
| GET    | `/is_valid`  | Checks whether the chain is valid        |

### Example usage

Open these in your browser, or use `curl` / a Python script with the `requests` library:

```bash
# Mine a block
curl http://127.0.0.1:5000/mine_block

# View the whole chain
curl http://127.0.0.1:5000/get_chain

# Validate the chain
curl http://127.0.0.1:5000/is_valid
```

Example Python script using `requests` (this is why the `requests` package is installed):

```python
import requests

response = requests.get('http://127.0.0.1:5000/mine_block')
print(response.json())
```

---

## ⛏️ How the Blockchain Works

Each **block** contains:

- `index` — its position in the chain (1, 2, 3, ...)
- `timestamp` — when it was created
- `proof` — the number found by proof-of-work
- `previous_hash` — the SHA-256 hash of the previous block (this is what "chains" the blocks together)

**Proof of work:** the `proof_of_work` method searches for a number `new_proof` such that
`sha256(new_proof² - previous_proof²)` starts with four zeros (`0000`). Finding it takes
real computation, which makes tampering with the chain expensive.

**Validation:** `is_chain_valid` re-checks every block — that its `previous_hash` matches
the real hash of the block before it, and that its proof genuinely solved the puzzle.

---

## 📦 requirements.txt

`requirements.txt` pins the exact packages the project needs. It exists so the environment
can be rebuilt anywhere with one command (`pip install -r requirements.txt`).
Regenerate it after adding packages with `pip freeze > requirements.txt`.

---

## 🧰 Useful Commands Cheat Sheet

```bash
python -m venv env                  # create virtual environment
env\Scripts\activate                # activate it (Windows cmd)
deactivate                          # exit the virtual environment
pip install -r requirements.txt     # install all dependencies
pip install <package>               # install one package
pip freeze > requirements.txt       # save installed packages to file
python giftcoin.py                  # run the app
```