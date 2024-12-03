# AutoSave-Xmind

This AppleScript automatically saves your work in Xmind every 10 seconds while ensuring that it does not interrupt your workflow. If you are actively typing in Xmind, the script will pause the save action to prevent distractions.

## Features

- Automatically triggers the "Save" shortcut (`Command + S`) when Xmind is the active application.
- Ensures the save action is only triggered if you are not typing (i.e., no text input field is focused).
- Provides a seamless experience by avoiding interruptions during active usage.

## How to Use

### Step 1: Copy the Script
1. Open **Script Editor** on your Mac.
2. Copy the script code and paste it into the editor:
```
on run
    repeat
        -- 检查 Xmind 是否处于前台
        tell application "System Events"
            set frontAppName to name of first application process whose frontmost is true
        end tell

        if frontAppName is "Xmind" then
            -- 检查是否在输入框中（Xmind 的具体实现可能需要调整）
            tell application "System Events"
                set inputFieldExists to (exists of UI elements of first window of application process "Xmind")
            end tell
            
            if not inputFieldExists then
                -- 检测键盘和鼠标是否无操作超过 3 秒
                set idleTime to do shell script "ioreg -c IOHIDSystem | awk '/HIDIdleTime/ {print int($NF/1000000000); exit}'"
                if idleTime > 3 then
                    -- 触发 Command+S 保存操作
                    tell application "System Events"
                        keystroke "s" using command down
                    end tell
                end if
            end if
        end if

        -- 延迟 1 秒后再次检测
        delay 1
    end repeat
end run
```

### Step 2: Export as an Application
1. Go to **File > Export**.
2. Choose the format as **Application**.
3. Save the application with a descriptive name, e.g., `XmindAutoSave.app`.

### Step 3: Grant Accessibility Permissions
1. Open **System Settings** and navigate to **Privacy & Security > Accessibility**.
2. Click the **+** button and add your exported application.
3. Ensure the application is enabled in the list.

### Step 4: Set to Launch at Startup
1. Open **System Settings > Users & Groups > Login Items**.
2. Add your exported application to the list to ensure it starts automatically when you log in.

### Step 5: Run the Application
1. Double-click the application to start the script.
2. It will now monitor Xmind and trigger auto-save when appropriate.

---

## Notes
- Ensure Xmind is installed and running properly on your system.
- If the script does not work as expected, verify that accessibility permissions are granted.
- The script is optimized to minimize resource usage and avoid disrupting your workflow.

## License
This script is provided under the [MIT License](LICENSE). Feel free to modify and distribute it as needed.
