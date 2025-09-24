
# Automated Notepad Data Entry Bot

This project automates data entry into Windows Notepad using the JSONPlaceholder API:

* Fetches the first 10 blog posts from [JSONPlaceholder](https://jsonplaceholder.typicode.com/)
* Opens Notepad for each post individually
* Types formatted blog content (title + body) automatically
* Saves each post to `Desktop/tjm-project/posts/post_<id>.txt`
* Closes Notepad after every post with proper cleanup
* Includes extensive error handling for network, process, focus, file-system, and user-interrupt issues

## Features

- **Smart Content Formatting**: Each post includes proper title, content structure, and metadata
- **Robust Process Management**: Uses `taskkill` for reliable Notepad cleanup
- **Focus Verification**: Confirms Notepad is active before typing to prevent accidents  
- **Failsafe Protection**: Move mouse to screen corner to abort operation instantly
- **Direct File Saving**: Saves files via Python (no GUI save dialog automation)
- **Professional Output**: Creates numbered files with clean formatting

## Error Handling & Edge Cases

The bot includes defensive code for common failure scenarios:

### Process-launch issues (Notepad missing)
- If Notepad is not installed or not on the PATH, launching with  
  `subprocess.Popen(["notepad"])` raises **FileNotFoundError**
- The script catches this and exits with a clear, user-friendly message

### Focus / UI automation failures
- **Window focus check:** Uses **pygetwindow** to confirm the active window title contains "Notepad" before typing
- **User abort:** Moving the mouse to any screen corner during typing triggers PyAutoGUI's **FailSafeException**  
  The script catches it, logs a message, and closes Notepad safely

### Network & API failures
- **Connection timeout:** 10-second timeout for API requests with proper error handling
- **API errors:** Graceful handling of HTTP errors and malformed responses
- **No internet:** Continues gracefully if API is unreachable

### File-system errors
- Handles **OSError** when writing output (e.g., Desktop is read-only, disk full, or permission denied)
- Creates output directory automatically if it doesn't exist
- Reports problems and cleans up without leaving stray Notepad windows

### Environment / platform checks
- Verifies the OS is Windows (`os.name == "nt"`) before starting, because the automation depends on `notepad.exe` and `taskkill`
- Detects missing Python dependencies at startup and prints instructions to install them

### Graceful interruption (Ctrl+C)
- A top-level `try/except KeyboardInterrupt` ensures that if the user stops the program with **Ctrl+C**,  
  all Notepad windows opened by the script are force-closed before exiting

---

These safeguards mean the application **fails safely** and leaves the system clean even when something goes wrong.

## Requirements

- **Windows OS** (Windows 10/11 recommended)
- **Python 3.6+** (for running from source)
- **Internet connection** (for fetching blog posts)

### Python Dependencies
```
requests>=2.25.1
pyautogui>=0.9.54
pygetwindow>=0.0.9
```

## Run the Standalone App (No Python Required)

1. Download the latest `NotePadAutomationBot.exe` from the [Releases](https://github.com/YOUR_USERNAME/notepad-automation-bot/releases) page
2. Double-click the `.exe` file on Windows 10/11
3. The application will automatically:
   - Create output folder on Desktop
   - Fetch blog posts from API
   - Process each post through Notepad
   - Display progress and completion status

## Run From Source (Optional)

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/notepad-automation-bot.git
cd notepad-automation-bot

# Install dependencies
pip install -r requirements.txt

# Run the script
python main.py
```

## Usage

1. **Before running**: Close any open Notepad windows
2. **Start the application**: Run the executable or Python script
3. **Don't interfere**: Let the bot work automatically (don't click or type)
4. **Emergency stop**: Move mouse to any screen corner to abort
5. **Check results**: Find generated files in `Desktop/tjm-project/posts/`

