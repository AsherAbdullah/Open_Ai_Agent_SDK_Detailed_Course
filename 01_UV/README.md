# Lecture: UV Ko Install Aur Project Banane Ka Tareeqa (Urdu-English)

## Introduction: UV Kya Hai?
As-salamu Alaikum! Aaj hum baat karenge **UV** ke baare mein, jo ek bohat tezi se kaam karne wala Python package aur project manager hai. Yeh tool Rust programming language mein bana hai, aur yeh **pip**, **venv**, **poetry**, aur dosre tools ka ek all-in-one replacement hai. UV ke faide yeh hain:
- **Tez**: UV pip se 10-100 times faster hai.[](https://realpython.com/python-uv/)
- **Asaan**: Ek hi tool se aap Python versions, virtual environments, aur dependencies manage kar sakte hain.
- **Cross-Platform**: Windows, macOS, aur Linux sab pe kaam karta hai.

Agar aap Python projects banate hain, toh UV aapka kaam asaan aur tezi se karega. Chalen, shuru karte hain!

## Step 1: UV Ko Install Karna
Sabse pehle, hum UV ko apne system pe install **install** karenge. UV ko install karna bohat asaan hai, aur aap standalone installer ya PyPI ke zariye install kar sakte hain. Niche steps hain:

### Windows
1. **PowerShell** ya **Command Prompt** kholen.
2. Yeh command run karen:
   ```
   curl -sSL https://astral.sh/uv/install.sh | sh
   ```
   Yeh latest version download aur install karega.[](https://realpython.com/python-uv/)
3. Installation check karne ke liye, yeh command run karen:
   ```
   uv --version
   ```
   Agar version number dikhe (jaise 0.6.12), toh UV install ho gaya!

### macOS/Linux
1. **Terminal** kholen.
2. Yeh command run karen:
   ```
   curl -sSL https://astral.sh/uv/install.sh | sh
   ```
3. Version check karen:
   ```
   uv --version
   ```

### PyPI se Install (Optional)
Agar aapke paas Python aur pip pehle se installed hain, toh aap yeh bhi kar sakte hain:
```
pip install uv
```
Lekin standalone installer zyada recommended hai kyunke iske liye Python ki zarurat nahi.[](https://www.infoworld.com/article/2336295/how-to-use-uv-a-superfast-python-package-installer.html)

**Note**: Agar aap specific version chahte hain (jaise 0.6.12), toh URL mein version add karen:
```
curl -sSL https://astral.sh/uv/0.6.12/install.sh | sh
```

## Step 2: UV Ke Saath Project Banana
Ab hum ek naya Python project banayenge UV ke saath. Yeh steps follow karen:

1. **Project Folder Banayein**:
   Apne desired directory mein jayein aur ek folder banayein. Misal ke liye:
   ```
   mkdir mera-project
   cd mera-project
   ```

2. **Project Initialize Karen**:
   UV ke `init` command se project shuru karen:
   ```
   uv init mera-project
   ```
   Yeh command ek basic project structure banayega, jisme yeh files hongi:
   - `pyproject.toml`: Project metadata aur dependencies ke liye.
   - `main.py`: Ek sample Python script.
   - `.python-version`: Project ke liye Python version specify karta hai.
   - `README.md`: Project description ke liye.[](https://docs.astral.sh/uv/guides/projects/)

3. **Virtual Environment Banayein**:
   UV automatically virtual environment banata hai jab aap pehli baar project command chalate hain (jaise `uv run` ya `uv sync`). Virtual environment dependencies ko isolate rakhta hai.

## Step 3: Dependencies Manage Karna
UV ke saath dependencies add aur remove karna bohat asaan hai. Chalen, dekhte hain kaise:

### Dependency Add Karna
Maan lo, aapko `requests` library chahiye. Yeh command run karen:
```
uv add requests
```
Yeh:
- `pyproject.toml` mein `requests` add karega.
- `uv.lock` file update karega (exact versions ke liye).
- Virtual environment mein `requests` install karega.[](https://github.com/astral-sh/uv)

Agar aap specific version chahte hain, toh yeh karen:
```
uv add requests==2.31.0
```

### Dependency Remove Karna
Agar aap `requests` hataana chahte hain, toh:
```
uv remove requests
```
Yeh `pyproject.toml` aur `uv.lock` se dependency hata dega.

### Dependencies Sync Karna
Agar aapke paas `pyproject.toml` ya `uv.lock` hai aur aap virtual environment ko sync karna chahte hain, toh:
```
uv sync
```
Yeh ensure karta hai ke virtual environment mein wahi dependencies hain jo `uv.lock` mein hain.[](https://github.com/astral-sh/uv)

## Step 4: Scripts Chalana
UV ke saath aap apne Python scripts asani se chala sakte hain using `uv run`. Yeh virtual environment ke saath script execute karta hai.

Misal ke liye, agar `main.py` mein yeh code hai:
```python
print("Hello, World!")
```
Toh isse chalane ke liye:
```
uv run python main.py
```
Yeh "Hello, World!" print karega.

Agar aap direct script chalana chahte hain, toh:
```
uv run main.py
```

## Practical Examples
Ab hum do practical examples dekhte hain UV ke saath projects banane ke.

### Example 1: Simple Hello World
1. Ek naya project banayein:
   ```
   uv init hello-world
   cd hello-world
   ```
2. `main.py` mein yeh code likhein:
   ```python
   print("As-salamu Alaikum, Dunya!")
   ```
3. Script chalayein:
   ```
   uv run main.py
   ```
   **Output**: As-salamu Alaikum, Dunya!

### Example 2: API Call with Requests
1. Ek naya project banayein:
   ```
   uv init api-project
   cd api-project
   ```
2. `requests` library add karen:
   ```
   uv add requests
   ```
3. `main.py` mein yeh code likhein:
   ```python
   import requests

   response = requests.get("https://api.github.com")
   print(response.json())
   ```
4. Script chalayein:
   ```
   uv run main.py
   ```
   **Output**: GitHub API ka JSON response dikhega.

## Conclusion
UV ek zabardast tool hai Python projects ke liye, jo tezi aur asani se kaam karta hai. Is lecture mein humne dekha ke UV ko kaise install karte hain, project kaise banate hain, dependencies kaise manage karte hain, aur scripts kaise chalate hain. Ab aap apne projects shuru kar sakte hain!

**Practice Tip**: Ek chhota project banayein, jaise ek weather API se data fetch karne wala script, aur UV ke commands try karen. Agar koi sawal ho, toh documentation dekhein: [docs.astral.sh/uv](https://docs.astral.sh/uv).[](https://github.com/astral-sh/uv)

Shukriya aur Allah Hafiz!
