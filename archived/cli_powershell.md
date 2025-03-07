### **Creating a New File**

```powershell
New-Item -Path "<file_name>" -ItemType File
```

This command creates an empty file with the specified name.

### **Creating or Deleting a Directory**

- **Create a Directory:**
```powershell
New-Item -Path "<dir_name>" -ItemType Directory
```

- **Delete a Directory:**
```powershell
Remove-Item -Path "<dir_name>" -Recurse
```

This command deletes the specified directory and its contents recursively.

### **Opening, Renaming, Copying, or Moving a File**

- **Open a File:**
```powershell
notepad "<file_name>"
```

- **Rename a File:**
```powershell
Rename-Item -Path "<file_name>" -NewName "<new_file_name>"
```

- **Copy a File:**
```powershell
Copy-Item -Path "<file_name>" -Destination "<destination_path>"
```

- **Move a File:**
```powershell
Move-Item -Path "<file_name>" -Destination "<destination_path>"
```

### **Redirecting Streams**

- **Overwrite File Content:**
```powershell
Set-Content -Path "<file_name>" -Value "<some_text>"
```

- **Append to File Content:**
```powershell
Add-Content -Path "<file_name>" -Value "<some_text>"
```

### **Environment Variables**

- **Set a User Environment Variable:**
```powershell
[System.Environment]::SetEnvironmentVariable("<ENV_VAR>", "<value>", "User")
```

- **Set a System Environment Variable:**
```powershell
[System.Environment]::SetEnvironmentVariable("<ENV_VAR>", "<value>", "Machine")
```

> Note: Modifying system environment variables may require administrative privileges.

### **Additional Important Commands**

- **List Directory Contents:**
```powershell
Get-ChildItem -Path "<dir_name>"
```

- **Remove a File:**
```powershell
Remove-Item -Path "<file_name>"
```

- **Create a Symbolic Link:**
```powershell
New-Item -ItemType SymbolicLink -Path "<link_path>" -Target "<target_path>"
```

- **Check if a File or Directory Exists:**
```powershell
Test-Path -Path "<path>"
```