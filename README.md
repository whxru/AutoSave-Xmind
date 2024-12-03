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
```applescript
on run
	repeat
		try
			tell application "System Events"
				-- 获取当前前台应用的名称
				set activeApp to name of first application process whose frontmost is true
				
				-- 检查是否是 Xmind
				if activeApp = "Xmind" then
					-- 尝试获取当前窗口的焦点元素
					set focusedElement to missing value
					try
						set focusedElement to value of attribute "AXFocusedUIElement" of application process "Xmind"
					end try
					
					-- 如果成功获取焦点元素，检查其角色
					if focusedElement is not missing value then
						set focusedRole to role of focusedElement
						
						-- 检查是否不是文本输入区域
						if focusedRole ≠ "AXTextField" and focusedRole ≠ "AXTextArea" then
							keystroke "s" using {command down} -- 发送保存快捷键
						end if
					else
						-- 如果没有焦点元素，认为没有输入，执行保存
						keystroke "s" using {command down}
					end if
				end if
			end tell
		on error errMsg
			-- 捕获和记录错误信息（可选）
			display dialog "Error: " & errMsg giving up after 5
		end try
		
		delay 10
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
