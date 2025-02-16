
# **Batch Convert Chinese Text Files to UTF-8 with Simplified Chinese**  

## **Introduction**  
If you have a collection of Chinese text files in different encodings (e.g., **GB2312, Big5, Traditional Chinese**), converting them all to **UTF-8 with Simplified Chinese** can be a tedious task. This guide provides a **Python-based solution** to automate the conversion for an entire folder.  

---

## **Why Convert to UTF-8?**  
- Ensures compatibility across all platforms.  
- Prevents encoding-related errors in text editors and applications.  
- UTF-8 is the global standard for modern text processing.  

---

## **The Problem with Mixed Encodings**  
Many Chinese text files may use older encodings like **GB2312, Big5, or other ANSI-based formats**, which can cause:  
❌ **Unreadable characters (mojibake)** when opened in the wrong encoding.  
❌ **Incompatibility** with modern applications that expect UTF-8.  
❌ **Mixing of Traditional and Simplified Chinese**, making processing inconsistent.  

To solve this, we need a script that:  
✅ **Detects file encoding** automatically.  
✅ **Converts Traditional Chinese to Simplified Chinese**.  
✅ **Saves all files in UTF-8 format**.  

---

## **Python Script to Automate the Conversion**  

### **1️⃣ Install Required Libraries**  
Run the following command to install dependencies:  
```bash
pip install chardet opencc
```
- **`chardet`** → Detects file encoding.  
- **`opencc`** → Converts Traditional Chinese to Simplified Chinese.  

---

### **2️⃣ Python Script**  
Save the following script as `convert_utf8.py`:  
```python
import os
import chardet
from opencc import OpenCC

# Set the folder containing text files
folder_path = r"path/to/your/folder"  # Replace with your actual folder path

# Initialize converter (Traditional to Simplified Chinese)
cc = OpenCC('t2s')

def detect_encoding(file_path):
    """Detect file encoding."""
    with open(file_path, 'rb') as f:
        raw_data = f.read()
    result = chardet.detect(raw_data)
    return result['encoding']

def convert_file(file_path):
    """Convert file to UTF-8 with Simplified Chinese."""
    encoding = detect_encoding(file_path)
    
    if encoding is None:
        print(f"Skipping {file_path}, unable to detect encoding.")
        return
    
    with open(file_path, 'r', encoding=encoding, errors='ignore') as f:
        content = f.read()

    # Convert to Simplified Chinese
    simplified_content = cc.convert(content)

    # Save as UTF-8
    with open(file_path, 'w', encoding='utf-8') as f:
        f.write(simplified_content)
    
    print(f"Converted {file_path} from {encoding} to UTF-8 with Simplified Chinese.")

# Process all text files in the folder
for filename in os.listdir(folder_path):
    if filename.endswith(".txt"):
        convert_file(os.path.join(folder_path, filename))

print("✅ All files converted!")
```

---

## **3️⃣ Running the Script**
1. Place the script in the same folder as your text files or adjust `folder_path` to the correct location.  
2. Run the script using Python:  
   ```bash
   python convert_utf8.py
   ```
3. The script will scan all `.txt` files in the folder, detect their encoding, convert them to **UTF-8 + Simplified Chinese**, and overwrite the original files.  

---

## **Common Errors and Fixes**  

### **1️⃣ Unicode Escape Error (`\U` Issue)**  
**Error Message:**  
```
SyntaxError: (unicode error) 'unicodeescape' codec can't decode bytes in position 2-3
```
**Fix:** Use either of the following solutions:  
✔ Use a **raw string** by adding `r` before the path:  
```python
folder_path = r"path/to/your/folder"
```
✔ Use **double backslashes (`\\`)**:  
```python
folder_path = "C:\\path\\to\\your\\folder"
```

---

## **Conclusion**  
This script provides an efficient way to **batch convert** all Chinese text files in a folder to **UTF-8 with Simplified Chinese**, solving encoding issues and ensuring consistency.  

✅ **Automatic encoding detection**  
✅ **Traditional to Simplified conversion**  
✅ **Batch processing for an entire folder**  

Now, you can safely manage and use your text files without worrying about encoding errors! 🚀
